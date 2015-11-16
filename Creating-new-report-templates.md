Introduction
------------

If you want to add customized report templates, you have to create a new Plone add-on aka Python package.

There are only a few simple steps required.

* Create the add-on using ZopeSkel for all the boilerplate code.
* Add report templates to the add-on and register them in your ZCML.
* Make your add-on available to buildout.

Create the add-on
-----------------

> You can use an existing extension package to create your reports.  If you have already modified [bika.custom](https://github.com/bikalabs/bika.custom) or created an extension in some other way, you can skip this step and continue with [Add report templates to your addon](add-report-templates-to-your-addon)

If you don't have ZopeSkel already installed, add the following list of eggs to your
buildout.cfg.

    eggs =
        ZopeSkel
        Paste
        PasteDeploy
        PasteScript

Don't forget to always run buildout after changes to the buildout.cfg.  Also remember to keep the indentation in buildout.cfg consistent between lines.

Now, in a command prompt or shell window, change directory to the *src* folder of your Plone instance, and create your customization add-on:
 
    zopeskel plone customer.reports

> You will probably need to type the full path to the `zopeskel` script.  This should be located in the *bin* folder of your Plone instance.

> You can Change *customer.reports* to whatever you like.

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

Add report templates to your addon
----------------------------------

Change into the directory *customer.reports/customer/reports* and add a new directory for your report templates.

    cd customer.reports/customer/reports
    mkdir templates

Add some templates to the newly created directory.  As a starting point you can copy the default template from the bika.lims repository.  You can browse directly to the default template by clicking [here](https://github.com/bikalabs/Bika-LIMS/tree/master/bika/lims/browser/analysisrequest/templates/reports).

Next you have to register this directory to be found by Bika LIMS.  Edit your configure.zcml file in the appropriate places:

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

> In the plone:static directive you have to match the directory name to the one created in the previous step.

> *type* must be set to *reports* in order to get recognized by bika.lims as a report template resource.

> The *name* is solely for identifying your new report template in the pull-down on the publishing process.

Install your addon using buildout
---------------------------------

Now, you need to tell Plone to use your newly created addon.  To do this, edit buildout.cfg:

* Add "customer.reports" to the "eggs=" section.
* Add "src/customer.reports" to the "develop=" section.

Now, run "bin/buildout" again.  If this completes without errors, you should be able to install the new add-on in Plone.

* If you already have a Plone site, you can browse to site-setup->addons, and activate the new addon.

* If you are creating a new Plone site, you should select "Bika LIMS" and also select your new add-on, before clicking the "Add Plone Site" button.

Fields reference
----------------
```
client_order_num:''
managers:{
    dict:{
        labcontact-1:{
            departments:''
            signature:''
        }
    }
    ids:[]
}
member_discount:''
sample:{
    sample_type:{
        url:''
        id:''
        title:''
    }
    remarks:''
    id:''
    url:''
    sample_point:{
    }
    client_sampleid:''
    sampler:{
    }
}
laboratory:{
    accreditation_body:''
    accreditation_logo:''
    title:''
    url:''
    logo:''
    address:''
}
id:''
categorized_analyses:{
    Lab Analyses:{
        Aguas. Caracteres físico-químicos:[]
        Aguas. Agentes para el tratamiento de potabilización:[]
        Aguas. Caracteres microbiológicos:[]
        Metales:[]
    }
}
qcanalyses:[]
points_of_capture:[]
reporter:{
    username:''
    fullname:''
    email:''
    signature:''
}
categorized_qcanalyses:{
    c:{
        Lab Analyses:{
            Metales:[]
        }
    }
    b:{
        Lab Analyses:{
            Metales:[]
        }
    }
}
specifications:{
    url:''
    id:''
    resultsrange:{
        pH:{
            hidemin:''
            rangecomment:''
            hidemax:''
        }
        Ni:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Conduct_25:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Color_A436:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Cl_total:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Pb:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Cl_libre:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Ecoli:{
            hidemin:''
            rangecomment:''
            min:''
            max:''
            hidemax:''
            error:''
        }
        NH3-N:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Enterococos:{
            hidemin:''
            rangecomment:''
            min:''
            max:''
            hidemax:''
            error:''
        }
        Cl_combinado:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Fe:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Color_A525:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
        Coliformes_totales:{
            hidemin:''
            rangecomment:''
            min:''
            max:''
            hidemax:''
            error:''
        }
        Cr:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Aerobios_22:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Cperfringens:{
            hidemin:''
            rangecomment:''
            min:''
            max:''
            hidemax:''
            error:''
        }
        Aerobios_36:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Cu:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
        }
        Color_A620:{
            hidemin:''
            rangecomment:''
            min:''
            hidemax:''
            error:''
        }
    }
    title:''
}
client:{
    fax:''
    name:''
    url:''
    phone:''
    address:''
    id:''
}
department_analyses:{
    99dd3fde33b14ee8a0b195df38270c64:{
        Lab Analyses:{
            Metales:[]
        }
    }
    b6f56330851b4e0cac89a95401ad6c78:{
        Lab Analyses:{
            Aguas. Caracteres físico-químicos:[]
            Aguas. Agentes para el tratamiento de potabilización:[]
            Aguas. Caracteres microbiológicos:[]
        }
    }
}
resultsinterpretationdepts:{
    :{
        uid:''
        richtext:''
    }
    Microbiología:{
        uid:''
        richtext:''
    }
    Química:{
        uid:''
        richtext:''
    }
}
portal:{
    url:''
}
remarks:''
analyses:[]
categories:[]
client_reference:''
footer:''
url:''
client_sampleid:''
batch:{
}
contact:{
    fullname:''
    email:''
}
resultsinterpretation:''
```