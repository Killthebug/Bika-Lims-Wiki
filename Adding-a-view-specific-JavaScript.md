You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Developing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/wiki/Developing-Bika-LIMS) · Adding View-Specific JavaScript
***
### Table of Contents
1. [Introduction](#introduction)
2. [Create Specific JavaScript Files](#create-specific-javascript-file)
3. [Modify JavaScript's Loader](#modify-loader)
4. [Modify JavaScript's Registry](#modify-registry)
5. [Make an Upgrade](#make-an-upgrade)

***

### Introduction

If you want to add some new JavaScript functionalities in a specific View page, write the actions and functions inside a generic JavaScript file, provably, is going to cause problems.

It is due because a generic file would be loaded for every opened page and they could rewrite methods defined before.

The best solution is to write a specific JavaScript file which only could be loaded with their own view, avoiding to modify other views.

### Create Specific JavaScript Files

The JavaScript file has to be allocated in _bika/lims/browser/js/_

#### File Name

There is a model to write the file name correctly and avoid confusions: `bika.lims.<contenttype>.<action>.js`.

For example, if we want to generate a JavaScript file to control _artemplate's edit view_, the ``<contenttype>`` is _artemplate_ and the ``<action>`` is _edit_. In this case, the obtained file's name would be: **bika.lims.artemplate.edit.js**.

#### File Structure

The file has to be composed by a function with the name of the object it is featuring.

```JavaScript
function <ContentTypeAction>View() {
```

The next field has to contain the translating parameters and the "global" variables we are going to use.

```JavaScript
    window.jarn.i18n.loadCatalog("bika");
    var _ = window.jarn.i18n.MessageFactory("bika");
    var that = this;
    var first = $('#whateverIwant')
    ...
```

Here is the place where the main function which invoke all actions/functions is defined. The method load is the entry point of the class. As we will see later this method is called automatically by bika.lims.loader.js

```JavaScript
     that.load = function() {
        function1();
        ...
    }
```
After that, we can define all the functions to be used.

```JavaScript
    function function1() {
    ...
    }
}
```

### Modify JavaScript's Loader

After the new JavaScript file has been defined, now we have to modify the JS Loader file (_bika.lims.loader.js_). It is located in the same directory as the before JavaScript file. 

This file only contains a function which starts when the page has been fully loaded. 

```JavaScript
(function( $ ) {
$(document).ready(function(){
```

Then, it defines a variable which hosts a dictionary with the correlation amongst DOM nodes (using jQuery style selector) and the JS view to be loaded. This is the variable where we have to add our View-JavaScriptClass relation.

To define the selector, is a good idea to use the body's class _template-_ and _portaltype-_ definitions because they specifies very well the view and, in addition, they are unique.

For example in case we were defining a JavaScript to use in the method view, we would have the follow body class in the HTML's view: 

```HTML
<body class="template-base_view portaltype-method site-Plone section-methods subsection-method-24 icons-on userrole-member userrole-labmanager userrole-site administrator userrole-authenticated" dir="ltr">
```
In the example, we can see _template-base_view portaltype-method_ definition class. It means that the loaded page shows a method view.

Following the example, we can add the next dictionary key:

```JavaScript
    var views = {
            ...
        ".template-base_view.portaltype-method":
            ['MethodBaseView'],
            ...
    };
```

Then, bika.lims.loader call the load() method for the desired JS View.

```JavaScript
 var loaded = new Array();
    for (var key in views) {
        if ($(key).length) {
            views[key].forEach(function(js) {
                if ($.inArray(js, loaded) < 0) {
                    obj = new window[js]();
                    obj.load();
                    loaded.push(js);
                }
            });
        }
    }
});
}(jQuery));
```

### Modify JavaScript's Registry

There is an xml file which defines each JS file to be load in the LIMS installation.The file's path is bika/lims/profiles/default/jsregistry.xml.

There we have to add our feature at the bottom of the file, above the commented line that says  <!-- Add javascripts ALWAYS BEFORE this one -->

Following the example above, the registry file will look like:

```JavaScript
   ...
<javascript authenticated="False"
              id="++resource++bika.lims.js/bika.lims.method.view.js"
              cacheable="True"
              compression="safe"
              conditionalcomment=""
              cookable="True"
              enabled="on"
              expression=""
              inline="False"
              />
   ...
```

### Make an Upgrade

After that, we surely want to add this feature in our current installed bika, for this reason we have to make an upgrade to rerun the jsregistry.