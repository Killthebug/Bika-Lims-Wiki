### Table of Contents
1. [Introduction](#introduction)
2. [Linux Installation Steps](#linux-installation-steps)
3. [Linux Installer Script](#linux-installer-script)
4. [Upgrading Bika LIMS](#upgrading-bika-lims)
5. [Windows Installation Steps](#windows-installation-steps)
6. [Extras](#extras)

***

### Introduction

If you have any issues you should read the following pages carefully:

- [Plone Installation Documentation](http://docs.plone.org/manage/installing/index.html).

- [Guide to deploying and installing Plone in production](http://docs.plone.org/manage/deploying/index.html).

***

### Linux Installation Steps

The most commonly used OS for running Plone and Bika LIMS is Linux, although this process should be fairly similar for all systems on which Plone is supported.

#### 1. Install the following system packages

You can paste the following command directly into a terminal:

    sudo apt-get install build-essential python-dev git-core libffi-dev libpcre3-dev gcc autoconf libtool pkg-config zlib1g-dev apt-get install libssl-dev libexpat1-dev libxslt1.1 gnuplot libpcre3 libcairo2 libpango1.0-0 libgdk-pixbuf2.0-0

The version numbers of dependencies are known to be valid in Ubuntu 12.04, 14.04, and Debian Wheezy.  If you use a different distribution or version, you may need to find the versions of these packages which are provided with your system.

#### 2. Download and Install Plone

Download the latest stable release of the Plone Unified Installer from http://plone.org/products/plone/releases and unpack the file. The following installation command will be sufficient for most purposes:

    ./install.sh --target=/usr/local/Plone --build-python --static-lxml zeo

#### 3. Add Bika LIMS to your buildout.cfg

In your new Plone folder, you will see a folder named `zeocluster`.  Change directory into this folder, and edit `buildout.cfg`.  Find the section beginning with `eggs=`, and add `bika.lims` to the existing entries.

    eggs =
        Plone
        Pillow
        bika.lims

Save the file, and then run bin/buildout again.  Buildout will download and install all remaining dependencies.

Verify successful build from the output of the installer script, which should include a list of found versions like this:

    *************** PICKED VERSIONS ****************
    [versions]
    Babel = 1.3
    ...
    plone.jsonapi.core = 0.4
    *************** /PICKED VERSIONS ***************

> The "PICKED VERSIONS" block above indicates a successful buildout.  If errors appear while buildout is installing dependencies, you can safely ignore them if this message is displayed.

> If the downloads are interrupted, simply run bin/buildout again.  The process will be resumed.

If the buildout finished successfully, an 'adminPassword.txt' will be created automatically inside the Plone instance folder. It contains the super-user credentials you'll need to login with.

#### 4. Test run

To start Plone in debug mode, run this command:

    bin/plonectl fg

Note any error messages, and take corrective action if required. If no errors are encountered, you can press Control+C to exit.

#### 5. Add a new Plone site

Open a browser and go to http://localhost:8080/.  Select "Add Plone Site", ensure that the Bika LIMS option is checked, then submit the form.

#### 6. Start working with Bika LIMS

    bin/plonectl start

Open a browser and go to your Bika LIMS instance: http://localhost:8080/Plone

***

### Upgrading Bika LIMS

If a new release of the LIMS is made available, the following procedure will upgrade your existing installation to use the new packages.

> Please read [Releases life-cycle](https://github.com/bikalabs/Bika-LIMS/wiki/Releases-life-cycle) documentation before upgrading your Bika LIMS instance.

#### 1. Backup

Make a full backup of your instance before continuing:

    bin/snapshotbackup

#### 2. Buildout

Run buildout with the "-n" option, to retreive the latest version of Bika LIMS and it's dependencies.

    bin/buildout -n

#### 3. Restart Plone

    bin/plonectl restart

#### 3. Migrate

Start Plone and login to your site as admin.  Go to site-setup, and click `Add-ons`.  Find Bika LIMS in the list of activated addons, and click the "bika.lims" upgrade button.

***

### Windows Installation Steps

#### 1. Download and Install Plone

Download the latest version of the Plone 4.3 Unified Installer from http://plone.org/products/plone/releases.  Execute the installer and follow through the steps.

> For this guide we will assume the default location of `C:\Plone43`

#### 2. Add Bika LIMS to your buildout.cfg

Open `C:\Plone43\buildout.cfg` in a text editor

Add `bika.lims` to the `eggs` section

    eggs =
        Plone
        Pillow
        Products.PloneHotfix20130618
        bika.lims

#### 3. Run buildout

Save and close the file, start cmd as Administrator (Click start, type "cmd", and press CTRL+SHIFT+ENTER), and then run bin/buildout.  Buildout will download and install all remaining dependencies.

    cd C:\Plone43
    bin\buildout

Verify successful build from the output of the installer script, which should include a list of found versions like this:

    *************** PICKED VERSIONS ****************
    [versions]
    bika.lims = 3.0
    ...
    pyphen = 0.9.1
    *************** /PICKED VERSIONS ***************

> The "PICKED VERSIONS" block above indicates a successful buildout.  If errors appear while buildout is installing dependencies, you can safely ignore them if this message is displayed.

> If the download is interrupted, simply run bin/buildout again.  The process will be resumed.

> If you see the following error: `Error: Couldn't install: cffi 0.8.2` Refer to: [Troubleshooting: A) Dependencies](#6-troubleshooting)

> If you see the following error: `Error 5: Access is denied` Refer to: [Troubleshooting: B) Privileges](#6-troubleshooting)

#### 4. Setting up Plone Services

Navigate to the Plone root directory

    cd C:\Plone43

Plone and Windows do not always play well together.  Install, Start and bring your newly created instance to the Foreground  (this should stop the default Plone 4.3 Service)

    bin\instance.exe install
    bin\instance.exe start
    bin\instance.exe fg

> If you see the following error: `OSError: cannot load library libcairo.so.2` Refer to: [Troubleshooting: A) Dependencies](#7-troubleshooting)

When you see `INFO Zope Ready to handle requests` the server is ready.

Open a browser and go to http://localhost:8080/. Select "Add Plone Site", ensure that the Bika LIMS option is checked, then submit the form.

Congratulations!! you have a successful build of Bika LIMS on Windows.

#### 5. Notes

If you are having trouble starting `bin\instance.exe fg` as follows:

    The program seems already to be running. If you believe not,
    check for dangling .pid and .lock files in var/.

You can try the following steps:

    -Find the running process id by opening the .pid file within your instance's var/ directory.
    -Open the Windows Task Manager and stop the running process with the above identifier.
    -Delete all .pid and .lock files in your instance's var/ directory.
    -Start your instance.

    __OR__

    -Run services.msc
    -Search for Plone 4.3
    -Try Starting or Stopping it along with your instance

#### 6. Troubleshooting

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

### Report errors directly to Bika Lab Systems:

To help us find and fix errors faster, please consider adding the following snippet to your test instance's buildout:

    eggs =
        ...
        raven

Then, in all [client] or [instance] sections, insert this:

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

Finally, add raven 4.0.4 into [versions] section

    [versions]
        ...
        raven = 4.0.4

Run bin/buildout, and restart Plone.
