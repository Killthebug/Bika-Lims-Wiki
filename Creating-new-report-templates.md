Introduction
------------

If you want to add customized report templates, you have to create a new Plone add-on aka Python package.

There are only a few simple steps required.

* Create the add-on using ZopeSkel for all the boilerplate code.
* Add report templates to the add-on and register them in your ZCML.
* Make your add-on available to buildout.

Create the add-on
-----------------

If you don't have ZopeSkel already installed, add the following list of eggs to your
buildout.cfg.

    eggs =
        ZopeSkel
        Paste
        PasteDeploy
        PasteScript

Don't forget to always run buildout after changes to the buildout.cfg.  Also remember to keep the indentation in buildout.cfg consistent between lines.

Now create your customization add-on in your src directory:

    zopeskel plone customer.reports

Change *customer.templates* to whatever you like.

Answer the question for *Expert Mode* with *easy* and hit return on the following questions.

Now you have a new Plone add-on:

    customer.reports
    ├── customer
    │   ├── __init__.py
    │   └── reports
    │       ├── configure.zcml
    │       ├── __init__.py
    │       └── tests.py
    ├── docs
    │   ├── HISTORY.txt
    │   ├── INSTALL.txt
    │   ├── LICENSE.GPL
    │   └── LICENSE.txt
    ├── README.txt
    ├── setup.cfg
    └── setup.py

Change into the directory *customer.reports/customer/reports* and add a new directory for your report templates.

    cd customer.reports/customer/reports
    mkdir templates

Add some templates to the newly created directory.

As a starting point you can copy the default template from 
*bika.lims/bika/lims/browser/analysisrequest/templates/reports*.

Next you have to register this directory to be found by bika.lims.

Edit your configure.zcml file in the appropriate places:

    <configure
        xmlns="http://namespaces.zope.org/zope"
        xmlns:five="http://namespaces.zope.org/five"
        xmlns:i18n="http://namespaces.zope.org/i18n"
        xmlns:plone="http://namespaces.plone.org/plone"
        i18n_domain="customer.reports">
        <five:registerPackage package="." initialize=".initialize" />
        <!-- -*- extra stuff goes here -*- -->
       <include package="plone.resource" file="meta.zcml"/>
       <plone:static
           directory="templates"
           type="reports"
           name="customer"
       />
    </configure>

In the plone:static directive you have to match the directory name to the one created in the previous step.
*type* must be set to *reports* in order to get recognized by bika.lims as a report template resource.
The *name* is solely for identifying your new report template in the pull-down on the publishing process.

Go back to your buildout.cfg, add the newly created add-on, run buildout and the new template will show up.
