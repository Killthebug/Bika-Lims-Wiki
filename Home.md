# Installing Bika LIMS 3

The following describes how to install Bika LIMS version 3 on a Unix server.

## 1. Get the latest Plone 4

Obtain the latest version of the Plone Unified Installer from
http://plone.org/products/plone/releases 

## 2. Copy link, and download the file via wget:

    wget http://launchpad.net/plone/4.1/4.1.2/+download/Plone-4.1.2-UnifiedInstaller.tgz

## 3. Expand the archive

    tar xzf Plone-4.1.2-UnifiedInstaller.tgz

## 4. Run installer.sh and point to new target directory

    sudo ./install.sh --target=/home/example standalone

## 5. Set up instance name on DNS (optional)

While the installer is running, set up a name for the Bika Plone instance
 and add the apache webserver mapping on server,
noting the new port used for the Zope instance.

Edit the apache configuration and add a new virtual host
section 

    sudo vim /etc/apache2/sites-enabled/000-default

Add a Virtualhost section as below, ensuring an existing used port is not used again:
 
    <VirtualHost *:80>
        ServerName example.bikalabs.com
        ServerAdmin webmaster@bikalabs.com
        ErrorLog /var/log/apache2/example.bikalabs.com.error.log
        LogLevel warn
        CustomLog /var/log/apache2/example.bikalabs.com.access.log combined
        RewriteEngine On
        RewriteRule ^/manage(.*) http://localhost:8030/VirtualHostBase/http/example.bikalabs.com:80/VirtualHostRoot/manage [L,P]
        RewriteRule ^/(.*) http://localhost:8030/VirtualHostBase/http/example.bikalabs.com:80/VirtualHostRoot/ [L,P]
    </VirtualHost>


## 6. Verify successfull Plone installation

Note the output of the installer script:

    ###################### Installation Complete ######################
 
    Plone successfully installed at /home/example
    See /home/example/zinstance/README.html
    for startup instructions
 
     Use the account information below to log into the Zope Management Interface
     The account has full 'Manager' privileges.
  
     Username: admin
     Password: password

The Plone installation can now be tested independently of the Bika build by starting it
 and adding a Plone site. Refer to [Plone.org](http://plone.org) if the build fails.

## 7. Add Bika LIMS to buildout

Edit /home/example/zinstance/buildout.cfg and make the changes to the 
eggs, develop, and instance environment variable sections, and 
optional changes to the http-address and effective-user.

### a. Find the eggs section. Add "bika.lims" to eggs:

    eggs =
        Plone
        Pillow
        lxml
        bika.lims

### b. Find the "develop" section. Add src/bika3

    develop =
        src/bika3


### c. Add the environment variable for ID-server

Add an [instance] section, containing the environment
variable matching the port number 
with that in the id-server shell script:

    [instance]
         environment-vars = IDServerURL http://localhost:8031


### d. Match Plone port to webserver rewrite rule

If remapping is required, note the port used for the Zope instance
should be unique (default is 8080). The server will not start if the
port is already held by another process.

    http-address = 8030

### e. Change the effective user if not "plone"

If a different user will be used to run the instance, replace "effective-user = plone" 
section with the new username. 

## 8. Check out the Bika LIMS bika3 code:

The Bika code is available from Github and from SourceForge. To
retrieve it, change into the instance directory and use either
 git or svn.

    cd /home/example/zinstance

### a. Clone from the Git repository

    git clone https://github.com/bikalabs/Bika-LIMS src/bika3

### b. From SourceForge with subversion

    sudo svn co https://bika.svn.sourceforge.net/svnroot/bika/bika3 src/bika3

## 9. Do the verbose buildout of the instance

    sudo bin/buildout -v

## 10. Create idserver.sh start script

Use the python binary from the instance's bin/plonectl script and create
a similar script to start the id-server, noting the port number.

    #!/bin/sh
    PYTHON=/home/exmple/Python-2.6/bin/python
    BIKA_BASE=/home/example/zinstance
    COUNTER_FILE=$BIKA_BASE/var/id.counter
    LOG_FILE=$BIKA_BASE/var/log/idserver.log
    PID_FILE=$BIKA_BASE/var/idserver.pid

    PORT=8031

    SRC_DIR=src/bika3

    exec $PYTHON $BIKA_BASE/$SRC_DIR/bika/lims/scripts/id-server.py \
        -f $COUNTER_FILE \
        -p $PORT \
        -l $LOG_FILE \
        -d $PID_FILE

## 11. Make start-idserver.sh executable and test

    sudo chmod +x start-idserver.sh

Start the id-server

    sudo su plone -c "./start-idserver.sh"

To test, use a text-mode browser 

    lynx http://localhost:8031/

A "1" should appear, incrementing on reloads. 

### 12. Create the stop-idserver.sh script:

    #!/bin/sh
    kill `cat var/idserver.pid`

## 13. Test web server configuration and new instance DNS

Ensure the web server configuration is valid

    sudo apache2ctl configtest

Ensure the DNS is active for the new instance name created

    dig example.bikalabs.com

If all valid, reload the webserver configuration

    sudo apachectl graceful

## 14. Remove/edit the id.counter file to reset the counter if needed and restart: (optional)

    sudo ./stop-idserver.sh
    sudo rm var/id.counter
    sudo su plone -c ./start-idserver.sh

## 15. First run of Bika instance

The first test run is done in foreground, noting any error messages

    sudo bin/plonectl fg

If no problems occur during startup, the end of the output should say

    2011-11-13 12:06:07 INFO Zope Ready to handle requests

## 16. Access Bika LIMS via browser

From a remote browser, use the URL 

    http://admin:password@example.bikalabs.com/manage

or, if on the same host  

    http://admin:password@localhost:8030/manage

## 17: Add a Bika site

Add a Plone site with the Bika extension, noting its instance name
by selecting the "Bika LIMS" option.

Click "Add Plone Site" in the top right of the Zope Management Interface. The dialog requires
an instance name, which is "Plone" by default, and a number of options to be ticked below.

## 18. Modify web server config to point to Bika instance root

For public consumption, the instance root maps to the site URL by changing the
web server rewrite rule and adding in the instance URL in  name.

    #Comment out and replace original rule, adding instance name Plone
    #RewriteRule ^/(.*) http://localhost:8030/VirtualHostBase/http/example.bikalabs.com:80/VirtualHostRoot/$1 [L,P]

    RewriteRule ^/(.*) http://localhost:8030/VirtualHostBase/http/example.bikalabs.com:80/Plone/VirtualHostRoot/$1 [L,P]

Reload the webserver configuration:

    sudo apache2ctl graceful

## 19. Stop foreground instance (Control C), start as process 

The foreground process can be stopped by typing Control-C. Then, restart the
 Plone instance as background process and optionally add it to the server startup scripts.

    sudo bin/plonectl start

Two commands should be added to startup scripts in eg. /etc/rc.local or equivalent:

    su plone -c /home/example/zinstance/start-idserver.sh
    /home/example/zinstance/bin/plonectl start

## 20. Test Bika LIMS

Test the LIMS on subdomain name URL as set up above by pointing
your browser at http://example.bikalabs.com/ or http://localhost:8031/Plone
