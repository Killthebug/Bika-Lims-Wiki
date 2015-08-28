Steps to configuring a Bika LIMS development environment

1) Install Plone and Bika LIMS

Follow the [Bika-LIMS installation instructions](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-Installation).  When installing for development or testing, it is not necessary to do a root mode installation.

> Although installing a standalone instance is quite sufficient for development purposes, a zeocluster is the preferred method.  The zeocluster method, used by most developers, is more flexible and reduces future confusion.

2) Install and configure git

Follow these steps: [Github: Set Up Git](https://help.github.com/articles/set-up-git).

3) Fork and Clone the Bika-LIMS sources

Follow the instructions at [Github: Fork A Repo](https://help.github.com/articles/fork-a-repo).

> When you clone the new fork that you have created, you should place it in the `src/` directory inside your Plone installation. 

> By default, the directory name of the new clone will be that of the repository, but the location and name of the source folder is up to you.

4) Configure Buildout

You must edit `buildout.cfg`.  You will need to add or edit the "develop = " statement in the [buildout] section, to include the folder that contains the forked source.

    develop =
        src/Bika-LIMS

> Remember to run `bin/buildout` again.

> Remember also, that the precise command is "bin/buildout" (or in the case of windows, bin\buildout).  The buildout.cfg file is located in the current working directory, so changing into the bin folder before running the command will not work.

5) Start the ZEO server

    cd Plone/zeocluster
    bin/zeoserver start

> This needs to be done once only - there is no need to re-start the zeoserver unless you have made changes to the data files on disk (eg, replaced the contents of the var/ folder, or restored from a backup)

6) Start Plone

During development, you should be starting Plone in debug/foreground mode, with the following commands:

    cd Plone/zeocluster
    bin/client1 fg

7) Next steps

- Join the bika-developers list at http://lists.sourceforge.net/lists/listinfo/bika-developers, and the IRC channel at irc.freenode.net/#bika immediately. We will be happy to help you acheive the highest code quality with your customisation project.

- Read the excellent [Plone developer documentation](http://docs.plone.org/develop/index.html).
