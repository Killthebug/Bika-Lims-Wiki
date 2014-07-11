You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Bika LIMS Installation
***
### Table of Contents
1. [Introduction](#introduction)
2. [Linux Installer Script](#linux-installer-script)
3. [Linux Installation Steps](#linux-installation-steps)
4. [Upgrading Bika LIMS](#upgrading-bika-lims)
5. [Windows Installation Steps](#windows-installation-steps)
6. [Extras](#extras)

***
### Introduction
This document details the installation steps for Bika LIMS version 3.1.

***
### Linux Installer Script
For unix-like systems, you can download and run an installer script.  This should install the prerequisites, Plone and Bika LIMS in /home/bika and start the server in foreground on port 8080.  You can obtain and run the script by running the following commands:

```bash
wget -nc --no-check-certificate https://raw.githubusercontent.com/bikalabs/Bika-LIMS/develop/install.sh
chmod +x install.sh
sudo ./install.sh
````

***
### Linux Installation Steps

The process should be similar for all systems on which Plone is supported.

#### 1. Install the following required system packages

```bash
sudo apt-get install python-dev build-essential libffi-dev libpcre3-dev
sudo apt-get install gcc autoconf libtool pkg-config zlib1g-dev git-core
sudo apt-get install libssl-dev libexpat1-dev libxslt1.1 gnuplot libpcre3
sudo apt-get install libcairo2 libpango1.0-0 libgdk-pixbuf2.0-0
```

> The version numbers of dependencies are valid for Ubuntu 12.04.  If you use a different distribution or version, you may need to find the versions of these packages which are provided with your system.

#### 2. Install Plone

Refer to the [Plone Installation Documentation](http://docs.plone.org/manage/installing/index.html)

#### 3. Add Bika LIMS to your buildout.cfg

Go to your new Plone instance, and edit buildout.cfg.  Find the section beginning with `eggs=`, and add `bika.lims` to the existing entries.

    eggs =
        Plone
        Pillow
        bika.lims

> Indentation in buildout.cfg is important, and should be kept uniform for all lines in the eggs section.

Save the file, and then run bin/buildout again.  Buildout will download and install all remaining dependencies.

> If the download is interrupted, simply run bin/buildout again.  The process will be resumed.

Verify successful build from the output of the installer script, which should include a list of found versions like this:

    *************** PICKED VERSIONS ****************
    [versions]
    Babel = 1.3
    CairoSVG = 1.0.7
    Products.ATExtensions = 1.1
    Products.AdvancedQuery = 3.0.3
    PyYAML = 3.11
    Pygments = 1.6
    Pyphen = 0.9.1
    Werkzeug = 0.9.4
    argh = 0.24.1
    bpython = 0.13
    cairocffi = 0.5.3
    cffi = 0.8.2
    collective.progressbar = 0.5
    collective.wtf = 1.0b9
    cssselect = 0.9.1
    gpw = 0.2
    i18ndude = 3.3.3
    magnitude = 0.9.3
    pathtools = 0.1.2
    plone.api = 1.1.0
    plone.jsonapi.core = 0.4
    *************** /PICKED VERSIONS ***************

> If errors appear while buildout is installing dependencies, ignore them.

If the buildout finished successfully, an 'adminPassword.txt' will have been created automatically inside the Plone instance folder. It contains the super-user credentials you'll need to create the Bika site.

#### 4. Test run in foreground, noting error messages if any and taking corrective action if so:

```bash
bin/plonectl fg
```

#### 5. Add the Plone instance with Bika LIMS extension

Open a browser and go to http://localhost:8080/.  Select "Add Plone Site", and ensure that the Bika LIMS option is checked, then submit the form.

#### 6. Start working with Bika LIMS

```bash
bin/plonectl start
```

Open a browser and go to your Bika LIMS instance: http://localhost:8080/Plone

***
### Upgrading Bika LIMS

If a new release of the LIMS is made available, the following procedure will upgrade your existing installation to use the new packages.

#### 1. Backup

Stop Plone, and make a full backup of your instance before continuing.

#### 2. Buildout

Run buildout with the "-n" option, to retreive the latest version of Bika LIMS and it's dependencies.

```bash
bin/buildout -n
```

#### 3. Migrate

Start Plone and login to your site as the admin user.  Go to site-setup, and click `Add-ons`.  Find Bika LIMS in the list of activated addons, and click the "bika.lims" upgrade button.

***
### Windows Installation Steps

#### 1. Download and Install Plone
Currently Bika LIMS for Windows requires a .Plone 4.3.1 installation.

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

#### 5. Notes
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

#### Start with a completely fresh instance::
Rename/move the Data.fs.* files in var/filestorage (after stopping instance).  Running buildout again will regenerate these files.

#### Report errors directly to Bika Lab Systems:

Add raven to your buildout.cfg in the eggs= section

    eggs = 
        ...
        raven

Then add the following snippet to your [instance] section.  If you are using a ZEO configuration, add this to all [clientX] sections:

```
event-log-custom =
    %import raven.contrib.zope
    <logfile>
      path ${buildout:directory}/var/client1/event.log
      level INFO
      max-size 5 MB
      old-files 5
    </logfile>
    <sentry>
      dsn http://90723864025d4520b084acee225ddb8a:f9f7dd0163a74fbeac4e24a5123b3d39@sentry.bikalabs.com/2
      level ERROR
    </sentry>
```

Run bin/buildout, and restart Plone.