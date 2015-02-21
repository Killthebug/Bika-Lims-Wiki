You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Changelog](https://github.com/bikalabs/Bika-LIMS/wiki/changelog) · Bika LIMS 3.1.6
***

Bika LIMS 3.1.6 is a new [minor release](https://github.com/bikalabs/Bika-LIMS/wiki/Release-cycle). Though mainly a bug-fix version of issues reported in 3.1.5, some small features and enhancements are included too.

**34 tickets have been closed!**

###Thank you

[Jordi](http://github.com/xispa), [Campbell](http://github.com/rockfruit), [Pau](http://github.com/espurna), [Alex](https://github.com/zylinx), and all other code contributors, testers, feature requesters and providers of the enthusiasm that keeps everybody fired up.  Sponsorship came from [De Bortoli Wines](http://www.debortoli.com.au/), [DEOHS University of Washington](http://deohs.washington.edu/), [SANBI](http://www.sanbi.ac.za/), Oshana and Alpha health labs in Namibia, [Lab San Martin](http://www.laboratoriosanmartin.com/), and off course individual [Naralabs](http://naralabs.com/) and [Bika Lab Systems](http://bikalabs.com/) team members who continue to contribute massively on their own accounts.

###Update instructions

Bika LIMS 3.1.6 is a minor release, and there is no need for further changes or modifications elsewhere in the Bika LIMS instance. If your instance to be updated has been customised, please review whether the customisations are affected by the update by checking the [CHANGELOG.txt file published with the release](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.6/CHANGELOG.txt).

See [Installing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst) and/or [upgrading Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst)

###After you update
- Use the [users](http://lists.sourceforge.net/lists/listinfo/bika-users), [designers](https://groups.google.com/forum/?hl=en) and [developers](http://lists.sourceforge.net/lists/listinfo/bika-developers) lists or IRC ([irc://freenode.net/#bika](http://webchat.freenode.net?randomnick=1&channels=%23bika&uio=d4)) if you have any question or feedback (free support),
- Please help us spread the word about Bika LIMS! Maybe you can write about the project on your blog, website, talk about Bika LIMS at conferences, or let your friends and colleagues know what is Bika LIMS. With your help we can grow the community!    
- To improve Bika LIMS in your language consider [contributing to translations](https://www.transifex.com/projects/p/bika-lims/),
- Log issues, feature requests, or bugs in the [Issue Tracker](http://jira.bikalabs.com/),
- Enable your Bika LIMS instance to [report bugs to us automatically](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst#log-errors-to-sentrybikalabscom).
- Contact any of the [Certified Professional Bika Service Providers](http://www.bikalims.org/support-and-service-provision) for any enquiry or to learn more about making the most of Bika LIMS.

### List of 34 tickets closed in Bika LIMS 3.1.6
- [LIMS-1530](https://jira.bikalabs.com/browse/LIMS-1530): Scrambled Analysis Category order in Published Results
- [LIMS-1529](https://jira.bikalabs.com/browse/LIMS-1529): Error while inserting an AR with container-based partitioning is required
- [LIMS-1460](https://jira.bikalabs.com/browse/LIMS-1460): Additional field in AR for comments or results interpretation
- [LIMS-1441](https://jira.bikalabs.com/browse/LIMS-1441): An error message related to partitions unit is shown when selecting analysis during AR creation
- [LIMS-1470](https://jira.bikalabs.com/browse/LIMS-1470): AS Setup. File attachment field tag is missing
- [LIMS-1422](https://jira.bikalabs.com/browse/LIMS-1422): Results doesn't display yes/no once verified but 1 or 0
- [LIMS-1486](https://jira.bikalabs.com/browse/LIMS-1486): Typos in instrument messages
- [LIMS-1498](https://jira.bikalabs.com/browse/LIMS-1498): Published Results not Showing for Logged Clients
- [LIMS-1445](https://jira.bikalabs.com/browse/LIMS-1445): Scientific names should be written in italics in published reports
- [LIMS-1389](https://jira.bikalabs.com/browse/LIMS-1389): Units in results publishing should allow super(sub)script format, for example in cm2 or m3
- [LIMS-1500](https://jira.bikalabs.com/browse/LIMS-1500): Alere Pima's Instrument Interfice
- [LIMS-1457](https://jira.bikalabs.com/browse/LIMS-1457): Exponential notation in published AR pdf should be formatted like a×10^b instead of ae^+b
- [LIMS-1334](https://jira.bikalabs.com/browse/LIMS-1334): Calculate result precision from Uncertainty value
- [LIMS-1446](https://jira.bikalabs.com/browse/LIMS-1446): After retracting a published AR the Sample gets cancelled
- [LIMS-1390](https://jira.bikalabs.com/browse/LIMS-1390): More workflow for Batches
- [LIMS-1378](https://jira.bikalabs.com/browse/LIMS-1378): Bulking up Batches
- [LIMS-1479](https://jira.bikalabs.com/browse/LIMS-1479): new-version and upgrade-steps should be python viewlets
- [LIMS-1362](https://jira.bikalabs.com/browse/LIMS-1362): File attachment uploads to Batches
- [LIMS-1404](https://jira.bikalabs.com/browse/LIMS-1404): New Batch attributes (and their integration with existing ones on Batch views)
- [LIMS-1467](https://jira.bikalabs.com/browse/LIMS-1467): Sample Point Lookup doesn't work on AR modify
- [LIMS-1363](https://jira.bikalabs.com/browse/LIMS-1363): Batches per Client
- [LIMS-1405](https://jira.bikalabs.com/browse/LIMS-1405): New Sample and AR attributes
- [LIMS-1085](https://jira.bikalabs.com/browse/LIMS-1085): Allow Clients to add Attachments to ARs
- [LIMS-1444](https://jira.bikalabs.com/browse/LIMS-1444): In AR published report accredited analysis services are not marked as accredited
- [LIMS-1443](https://jira.bikalabs.com/browse/LIMS-1443): In published reports the publishing date is not shown in the pdf
- [LIMS-1420](https://jira.bikalabs.com/browse/LIMS-1420): Status filter is not kept after moving to next page
- [LIMS-1442](https://jira.bikalabs.com/browse/LIMS-1442): Sample Type is not filtred by Sample Point
- [LIMS-1448](https://jira.bikalabs.com/browse/LIMS-1448): Reports: when you click on "Analysis turnaround time" displays others
- [LIMS-1440](https://jira.bikalabs.com/browse/LIMS-1440): Error when trying to publish with analysis from different categories
- [LIMS-1459](https://jira.bikalabs.com/browse/LIMS-1459): Error when checking instrument validity in manage_results
- [LIMS-1430](https://jira.bikalabs.com/browse/LIMS-1430): Create an AR from batch allows you to introduce a non existent Client and Contacts don't work properly
- After modifying analysis Category, reindex category name and UID for all subordinate analyses
- Setup data import improvements and fixes
- Simplify installation with a custom Plone overview and add site

### Download
- [Bika LIMS 3.1.6 at PyPI](https://pypi.python.org/pypi/bika.lims/3.1.6)
- [Python egg (Python 2.6)](https://pypi.python.org/packages/2.6/b/bika.lims/bika.lims-3.1.6-py2.6.egg#md5=a46b3832c2bd13efd20cfac7de3604b8) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=a46b3832c2bd13efd20cfac7de3604b8))
- [Python egg (Python 2.7)](https://pypi.python.org/packages/2.7/b/bika.lims/bika.lims-3.1.6-py2.7.egg#md5=135edc42993a798c6c19cc41376e2dcf) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=135edc42993a798c6c19cc41376e2dcf))
- [Source code (zip)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.6.zip)
- [Source code (tar.gz)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.6.tar.gz)