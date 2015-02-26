You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Changelog](https://github.com/bikalabs/Bika-LIMS/wiki/changelog) · Bika LIMS 3.1.7
***

Bika LIMS 3.1.7 is a new [minor release](https://github.com/bikalabs/Bika-LIMS/wiki/Release-cycle). Small features and enhancements, new instrument interfaces, printable worksheets and more improvements are included, specifically to Batch management, and bugs reported in 3.1.6 fixed.

**36 tickets have been closed!**

###Thank you

[Jordi](http://github.com/xispa), [Campbell](http://github.com/rockfruit), [Pau](http://github.com/espurna), [Alex](https://github.com/zylinx), and all other code contributors, testers, feature requesters and providers of the enthusiasm that keeps everybody fired up.  Sponsorship came from [De Bortoli Wines](http://www.debortoli.com.au/), [DEOHS University of Washington](http://deohs.washington.edu/), [SANBI](http://www.sanbi.ac.za/), Oshana medical lab in Namibia, [Lab San Martin](http://www.laboratoriosanmartin.com/), and off course individual [Naralabs](http://naralabs.com/) and [Bika Lab Systems](http://bikalabs.com/) team members who continue to contribute massively on their own accounts.

Viva Open Source LIMS, Viva! [Amandla Awethu! Power to the people!](http://en.wikipedia.org/wiki/Amandla_(power))

**····8<··················································**

###Update instructions

Bika LIMS 3.1.7 is a minor release, and there is no need for further changes or modifications elsewhere in the Bika LIMS instance. If your instance to be updated has been customised, please review whether the customisations are affected by the update by checking the [CHANGELOG.txt file published with the release](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.7/CHANGELOG.txt).

See [Installing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst) and/or [upgrading Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst)

###After your upgrade
- Use the [users](http://lists.sourceforge.net/lists/listinfo/bika-users), [designers](https://groups.google.com/forum/?hl=en) and [developers](http://lists.sourceforge.net/lists/listinfo/bika-developers) lists or IRC ([irc://freenode.net/#bika](http://webchat.freenode.net?randomnick=1&channels=%23bika&uio=d4)) if you have any question or feedback (free support),
- Please help us spread the word about Bika LIMS! Maybe you can write about the project on your blog, website, talk about Bika LIMS at conferences, or let your friends and colleagues know this feature rich FOSS LIMS. With your help we can grow the community!    
- To improve Bika LIMS in your language consider [contributing to translations](https://www.transifex.com/projects/p/bika-lims/),
- Log issues, feature requests, or bugs in the [Issue Tracker](http://jira.bikalabs.com/),
- Enable your Bika LIMS instance to [report bugs to us automatically](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst#log-errors-to-sentrybikalabscom).
- Contact any of the [Certified Professional Bika Service Providers](http://www.bikalims.org/support-and-service-provision) with any specific questions or to learn more about making the most of Bika LIMS.

### Tickets closed

**Improvements**
- [LIMS-1199](https://jira.bikalabs.com/browse/LIMS-1199) Worksheet totals in WS lists
- [LIMS-1365](https://jira.bikalabs.com/browse/LIMS-1365) Batch search parameters on Work sheets/Work sheets insides Batches
- [LIMS-1520](https://jira.bikalabs.com/browse/LIMS-1520) Allow invalidation of verified Analysis Requests
- [LIMS-1536](https://jira.bikalabs.com/browse/LIMS-1536) [Add] button to allow quick additions in reference widget
- [LIMS-1539](https://jira.bikalabs.com/browse/LIMS-1539) Printable Worksheets. In both Analysis Request by row or column orientations
- [LIMS-1623](https://jira.bikalabs.com/browse/LIMS-1623) Implement bika-frontpage as a BrowserView

**New Instrument interfaces**
- [LIMS-1569](https://jira.bikalabs.com/browse/LIMS-1569) Beckman Coulter Access 2
- [LIMS-1570](https://jira.bikalabs.com/browse/LIMS-1570) Roche Cobas Taqman 48
- [LIMS-1571](https://jira.bikalabs.com/browse/LIMS-1571) Sysmex XS-1000i
- [LIMS-1572](https://jira.bikalabs.com/browse/LIMS-1572) Sysmex XS-500i
- [LIMS-1575](https://jira.bikalabs.com/browse/LIMS-1575) Thermo Arena 20XT
- [LIMS-1603](https://jira.bikalabs.com/browse/LIMS-1603) Life Technologies Qubit
- [LIMS-1604](https://jira.bikalabs.com/browse/LIMS-1604) BioDrop uLite
- [LIMS-1605](https://jira.bikalabs.com/browse/LIMS-1605) Tescan TIMA

**Fixes**
- [LIMS-257](https://jira.bikalabs.com/browse/LIMS-257) Set Blank and Warning icons in Reference Sample main view
- [LIMS-1266](https://jira.bikalabs.com/browse/LIMS-1266) Sampling date format error
- [LIMS-1423](https://jira.bikalabs.com/browse/LIMS-1423) Save details when Analysis Request workflow action kicked off
- [LIMS-1428](https://jira.bikalabs.com/browse/LIMS-1428) Results capturing after receiving a sample with Sampling Workflow enabled
- [LIMS-1517](https://jira.bikalabs.com/browse/LIMS-1517) Storage field tag translation
- [LIMS-1518](https://jira.bikalabs.com/browse/LIMS-1518) Storage Location table
- [LIMS-1524](https://jira.bikalabs.com/browse/LIMS-1524) Populate invalidation email variables
- [LIMS-1527](https://jira.bikalabs.com/browse/LIMS-1527) CC Contact on Analysis Request view (edit) offers all contacts in system
- [LIMS-1540](https://jira.bikalabs.com/browse/LIMS-1540) When accent characters are used in a "Sample Type" name, it is not possible to create a new Analysis Request
- [LIMS-1574](https://jira.bikalabs.com/browse/LIMS-1574) Fixed Analysis Request and Analysis attachments
- [LIMS-1587](https://jira.bikalabs.com/browse/LIMS-1587) Better support for extension of custom sample labels
- [LIMS-1594](https://jira.bikalabs.com/browse/LIMS-1594) Re-ordered tabs on Client home page
- [LIMS-1614](https://jira.bikalabs.com/browse/LIMS-1614) Error when selecting Analysis Administration Tab after receiving a sample with Sampling Workflow enabled
- [LIMS-1617](https://jira.bikalabs.com/browse/LIMS-1617) Error with bin/test
- [LIMS-1622](https://jira.bikalabs.com/browse/LIMS-1622) Version Check does not correctly check cache
- [LIMS-1624](https://jira.bikalabs.com/browse/LIMS-1624) Import default test.xlsx fails
- [LIMS-1636](https://jira.bikalabs.com/browse/LIMS-1636) Batch Sample View crash
- [LIMS-1670](https://jira.bikalabs.com/browse/LIMS-1670) Fixed windows incompatibility in TAL (referencewidget.pt)
- [LIMS-1687](https://jira.bikalabs.com/browse/LIMS-1687) Added option to select landing page for clients in configuration registry
- [LIMS-1688](https://jira.bikalabs.com/browse/LIMS-1688) After Analysis Request invalidation, Analysis Requests list throws an error
- [LIMS-1689](https://jira.bikalabs.com/browse/LIMS-1689) Error while creating a new invoice batch
- [LIMS-1690](https://jira.bikalabs.com/browse/LIMS-1690) Typo. Instrument page

### Download
- [Bika LIMS 3.1.7 at PyPI](https://pypi.python.org/pypi/bika.lims/3.1.7)
- [Python egg (Python 2.6)](https://pypi.python.org/packages/2.6/b/bika.lims/bika.lims-3.1.6-py2.6.egg#md5=a46b3832c2bd13efd20cfac7de3604b8) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=a46b3832c2bd13efd20cfac7de3604b8))
- [Python egg (Python 2.7)](https://pypi.python.org/packages/2.7/b/bika.lims/bika.lims-3.1.6-py2.7.egg#md5=135edc42993a798c6c19cc41376e2dcf) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=135edc42993a798c6c19cc41376e2dcf))
- [Source code (zip)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.7.zip)
- [Source code (tar.gz)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.7.tar.gz)