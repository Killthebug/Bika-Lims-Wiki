### Table of Contents
1. [Introduction](#introduction)
2. [Linux Installation Steps](#linux-installation-steps)
3. [Linux Installer Script](#linux-installer-script)
4. [Upgrading Bika LIMS](#upgrading-bika-lims)
5. [Windows Installation Steps](#windows-installation-steps)
6. [OS X Yosemite Installation Steps](#os-x-yosemite-installation-steps)
7. [Enable Error Reporting](#enable-error-reporting)

***

### Introduction

Before beginning this process you should read the following documentation:

- [Plone Installation Documentation](http://docs.plone.org/manage/installing/index.html).

If you have any issues you should read the following pages carefully:

- [Guide to deploying and installing Plone in production](http://docs.plone.org/manage/deploying/index.html).

- [Simple Linux Plone Bika nginx SSL varnish zeocluster Supervisor configuration](https://github.com/bikalabs/Bika-LIMS/wiki/simple-Linux-Plone-Bika-nginx-SSL-varnish-zeocluster-supervisor-configuration)

***

### Linux Installation Steps

The most commonly used OS for running Plone and Bika LIMS is Linux, although this process should be fairly similar for all systems on which Plone is supported.

#### Install the following system packages

You can paste the following command directly into a terminal:

    sudo apt-get install build-essential python-dev git-core libffi-dev libpcre3-dev gcc autoconf libtool pkg-config zlib1g-dev libssl-dev libexpat1-dev libxslt1.1 gnuplot libpcre3 libcairo2 libpango1.0-0 libgdk-pixbuf2.0-0

The version numbers of dependencies are known to be valid in Ubuntu 12.04, 14.04, and Debian Wheezy.  If you use a different distribution or version, you may need to find the versions of these packages which are provided with your system.  The packages may also have slightly different names in different distributions.

#### Download and Install Plone

Download the latest stable release (4.3.x series) of the Plone Unified Installer from http://plone.org/products/plone/releases and unpack the file. The following installation command will be sufficient for most purposes:

    ./install.sh --target=/usr/local/Plone --build-python zeo

> If you receive an lxml error, you may need to include the `--static-lxml` flag in the above command.

> If you are installing for a production system, you should run the above command as root.  This will create additional users `plone_buildout` and `plone_daemon`, to add an extra layer of security to the installation.  If you do this, you will need to run buildout related commands as the plone_buildout user, and you will need to run Plone as the plone_daemon user.  For demo and testing instances, you can run the installation as a normal user.

#### Add Bika LIMS to your buildout.cfg

In your new Plone folder, you will see a folder named `zeocluster`.  Change directory into this folder, and edit `buildout.cfg`.  Find the section beginning with `eggs=`, and add `bika.lims` to the existing entries.

    eggs =
        Plone
        Pillow
        bika.lims

> In buildout.cfg, each line should have the same indentation as the one preceding it.  Mixing tabs and spaces may also cause errors.

Save the file, and then run `bin/buildout`.  Buildout will download and install all remaining dependencies.

> If you created a root installation, you will need to run buildout like this:

    sudo -u plone_buildout bin/buildout

Verify successful build from the output of the installer script, which should include a list of found versions like this:

    *************** PICKED VERSIONS ****************
    [versions]
    Babel = 1.3
    ...
    plone.jsonapi.core = 0.4
    *************** /PICKED VERSIONS ***************

> The "PICKED VERSIONS" block above indicates a successful buildout.  If warnings appear while buildout is installing dependencies, you can safely ignore them if this message is displayed.

> If the downloads are interrupted, simply run bin/buildout again.  The process will be resumed.

If the buildout finished successfully, an 'adminPassword.txt' will be created automatically inside the Plone instance folder. It contains the super-user credentials you'll need to login with.

#### Test run

First, you will need to start the zeoserver (this is the database process).

    bin/zeoserver start

To start a Plone client debug mode, run this command:

    bin/client1 fg

If you installed Plone using a root installation, you will need to use the following commands instead:

    sudo -u plone_daemon bin/zeoserver start
    sudo -u plone_daemon bin/client1 fg

Note any error messages, and take corrective action if required. If no errors are encountered, you can press Control+C to exit.

#### Add a new Plone site

Open a browser and go to http://localhost:8080/.  Select "Add Plone Site", ensure that the Bika LIMS option is checked, then submit the form.

#### Start working with Bika LIMS

Open a browser and go to your Bika LIMS instance: http://localhost:8080/Plone

***

### Upgrading Bika LIMS

If a new release of the LIMS is made available, the following procedure will upgrade your existing installation to use the new packages.

> Please read [Releases life-cycle](https://github.com/bikalabs/Bika-LIMS/wiki/Releases-life-cycle) documentation before upgrading your Bika LIMS instance.

#### Backup

Make a full backup of your instance before continuing:

    bin/snapshotbackup

#### Buildout

Run buildout with the "-n" option, to retreive the latest version of Bika LIMS and it's dependencies.

    bin/buildout -n

#### Restart Plone

    bin/plonectl restart

#### Migrate

Start Plone and login to your site as admin.  Go to site-setup, and click `Add-ons`.  Find Bika LIMS in the list of activated addons, and click the "bika.lims" upgrade button.

***

### Windows Installation Steps

## Please reconsider, and use Linux

Plone recently stopped building the Unified Installer for Windows in favour of a VirtualBox/Vagrant setup. Unless someone is really interested in integrating Bika into the Windows ways of doing things (i.e. server roles, IIS config, whatever), Bika LIMS can be installed in a single well-maintained Virtual Machine.

Getting help on users/developer lists for Windows-specific questions could be harder because most of the developers behind the scenes use GNU/Linux instead of Windows.

Bika Windows users are participating bika-win@googlegroups.com. Post to bika-win@googlegroups.com. Please also Google using the error messages you get, many have been dealt with before.

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

### OS X Yosemite Installation Steps

We need homebrew for install some dependencies, open a terminal and install brew with this command:

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

After the installation of brew we need to install some packages, so:

    brew install Caskroom/cask/xquartz
    brew install cairo pango gdk-pixbuf libxml2 libffi

Download and install Plone 4.3.3 OSX installer from official Plone repository (https://launchpad.net/plone/4.3/4.3.3/+download/Plone-4.3.3-64.dmg) because the 4.3.4 doesn’t have the OsX package, compiling from source give lot of error during compilation and i want to keep the system as clean as possible. Install Plone with zeocluster option, then open a terminal and 

    cd /Applications/Plone/zeocluster
    nano buildout.cfg

replace
```
extends =
    base.cfg
    versions.cfg
#    http://dist.plone.org/release/4.3.3-xxxxxxx/versions.cfg


# If you change your Plone version, you'll also need to update
# the repository link below.
find-links +=
    http://dist.plone.org/release/4.3.3-xxxxx
```
with
```
extends =
    base.cfg
#    versions.cfg
    http://dist.plone.org/release/4.3.4/versions.cfg


# If you change your Plone version, you'll also need to update
# the repository link below.
find-links +=
    http://dist.plone.org/release/4.3.4
```
then

    ./bin/buildout

This will upgrade your Plone to the last stable version, it take some time depending on your connection and system.

Edit again buildout.cfg

    nano buildout.cfg

and replace
```
eggs =
    Plone
    Pillow
```
with
```
eggs =
    Plone
    Pillow
    bika.health (or bika.lims it depend on your choice)
```
then

    ./bin/buildout

If no errors are shown simply start the zeocluster server and clients

    ./bin/plonectl start

Point your browser to http://localhost:8080 and install your Bika site.

***

### Enable error reporting

If you have problems that are hard to diagnose or reproduce, or if you just want to help us find and fix errors faster, please consider adding the following lines to your buildout's eggs= section.  It's the easiest way to contribute!

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
          dsn http://ffa1dbda4858459a99a80065947bc666:8989999123e54cf0acdd8dca79899872@sentry.bikalabs.com/2
          level ERROR
        </sentry>

Finally, add raven 4.0.4 into [versions] section

    [versions]
        ...
        raven = 4.0.4

Run bin/buildout, and restart Plone.
