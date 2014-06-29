You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Developing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/wiki/Developing-Bika-LIMS) · Translations and Localisation

***

### Table of Contents
1. [Translating strings in Python code](#translating-strings-in-python-code)
2. [Translating strings in javascript code](#translating-strings-in-javascript-code)
3. [Dates and Times](#dates-and-times)
4. [Overriding translations](#overriding-translations)
5. [Variable Substitutions](#variable-substitutions)

***

- The excellent [Plone documentation](http://docs.plone.org/develop/plone/i18n/internationalisation.html).

### Translating strings in Python code

Using _() to return a zope messageid is sufficient if the message is to be printed using TAL or a Plone utility such as plone_utils.addPortalMessage().  In the case of constructed strings, or views which output HTML directly to the browser, UnicodeDecodeError is often encountered.  To obtain translated values, and prevent encoding errors, the following must always be considered:

- Any string values in the mapping parameter dictionary passed to the _() message factory, must be encoded as UTF-8: `new_msg = _("The title is: ${title}", mapping={"title":to_utf8(value)})`

- TAL and friends usually pass the string through translate(), but in python scripts this must be done manually: `translated_msg = context.translate(new_msg)`.  This can be done directly with zope.i18n.translate, if it is convienient.

- The resulting value must be UTF-8!

There is a small utility `t` in the bika.lims.utils package for obtaining correctly translated strings:

```
from bika.lims.utils import t
mapping = {"string_value": "hello world"}
value = t(_("Some english string: ${string_value}", mapping=mapping))
print value  # translated according to browser headers 
```

### Translating strings in javascript code

Bika uses [jarn.jsi18n](https://github.com/ggozad/jarn.jsi18n) to translate values in javascript files.

> Strings from Javascript files need to be added in bika-custom.pot - they will not be recognised automatically by i18ndude's scanner.

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

To override translations, use the plone-manual.pot and plone-bika.pot files, respectively.  Any translation which cannot be automatically discovered during update, or translations which should override the defaults (for example, in packages that extend bika).  The references mentioned above make clear which domain contains which type of message, though there are some unavoidable duplications. 

### Variable Substitutions

Variable substitutions in messages that passed to the _() calls (in both Python and Javascript), are done with the gettext-style ${} substitution.  Don't use string concatenation, "%s" or "".format to create these strings, the message translation will not work.

Python:

    from bika.lims import bikaMessageFactory as _
    from bika.lims.utils import t
    mapping = {"number": 5}
    final = t(_("There are ${number} things.", mapping=mapping)

Javascript:

    window.jarn.i18n.loadCatalog("bika");
    var _ = window.jarn.i18n.MessageFactory("bika");   
    var mapping = {number: 5};
    var final = _("There are ${number} things.", mapping);
