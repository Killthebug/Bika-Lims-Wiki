Tested with Ubuntu and Debian.

We want:

- nginx virtualhost ln port 80 for test.com
- ssl config
- varnish http cache and load balancer
- zeoclients
- regular buildout stuff

Install Plone, then `sudo apt-get install varnish nginx`.

edit the following:

    /etc/nginx/sites-enabled/test.com.conf
    /etc/nginx/proxy_params
    /etc/nginx/ssl/test.com/nginx.conf
    /etc/varnish/default.vcl
    /etc/default/varnish
    /etc/supervisor/conf.d/plone.conf

/etc/nginx/sites-enabled/test.com.conf
----------------------

    upstream plone {
        # varnish port, or port of zeoclient
        server localhost:8090;
    }
    # http
    server {
        listen 80 default;
        server_name test.com;
        access_log /var/log/nginx/test.com_access.log;
        rewrite ^(.*)(/login_|/require_login|/failsafe_login_form)(.*) https://$server_name$1$2$3 redirect;
        # logged in?  https only.
        if ($http_cookie ~* "__ac=([^;]+)(?:;|$)" ) {
            rewrite ^(.*) https://$host/$1 redirect;
        }
        location / {
            include /etc/nginx/proxy_params;
            proxy_pass http://plone_backend/VirtualHostBase/http/test.com:80/Plone/VirtualHostRoot/;
            # if there is no Plone site yet, use this
            # proxy_pass http://plone_backend/VirtualHostBase/http/test.com:80/VirtualHostRoot/;
        }
    }
    # https
    server {
        listen 443 ssl;
        server_name test.com;
        include /etc/nginx/ssl/test.com/nginx.conf;
        access_log /var/log/nginx/test.com_access.log;
        if ($http_cookie ~* "__ac=([^;]+)(?:;|$)" ) {
            # prevent infinite recursions between http and https
            break;
        }
        rewrite ^(.*)(/logged_out)(.*) http://$server_name$1$2$3 redirect;
        location / {
            include /etc/nginx/proxy_params;
            proxy_pass http://plone_backend/VirtualHostBase/https/test.com:443/Plone/VirtualHostRoot/;
            # if there is no Plone site yet, use this
            proxy_pass http://plone_backend/VirtualHostBase/https/test.com:443/VirtualHostRoot/;

        }
    }

If you have a verified SSL certificate (non-self-signed) for test.com, you can either use the following:

    # Varnish
    upstream plone {
        server localhost:8090;
    }
   
    # http
    server {
        listen 80 default;
    
        # ALWAYS redirect to https!
        rewrite ^(.*) https://$host/$1 redirect;
    }
    
    # https
    server {
        listen 443 ssl;
    
        ssl_certificate      /etc/nginx/ssl/test.com.crt;
        ssl_certificate_key  /etc/nginx/ssl/test.com.key;
    
        location / {
            proxy_pass http://plone_backend/VirtualHostBase/https/test.com:443/Plone/VirtualHostRoot/;
            # if there is no Plone site yet, use this
            proxy_pass http://plone_backend/VirtualHostBase/https/test.com:443/VirtualHostRoot/;
            include /etc/nginx/proxy_params;
        }
    }

/etc/nginx/proxy_params
----------------------

    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 100M;
    client_body_buffer_size 1m;
    proxy_intercept_errors on;
    proxy_buffering on;
    proxy_buffer_size 128k;
    proxy_buffers 256 16k;
    proxy_busy_buffers_size 256k;
    proxy_temp_file_write_size 256k;
    proxy_max_temp_file_size 0;
    proxy_read_timeout 600;

/etc/nginx/ssl/test.com/nginx.conf
----------------------

    ssl_certificate /etc/nginx/ssl/test.com/crt;
    ssl_certificate_key /etc/nginx/ssl/test.com/key;
    ssl_session_timeout 10m;
    ssl_protocols SSLv3 TLSv1;
    ssl_ciphers ALL:!ADH:!EXPORT56:RC4+RSA:+HIGH:+MEDIUM:+LOW:+SSLv3:+EXP;
    ssl_prefer_server_ciphers on;

/etc/varnish/default.vcl
----------------------

    backend zeoclient_1 {
        .host = "127.0.0.1";
        .port = "8085";
        .connect_timeout = 10s;
        .first_byte_timeout = 600s;
        .between_bytes_timeout = 600s;
        .max_connections = 30;
        .probe = {
          .url = "/";
          .interval = 3s;
          .timeout = 3s;
          .window = 5;
          .threshold = 2;
          .initial = 1;
        }
    }
    backend zeoclient_2 {
        .host = "127.0.0.1";
        .port = "8086";
        .connect_timeout = 10s;
        .first_byte_timeout = 600s;
        .between_bytes_timeout = 600s;
        .max_connections = 30;
        .probe = {
          .url = "/";
          .interval = 3s;
          .timeout = 3s;
          .window = 5;
          .threshold = 2;
          .initial = 1;
        }
    }
    backend zeoclient_3 {
        .host = "127.0.0.1";
        .port = "8087";
        .connect_timeout = 10s;
        .first_byte_timeout = 600s;
        .between_bytes_timeout = 600s;
        .max_connections = 30;
        .probe = {
          .url = "/";
          .interval = 3s;
          .timeout = 3s;
          .window = 5;
          .threshold = 2;
          .initial = 1;
        }
    }
    director zeoclient_director round-robin {
            { .backend = zeoclient_1; }
            { .backend = zeoclient_2; }
            { .backend = zeoclient_3; }
    }
    acl purge {
        "localhost";
        "127.0.0.1";
    }
    sub vcl_recv {
        set req.grace = 120s;
        if (req.http.host ~ "^test.com(:[0-9]+)?$") {
            set req.backend = zeoclient_director;
        }
        /*** everything else goes to ... ***/
        // else {
        //     set req.backend = backend_plone;
        // }
        if (req.request == "PURGE") {
            if (!client.ip ~ purge) {
                error 405 "Not allowed.";
            }
            return(lookup);
        }
        if (req.request != "GET" &&
            req.request != "HEAD" &&
            req.request != "PUT" &&
            req.request != "POST" &&
            req.request != "TRACE" &&
            req.request != "OPTIONS" &&
            req.request != "DELETE") {
            /* Non-RFC2616 or CONNECT which is weird. */
            return(pipe);
        }
        if (req.request != "GET" && req.request != "HEAD") {
            /* We only deal with GET and HEAD by default */
            return(pass);
        }
        if (req.http.If-None-Match) {
            return(pass);
        }
        if (req.url ~ "createObject") {
            return(pass);
        }
        remove req.http.Accept-Encoding;
        return(lookup);
    }
    sub vcl_hit {
            if (req.request == "PURGE") {
                    purge;
                    error 200 "Purged.";
            }
    }
    sub vcl_miss {
            if (req.request == "PURGE") {
                    purge;
                    error 200 "Purged.";
            }
    }

/etc/default/varnish
----------------------

    START=yes
    NFILES=131072
    MEMLOCK=82000
    INSTANCE=$(uname -n)
    VARNISH_VCL_CONF=/etc/varnish/default.vcl
    VARNISH_LISTEN_ADDRESS=
    VARNISH_LISTEN_PORT=8090
    VARNISH_ADMIN_LISTEN_ADDRESS=127.0.0.1
    VARNISH_ADMIN_LISTEN_PORT=8089
    VARNISH_MIN_THREADS=1
    VARNISH_MAX_THREADS=1000
    VARNISH_THREAD_TIMEOUT=120
    VARNISH_STORAGE_FILE=/var/lib/varnish/$INSTANCE/varnish_storage.bin
    VARNISH_STORAGE_SIZE=1G
    VARNISH_SECRET_FILE=/etc/varnish/secret
    VARNISH_STORAGE="file,${VARNISH_STORAGE_FILE},${VARNISH_STORAGE_SIZE}"
    VARNISH_TTL=120
    DAEMON_OPTS="-a ${VARNISH_LISTEN_ADDRESS}:${VARNISH_LISTEN_PORT} \
                 -f ${VARNISH_VCL_CONF} \
                 -T ${VARNISH_ADMIN_LISTEN_ADDRESS}:${VARNISH_ADMIN_LISTEN_PORT} \
                 -t ${VARNISH_TTL} \
                 -w ${VARNISH_MIN_THREADS},${VARNISH_MAX_THREADS},${VARNISH_THREAD_TIMEOUT} \
                 -S ${VARNISH_SECRET_FILE} \
                 -s ${VARNISH_STORAGE}"

/etc/supervisor/conf.d/plone.conf
----------------------

    [program:zeoserver]
    directory=/home/plone/test.com/zeocluster
    command=/home/plone/test.com/zeocluster/bin/zeoserver fg
    user=plone_daemon
    redirect_stderr=true
    stopwaitsecs=20

    [program:client1]
    directory=/home/plone/test.com/zeocluster
    command=/home/plone/test.com/zeocluster/bin/client1 console
    user=plone_daemon
    redirect_stderr=true
    stopwaitsecs=20

    [program:client2]
    directory=/home/plone/test.com/zeocluster
    command=/home/plone/test.com/zeocluster/bin/client2 console
    user=plone_daemon
    redirect_stderr=true
    stopwaitsecs=20

    [program:client3]
    directory=/home/plone/test.com/zeocluster
    command=/home/plone/test.com/zeocluster/bin/client3 console
    user=plone_daemon
    redirect_stderr=true
    stopwaitsecs=20

    [eventlistener:memmon]
    command=memmon -p client1=700MB -p client2=700MB
    events=TICK_60

And now
-------

    sudo supervisorctl restart client2 stop

or for a debug client

    sudo supervisorctl stop client1
    cd /home/plone/test.com/zeocluster
    bin/plonectl client2 debug
    sudo supervisorctl start client1





