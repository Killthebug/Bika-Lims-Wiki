You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Changelog](https://github.com/bikalabs/Bika-LIMS/wiki/changelog) · Bika LIMS 3.1.8
***

Bika LIMS 3.1.8 is a new [minor release](https://github.com/bikalabs/Bika-LIMS/wiki/Release-cycle). Small features and enhancements, [five new instrument interfaces](https://github.com/bikalabs/Bika-LIMS/wiki/Supported-instrument-interfaces#bika-lims-318), Detection Limits (LDL, UDL) support and more improvements are included and bugs reported in 3.1.7 fixed.

**Forty tickets were closed!**

###Thank you

[Jordi](http://github.com/xispa), [Campbell](http://github.com/rockfruit), [Pau](http://github.com/espurna), [Alex](https://github.com/zylinx), [Jayadeep](https://github.com/jayadeepk), [Aman](https://github.com/Ammy2), [Chandan](https://github.com/chandan5), [AbHiram](https://github.com/abhi12ravi) all other code contributors, testers, feature requesters and providers of the enthusiasm that keeps everybody fired up.

Sponsorship was provided by [Equigerminal](http://equigerminal.org/), [DEOHS University of Washington](http://deohs.washington.edu/), [SANBI](http://www.sanbi.ac.za/), [Lab San Martin](http://www.laboratoriosanmartin.com/), and off course individual [Naralabs](http://naralabs.com/) and [Bika Lab Systems](http://bikalabs.com/) team members who continue to contribute massively on their own accounts.


###Before your update

![](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/hotfix/3.1.8/bika/lims/browser/images/warning.png)  **Be sure to verify all pending/open Analysis Requests before the update**

This new release includes new functionalities regarding to Detection Limits (LDL and UDL), as well as Uncertainties management. After the upgrade, the user will not be able to set manually the detection limits in results for previously created analyses. You can change this behavoir enabling "Display a Detection Limit selector" and/or "Allow Manual Detection Limit input" options from inside Analysis Service edit view, but these changes will not take any effect on previously created analyses.

![](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/hotfix/3.1.8/bika/lims/browser/images/warning.png) **Remember to [make a backup](http://docs.plone.org/manage/deploying/backup.html)**

###Update instructions

Bika LIMS 3.1.8 is a minor release, and there is no need for further changes or modifications elsewhere in the Bika LIMS instance. If your instance to be updated has been customised, please review whether the customisations are affected by the update by checking the [CHANGELOG.txt file published with the release](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.8/docs/CHANGELOG.txt).

See [Installing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/3.1.8/docs/INSTALL.rst) and/or [upgrading Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/3.1.8/docs/INSTALL.rst)

###After your update
- Use the [users](http://lists.sourceforge.net/lists/listinfo/bika-users), [designers](https://groups.google.com/forum/?hl=en) and [developers](http://lists.sourceforge.net/lists/listinfo/bika-developers) lists or IRC ([irc://freenode.net/#bika](http://webchat.freenode.net?randomnick=1&channels=%23bika&uio=d4)) if you have any question or feedback (free support),
- Please help us spread the word about Bika LIMS! Maybe you can write about the project on your blog, website, talk about Bika LIMS at conferences, or let your friends and colleagues know this feature rich FOSS LIMS. With your help we can grow the community!    
- To improve Bika LIMS in your language consider [contributing to translations](https://www.transifex.com/projects/p/bika-lims/),
- Log issues, feature requests, or bugs in the [Issue Tracker](http://jira.bikalabs.com/),
- Enable your Bika LIMS instance to [report bugs to us automatically](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst#log-errors-to-sentrybikalabscom).
- Contact any of the [Certified Professional Bika Service Providers](http://www.bikalims.org/support-and-service-provision) with any specific questions or to learn more about making the most of Bika LIMS.

### Tickets closed

**Improvements**
- [LIMS-280](https://jira.bikalabs.com/browse/LIMS-280): System IDs starting from a specific value
- [LIMS-1819](https://jira.bikalabs.com/browse/LIMS-1819): Bika LIMS in footer, not Bika Lab Systems
- [LIMS-1808](https://jira.bikalabs.com/browse/LIMS-1808): Uncertainty calculation on DL
- [LIMS-1705](https://jira.bikalabs.com/browse/LIMS-1705): Invoices. Currency unit overcooked
- [LIMS-1507](https://jira.bikalabs.com/browse/LIMS-1507): Bika must notify why is not possible to publish an AR
- [LIMS-1811](https://jira.bikalabs.com/browse/LIMS-1811): Refactor AR Add form Javascript, and related code.
- [LIMS-1812](https://jira.bikalabs.com/browse/LIMS-1812): Use asynchronous requests for expanding categories in listings
- [WINE-44](https://jira.bikalabs.com/browse/WINE-44): Sample stickers uses Partition ID only if ShowPartitions option is enabled
- [LIMS-1779](https://jira.bikalabs.com/browse/LIMS-1779): Results report new fields and improvements
- [LIMS-1775](https://jira.bikalabs.com/browse/LIMS-1775): Allow to select LDL or UDL defaults in results with readonly mode
- [LIMS-1769](https://jira.bikalabs.com/browse/LIMS-1769): Allow to use LDL and UDL in calculations.
- [LIMS-1700](https://jira.bikalabs.com/browse/LIMS-1700): Lower and Upper Detection Limits (LDL/UDL). Allow manual input
- [LIMS-1379](https://jira.bikalabs.com/browse/LIMS-1379): Allow manual uncertainty value input
- [LIMS-1324](https://jira.bikalabs.com/browse/LIMS-1324): Allow to hide analyses in results reports
- [LIMS-1629](https://jira.bikalabs.com/browse/LIMS-1629): Pdf reports should split analysis results in different pages according to the lab department
- [LIMS-1628](https://jira.bikalabs.com/browse/LIMS-1628): There should be a results interpretation field per lab department
- Some new ID Generator's features, as the possibility to set the separator char

**New Instrument Interfaces**

- [LIMS-1773](https://jira.bikalabs.com/browse/LIMS-1773): Instrument. Thermo Fisher ELISA Spectrophotometer
- [LIMS-1806](https://jira.bikalabs.com/browse/LIMS-1806): Instrument Interface. AQ2. Seal Analytical
- [LIMS-1771](https://jira.bikalabs.com/browse/LIMS-1771): Instrument. Scil Vet abc Plus
- [LIMS-1772](https://jira.bikalabs.com/browse/LIMS-1772): Instrument. VetScan VS2
- [LIMS-1805](https://jira.bikalabs.com/browse/LIMS-1805): Instrument Interface. Horiba JY ICP

**Fixes**

- [LIMS-1697](https://jira.bikalabs.com/browse/LIMS-1697): Error updating bika.lims 317 to 318 via quickinstaller
- [LIMS-1820](https://jira.bikalabs.com/browse/LIMS-1820): QC Graphs DateTime's X-Axis not well sorted
- [LIMS-1522](https://jira.bikalabs.com/browse/LIMS-1522): Site Error adding display columns to sorted AR list
- [LIMS-1770](https://jira.bikalabs.com/browse/LIMS-1770): FIAStar import 'no header'
- [LIMS-1710](https://jira.bikalabs.com/browse/LIMS-1710): UnicodeEncode error while creating an Invoice from AR view
- [LIMS-1634](https://jira.bikalabs.com/browse/LIMS-1634): AR Import fields (ClientRef, ClientSid) not importing correctly
- [LIMS-1474](https://jira.bikalabs.com/browse/LIMS-1474): Disposed date is not shown in Sample View
- [LIMS-1754](https://jira.bikalabs.com/browse/LIMS-1754): Easy install for LIMS' add-ons was not possible
- [LIMS-1741](https://jira.bikalabs.com/browse/LIMS-1741): Fixed unwanted overlay when trying to save supply order
- [LIMS-1748](https://jira.bikalabs.com/browse/LIMS-1748): Error in adding supply order when a product has no price
- [LIMS-1745](https://jira.bikalabs.com/browse/LIMS-1745): Retracted analyses in duplicates
- [LIMS-1738](https://jira.bikalabs.com/browse/LIMS-1738): Regression. 'NoneType' object has no attribute 'getResultsRangeDict'
- [LIMS-1739](https://jira.bikalabs.com/browse/LIMS-1739): Error with results interpretation field of an AR lacking departments
- [LIMS-1740](https://jira.bikalabs.com/browse/LIMS-1740): Error when trying to view any Sample
- [LIMS-1724](https://jira.bikalabs.com/browse/LIMS-1724): Fixed missing start and end dates on reports
- [LIMS-1737](https://jira.bikalabs.com/browse/LIMS-1737): Error when adding pricelists of lab products with no volume and unit
- [LIMS-1696](https://jira.bikalabs.com/browse/LIMS-1696): Decimal mark conversion is not working with "<0,002" results type
- [LIMS-1729](https://jira.bikalabs.com/browse/LIMS-1729): Analysis Specification Not applying to Sample when Selected
- [LIMS-1507](https://jira.bikalabs.com/browse/LIMS-1507): Do not cause exception on SMTPServerDisconnect when publishing AR results.

### Download
- [Bika LIMS 3.1.8 at PyPI](https://pypi.python.org/pypi/bika.lims/3.1.8)
- [Python egg (Python 2.7)](https://pypi.python.org/packages/2.7/b/bika.lims/bika.lims-3.1.8.0-py2.7.egg#md5=673819497a4e20e836ddc5c8250552fa) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=673819497a4e20e836ddc5c8250552fa))
- [Source code (zip)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.8.zip)
- [Source code (tar.gz)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.8.tar.gz)