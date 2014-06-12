You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Installation
***
### Table of Contents
1. [Introduction](#introduction)  
2. [Linux Installation Steps](#linux-installation-steps)  
3. [Windows Installation Steps](#windows-installation-steps)  
4. [Extras](#extras)  

***
### Introduction
This document details the installation steps for Bika LIMS version 3.1 from the Plone Unified Installer package for Linux, as well as the setup for Apache as web proxy to make the LIMS available on the standard http port 80. The process should be similar for MacOSX and other Unix-type operating systems. The **gcc compiler**, **python**, the **python-dev library** and **git** are all required,  you may use those already installed on your operating system. Basic skills with GNU/Linux terminal are required.

***
### Linux Installation Steps

#### 1. Creating your Bika LIMS root directory
Create the directory where you want to place your LIMS instance and call it 'bika'. In this recipe, we'll place bika inside a /home/user directory, but ask to your system admin about the best place in your case.
```bash
$ cd /home/user
$ mkdir bika
```
#### 2. Installing required system packages
```
    $ sudo apt-get install python-dev build-essential libffi-dev libpcre3-dev
    $ sudo apt-get install gcc autoconf libtool pkg-config zlib1g-dev git-core
    $ sudo apt-get install libssl-dev libexpat1-dev libxslt1.1 gnuplot libpcre3
```
Bika LIMS uses the [WeasyPrint](http://weasyprint.org) module for pdf creation, so some other packages will be needed. For Debian 7.0 Wheezy or newer, Ubuntu 11.10 Oneiric or newer:

```bash
$ sudo apt-get install libcairo2 libpango1.0-0 libgdk-pixbuf2.0-0
```

#### 3. Installing Plone 4.3.2

```bash
$ wget -nc http://launchpad.net/plone/4.3/4.3.2/+download/Plone-4.3.2-UnifiedInstaller.tgz
```

Untar the archive and run the installer:

```bash
$ tar xzf Plone-4.3.2-UnifiedInstaller.tgz
$ cd Plone-4.3.2-UnifiedInstaller/
$ ./install.sh --build-python --static-lxml=yes --target=../bika standalone
```

This takes a while and some warnings might appear, omit them. Verify successful build from the output of the installer script. Refer to [Plone's installation documentation](http://plone.org/documentation/topic/Installation) if installation fails:

    ######################  Installation Complete  ######################

    Plone successfully installed at /home/example
    See /home/example/zinstance/README.html
    for startup instructions

    Use the account information below to log into the Zope Management Interface
    The account has full 'Manager' privileges.

    Username: admin
    Password: xyz
    ...


#### 4. Downloading Bika LIMS

```bash
$ cd ../bika/zinstance
$ git clone -b release/3.1 http://github.com/bikalabs/Bika-LIMS.git src/bika.lims
```

In this recipe, the Bika LIMS 3.1 release is used, which is the preferred version for production environments. Change ```release/3.1``` to ```develop``` if you want to use the latest Bika LIMS at your own risk. Refer to [git documentation](http://git-scm.com/documentation) for further information about how to update the source code or change to another branch from the repository. 

#### 5. Edit the buildout.cfg
Open the ``buildout.cfg`` (in ``bika/zinstance dir``):

```bash
$ nano buildout.cfg
```

  a) Find the ``eggs`` section.  Add ``bika.lims`` and ``WeasyPrint``:

       eggs =
           Plone
           Pillow
           bika.lims
           WeasyPrint

   b) Find the ``develop`` section. Add ``src/bika.lims``:

       develop =
           src/bika.lims
   
   c) Find the ``versions`` section. Add ``WeasyPrint = 0.19.2``

       [versions]
       WeasyPrint = 0.19.2

   c) (Optional) Change the Zope instance port if the default 8080 is not used::

       http-address = 8080

   d) (Optional) Change the ``effective-user`` if ``plone`` is not the one used. 

   e) (Optional) Add the environment-vars entry for the ID-server for multiple-client
       installations, noting port number. Refer to [External ID Server documentation](https://github.com/bikalabs/Bika-LIMS/wiki/External-ID-server) for further information.

       [instance]
       environment-vars =
           IDServerURL http://localhost:8081

#### 6. Execute the buildout
Do the (verbose, if needed) buildout of the instance::

```bash
$ bin/buildout -v
```

This takes a while and some warnings might appear. Verify successful build from the output of the installer script. Refer to [Plone's installation documentation](http://plone.org/documentation/topic/Installation) if installation fails:
```
*************** PICKED VERSIONS ****************
[versions]
CairoSVG = 1.0.7
Products.ATExtensions = 1.1
bika.lims = 3.0
cairocffi = 0.5.3
cffi = 0.8.2
cssselect = 0.9.1

#Required by:
#bika.lims 3.0
Products.AdvancedQuery = 3.0.3

#Required by:
#WeasyPrint 0.19.2
Pyphen = 0.9.1

#Required by:
#bika.lims 3.0
gpw = 0.2

#Required by:
#bika.lims 3.0
magnitude = 0.9.3

#Required by:
#cffi 0.8.2
pycparser = 2.10

*************** /PICKED VERSIONS ***************
```

#### 7. Get the admin's password
If the buildout finished successfully, an 'adminPassword.txt' have been created automatically inside zinstance directory. That file contains the super-user credentials you'll need to create the Bika site.
```bash
$ cat adminPassword.txt
```

#### 8. Test run in foreground, noting error messages if any and taking corrective action if so:
```bash
$ bin/plonectl fg
...
2011-11-13 12:06:07 INFO Zope Ready to handle requests
```

#### 9. Add the Plone instance with Bika LIMS extension
Open a browser and type http://localhost:8080/

Set 'Plone' as identifier and ensure that the Bika LIMS option is ticked.
If your server has no GUI installed, you can use a terminal line browser like ``lynx``:
```bash
$ lynx http://localhost:8080/
```

#### 10. Start working with Bika LIMS
```bash
$ bin/plonectl start
```
Open a browser and go to your Bika LIMS instance: http://localhost:8080/Plone

***
### Windows Installation Steps

#### 1. Download and Install Plone
Currently Bika LIMS for Windows is only compatible with Plone 4.3.1

* Download the Windows Installer from http://plone.org/products/plone/releases/4.3.1
* Execute the installer and follow through the steps

For this guide we will assume the default location of `C:\Plone43`  
For more information visit: http://docs.plone.org/manage/installing/index.html  
  
#### 3. Installing Bika LIMS
* Open `C:\Plone43\buildout.cfg` in a text editor
* Add **bika.lims** to the **eggs**
```
eggs =
    Plone
    Pillow
    Products.PloneHotfix20130618
    bika.lims
```

* Run __buildout__ from cmd **(** `⊞ Win` **>>** _type:_ `cmd` **>>** `↵ Enter` **)**  
```
cd C:\Plone43
bin\buildout.exe
```  
* A __successful__ buildout should output:
```
Updating run-instance.
Updating service.
*************** PICKED VERSIONS ****************
[versions]
bika.lims = 3.0
cairocffi = 0.5.4
cairosvg = 1.0.7
cssselect = 0.9.1
gpw = 0.2
magnitude = 0.9.3
products.advancedquery = 3.0.3
products.atextensions = 1.1
pycparser = 2.10
pyphen = 0.9.1

*************** /PICKED VERSIONS ***************
```

_If you see the following error:_ `Error: Couldn't install: cffi 0.8.2`  
Refer to: [Troubleshooting: A) Dependencies](#7-troubleshooting)  

_If you see the following error:_ `Error 5: Access is denied`  
Refer to: [Troubleshooting: B) Privileges](#7-troubleshooting)  

#### 4. Setting up Plone Services
* Run cmd as Administrator **(** `⊞ Win` **>>** _type:_ `cmd` **>>** `CTRL`+`⇧ Shift`+`↵ Enter` **)**  
* Navigate to the Plone root directory
```
cd C:\Plone43
```
* Install, Start and bring your newly created instance to the Foreground  
_this should stop the default Plone 4.3 Service_
```
bin\instance.exe install
bin\instance.exe start
bin\instance.exe fg
```  
_If you see the following error:_ `OSError: cannot load library libcairo.so.2`  
Refer to: [Troubleshooting: A) Dependencies](#7-troubleshooting)  

* If you see `INFO Zope Ready to handle requests` then the server is running
* Point your web browser at __http://localhost:8080__  
Congradulations!! you have a successful build of Bika LIMS 3.0 on Plone 3.4.1  
You can now create a site

#### 5. Upgrading
Please refer to [4. Downloading Bika LIMS](#4-downloading-bika-lims) in the [Linux Installation Steps](#linux-installation-steps)  

#### 6. Notes
* If you are having trouble starting `bin\instance.exe fg` as follows:
```
The program seems already to be running. If you believe not,  
check for dangling .pid and .lock files in var/.  
```
* You can try the following steps:  
```
-Find the running process id by opening the .pid file within your instance's var/ directory.  
-Open the Windows Task Manager and stop the running process with the above identifier.  
-Delete all .pid and .lock files in your instance's var/ directory.  
-Start your instance.  
```
__OR__
```
-Run services.msc
-Search for Plone 4.3
-Try Starting or Stopping it along with your instance
```

#### 7. Troubleshooting

A) __Dependencies__  
You need to install some dependencies manually  
Download and install __bika_dependencies(Plone 4.3.1).exe__ from https://github.com/zylinx/bika.dependencies  
This fixes the fact that Plone's buildout cannot compile the libraries required by weasyprint.  
It installs the pre-compiled binaries into System32 and Plone's installation folder instead.  

B) __Privileges__  
Open `Explorer` __>>__ Navigate to `C:\` __>>__ Right-Click on the `Plone43` directory __>>__ select `Properties`  
Select the `Security` Tab __>>__  Click `Edit`  __>>__ Check `Full Control` Allow for necessary User / Group  
Click  `Apply`


***
### Extras

#### Using apache to redirect http requests to Bika LIMS
Set up a domain name for the LIMS site URL and add the Apache mapping noting the Zope server port used by the instance (default 8080).
Follow the instructions here: https://github.com/bikalabs/Bika-LIMS/blob/3.01a/docs/APACHE.md

#### Update the Bika source code from the GitHub repository
Rename (or move) the src/bika.lims directory or rerun the ``git clone`` or ``git pull`` or switch branches on the command line in the source directory src/bika.lims, then re-run bin/buildout.

#### Start with a completely fresh instance::
Rename/move the Data.fs.* files in var/filestorage (after stopping instance). 