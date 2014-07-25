You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Developing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/wiki/Developing-Bika-LIMS) · Translations and Localisation

***

### Table of Contents
1. [Translating strings in Python code](#translating-strings-in-python-code)
2. [Translating strings in javascript code](#translating-strings-in-javascript-code)
3. [Dates and Times](#dates-and-times)
4. [Variable Substitutions](#variable-substitutions)
5. [Overriding Translations](#overriding-translations)

***

The excellent [Plone documentation](http://docs.plone.org/develop/plone/i18n/internationalisation.html).

***

### Translating strings in Python code

In most python code, just using a MessageFactory is sufficient:

    message = _("Some string to be translated")

The MessageFactory does not actually translate anything though.  That is normally handled by TAL, or some other utility such as plone_utils.addPortalMessage().

In the case of strings that are being rendered by code outside of Plone (eg, sending an email, writing a text file), the translation must be done manually:

    translated_string = context.translate(message)

There is a small utility `t` in the bika.lims.utils package for obtaining correctly translated, utf-8 encooded strings:

    from bika.lims.utils import t
    translated = t(_("Some english string"))

The MessageFactory substitutes mapping keys with gettext-style ${} syntax.  Don't use regular string concatenation to create these strings, the message translation will not work.

    from bika.lims.utils import t
    mapping = {"number": 5}
    final = t(_("There are ${number} things.", mapping=mapping)

To prevent encoding errors, all string values in the mapping parameter passed to the MessageFactory must be encoded as UTF-8!

    title = to_utf8(value)
    message = _("The title is: ${title}", mapping={"title":title})

***

### Translating strings in javascript code

Bika uses [jarn.jsi18n](https://github.com/ggozad/jarn.jsi18n) to translate values in javascript files.

> Strings from Javascript files need to be added in bika-custom.pot - they will not be recognised automatically by i18ndude's scanner.

The jsi18n function works just like a zope MessageFactory, substituting mapping keys with gettext-style ${} syntax.  Don't use regular string concatenation to create these strings, the message translation will not work:

    window.jarn.i18n.loadCatalog("bika");
    var _ = window.jarn.i18n.MessageFactory("bika");   
    var mapping = {number: 5};
    var final = _("There are ${number} things.", mapping);

### Dates and Times

There are four msgid's in the that need to be overridden to set the Date and Time formats: These messages most probably should be kept translated uniformly across the plone and bika domains.

```
#. The variables used here are the same as used in the strftime formating.
#. Supported are ${A}, ${a}, ${B}, ${b}, ${H}, ${I}, ${m}, ${d}, ${M}, ${p},
#. ${S}, ${Y}, ${y}, ${Z}, each used as variable in the msgstr.
#. In english speaking countries default is: ${b} ${d}, ${Y} ${I}:${M} ${p}
msgid "date_format_long"
msgstr "${Y}-${m}-${d} ${I}:${M} ${p}"

#. The variables used here are the same as used in the strftime formating.
#. Supported are ${A}, ${a}, ${B}, ${b}, ${H}, ${I}, ${m}, ${d}, ${M}, ${p},
#. ${S}, ${Y}, ${y}, ${Z}, each used as variable in the msgstr.
#. In english speaking countries default is: ${b} ${d}, ${Y}
msgid "date_format_short"
msgstr "${Y}-${m}-${d}"

#. The variables used here are the same as used in the strftime formating.
#. Supported are ${A}, ${a}, ${B}, ${b}, ${H}, ${I}, ${m}, ${d}, ${M}, ${p},
#. ${S}, ${Y}, ${y}, ${Z}, each used as variable in the msgstr.
#. In english speaking countries default is: ${I}:${M} ${p}
#: ./TranslationServiceTool.py
msgid "time_format"
msgstr "${I}:${M} ${p}"

#. Date format used with the datepicker jqueryui plugin.
# The variables used here are **NOT** the same as the strftime formatting.  They are taken from the jqueryui datepicker widget's readme, and the available strings are: 'dd', 'mm', 'yy', '-', '/', and '.'
#. Default: "mm/dd/yy"
msgid "date_format_short_datepicker"
msgstr "yy-mm-dd"
```

### Overriding translations

Simple instructions for changing defaults or overriding translations for text strings.  This is documented lots elsewhere, but simple step-by-step instructions are also good.  If you are fixing something you think could be improved in the core translations, please consider opening a bug report or pull request instead.

##### 1: Install Plone.

You will also need to install some or all of the dependencies listed in the [Bika installation guide](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-Installation), depending on what you are going to get up to.

##### 2. Run development buildout

    bin/buildout -c develop.cfg

In Plone-4.3.2 I had to add 'ExtensionClass >= 4.1a1' to [versions], because buildout
will not automatically download versions that look like pre-releases, seemingly even
when a package depends on this version.

##### 3. Create a new python package

    cd src/
    ../bin/zopeskel basic_namespace test.package

##### 4. Create translation content

Add language folders for all languages that you want to override.

    mkdir -p test.package/test/package/locales/en/LC_MESSAGES

You should download the relevant po files from [Github](https://github.com/bikalabs/Bika-LIMS/tree/master/bika/lims/locales) and place it inside your new language folders.

> Some strings are translated in the bika domain, some strings are translated in the plone domain, and others are actually present in both, so as to be translated in different areas of the site.

You can edit the files with a normal text editor.  I use poedit:

    sudo apt-get install poedit
    poedit test.package/test/package/locales/en/LC_MESSAGES/bika.po

##### 5. Add the required zcml snippet

In `test.package/test/package/configure.zcml`:

```
<configure xmlns:i18n="http://namespaces.zope.org/i18n"
           i18n_domain="my.package">
    <i18n:registerTranslations directory="locales" />
</configure>
```

##### 6. Add test.package to the eggs, develop and zcml parts of buildout.cfg

My entire buildout.cfg looks like this so far:

    [extends]

    buildout-original.cfg

    eggs +=
        bika.lims
        test.package

    develop +=
        src/test.packagen

    zcml += 
        test.package

##### 7. Run buildout, and we're done.

    bin/buildout
    bin/plonectl restart
