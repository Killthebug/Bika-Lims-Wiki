You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Changelog](https://github.com/bikalabs/Bika-LIMS/wiki/changelog) · Bika LIMS 3.1.9
***

##Bika LIMS 3.1.9 will debut on October 8th

***

**Bika LIMS 3.1.9 (aka BL319) is the new current stable release of Bika LIMS**. 

BL319 includes a lot of new features and beautiful improvements, as well as bug-fixes that have been submitted since 3.1.8 was released. 

Some of the new functionalities included in BL319 are Sampling Rounds management, a new Dashboard for Lab Managers and Administrators, improved previews for results reports, new instrument import interfaces, stickers improvements, pricing per analysis profiles, transposed view for worksheets and much more. 

[GSoC functionalities](https://jira.bikalabs.com/issues/?jql=labels%20%3D%20GSoC-15) haven't been included in this release. Will be in the next release after some, hint hint, burnishing, testing and maturation. We'll make a bigger whooha again then. 

**[Fifty tickets have been closed!](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-3.1.9#tickets-closed)**

###Thank you

[lemoene](https://www.linkedin.com/in/lemoene), [Jordi](http://github.com/xispa), [Campbell](http://github.com/rockfruit), [Pau](http://github.com/espurna), [Alex](https://github.com/zylinx), [Jayadeep](https://github.com/jayadeepk), [Aman](https://github.com/Ammy2), [Chandan](https://github.com/chandan5), [Abhiram](https://github.com/abhi12ravi), [Leo](https://github.com/leonardorojass) all other code contributors, testers, feature requesters and providers of the enthusiasm that keeps everybody fired up.

Sponsorship was provided by [Equigerminal](http://equigerminal.org/), [Metron Geochemistry](http://metronlaboratory.co.za/), [BBK](http://bbk.co.za/), [DEOHS University of Washington](http://deohs.washington.edu/), [Lab San Martin](http://www.laboratoriosanmartin.com/), and of course individual [Naralabs](http://naralabs.com/) and [Bika Lab Systems](http://bikalabs.com/) team members who continue to contribute massively on their own accounts.

***

###Before your upgrade

![](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.9/bika/lims/browser/images/warning.png)  **From versions prior to 3.1.8.2**

If your version is prior to 3.1.8.2, read the ["Before your upgrade" from BL318 announcement](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-3.1.8#before-your-upgrade) first!.

![](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.9/bika/lims/browser/images/warning.png) **Remember to [make a backup](http://docs.plone.org/manage/deploying/backup.html)**

###Upgrade instructions

There is no need for further changes or modifications elsewhere in the Bika LIMS instance. If your instance to be updated has been customized, please review whether the customizations are affected by the upgrade through the [CHANGELOG.txt file published with the release](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.9/docs/CHANGELOG.txt).

See [Installing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/3.1.9/docs/INSTALL.rst) and/or [upgrading Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/3.1.9/docs/INSTALL.rst)

###After your upgrade
- Use the [users](http://lists.sourceforge.net/lists/listinfo/bika-users), [designers](https://groups.google.com/forum/?hl=en) and [developers](http://lists.sourceforge.net/lists/listinfo/bika-developers) lists or IRC ([irc://freenode.net/#bika](http://webchat.freenode.net?randomnick=1&channels=%23bika&uio=d4)) if you have any question or feedback (free support),
- Please help us spread the word about Bika LIMS! Maybe you can write about the project on your blog, website, talk about Bika LIMS at conferences, or let your friends and colleagues know this feature rich FOSS LIMS. With your help we can grow the community!    
- To improve Bika LIMS in your language consider [contributing to translations](https://www.transifex.com/projects/p/bika-lims/),
- Log issues, feature requests, or bugs in the [Issue Tracker](http://jira.bikalabs.com/),
- Enable your Bika LIMS instance to [report bugs to us automatically](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst#log-errors-to-sentrybikalabscom).
- Contact any of the [Certified Professional Bika Service Providers](http://www.bikalims.org/support-and-service-provision) with specific questions or to learn more about making the most of Bika LIMS.

***

### F.A.Q
- Portlets error:

They are supposed to be solved in LIMS 3.1.8.2. So If you have this error trace, probably you are using a new Plone version (4.3.6 or 4.3.7) with an old BikaLIMS version (< 3.1.8.2)

- The upgrade step is taking several minutes to finish:

This upgrade step will take some minutes because all Departments, Analysis Request Templates and Analysis Request Templates will be re-indexed to portal_catalog.

- Dashboard or other functionalities aren't working as expected:

Remember that this new feature have lots of new JavaScript functionalities, maybe a Ctrl+F5 will be needed.

### Tickets closed
**Improvements**

- [LIMS-1543](https://jira.bikalabs.com/browse/LIMS-1543): Add "Security Seal Intact Y/N" checkbox for partition container
- [LIMS-1544](https://jira.bikalabs.com/browse/LIMS-1544): Add "File attachment" field on Sample Point
- [LIMS-1949](https://jira.bikalabs.com/browse/LIMS-1949): Enviromental conditions
- [LIMS-1549](https://jira.bikalabs.com/browse/LIMS-1549): Sampling Round Templates privileges and permissions
- [LIMS-1564](https://jira.bikalabs.com/browse/LIMS-1564): Cancelling a Sampling Round
- [LIMS-2020](https://jira.bikalabs.com/browse/LIMS-2020): Add Sampling Round - Department not available for selection
- [LIMS-1545](https://jira.bikalabs.com/browse/LIMS-1545): Add "Composite Y/N" checkbox on AR Template
- [LIMS-1547](https://jira.bikalabs.com/browse/LIMS-1547): AR Templates tab inside Sampling Round Template
- [LIMS-1561](https://jira.bikalabs.com/browse/LIMS-1561): Editing a Sampling Round
- [LIMS-1558](https://jira.bikalabs.com/browse/LIMS-1558): Creating Sampling Rounds
- [LIMS-1312](https://jira.bikalabs.com/browse/LIMS-1312): Transposed Worksheet view, ARs in columns
- [LIMS-1760](https://jira.bikalabs.com/browse/LIMS-1760): Customised AR Import spreadsheets (refactored, support importing to Batch)
- [LIMS-1548](https://jira.bikalabs.com/browse/LIMS-1548): Client-specific Sampling Round Templates
- [LIMS-1546](https://jira.bikalabs.com/browse/LIMS-1546): Sampling Round Template Creation and Edit view
- [LIMS-1934](https://jira.bikalabs.com/browse/LIMS-1934): Hyperlinks in invoices
- [LIMS-1943](https://jira.bikalabs.com/browse/LIMS-1943): Stickers preview and custom stickers templates support
- [LIMS-1855](https://jira.bikalabs.com/browse/LIMS-1855): Small Sticker layout. QR-code capabilities
- [LIMS-1627](https://jira.bikalabs.com/browse/LIMS-1627): Pricing per Analysis Profile
- [LIMS-1867](https://jira.bikalabs.com/browse/LIMS-1867): Auto-header, auto-footer and auto-pagination in results reports
- [LIMS-1743](https://jira.bikalabs.com/browse/LIMS-1743): Reports: ISO (A4) or ANSI (letter) pdf report size
- [LIMS-1695](https://jira.bikalabs.com/browse/LIMS-1695): Invoice export function missing
- New System Dashboard for LabManagers and Admins

**New Instrument Interfaces**

- [LIMS-1818](https://jira.bikalabs.com/browse/LIMS-1818): Instrument Interface. Eltra CS-2000
- [LIMS-1817](https://jira.bikalabs.com/browse/LIMS-1817): Instrument Interface. Rigaku Supermini XRF


**Fixes**

- [LIMS-2049](https://jira.bikalabs.com/browse/LIMS-2049): Displaying lists doesn't work as expected in 319
- [LIMS-1875](https://jira.bikalabs.com/browse/LIMS-1875): Able to deactivate instruments and reference samples without logging in
- [LIMS-2049](https://jira.bikalabs.com/browse/LIMS-2049): Displaying lists doesn't work as expected in 319
- [LIMS-1908](https://jira.bikalabs.com/browse/LIMS-1908): Navigation tree order
- [LIMS-1965](https://jira.bikalabs.com/browse/LIMS-1965): Modified default navtree order for new installations
- [LIMS-1987](https://jira.bikalabs.com/browse/LIMS-1987): AR Invoice tab should not be shown if pricing is toggled off
- [LIMS-1523](https://jira.bikalabs.com/browse/LIMS-1523): Site Error when transitioning AR from 'Manage Analyses' or 'Log' tab
- [LIMS-1970](https://jira.bikalabs.com/browse/LIMS-1970): Analyses with AR Specifications not displayed properly in AR Add form
- [LIMS-1969](https://jira.bikalabs.com/browse/LIMS-1969): AR Add error when "Categorise analysis services" is disabled
- [LIMS-1397](https://jira.bikalabs.com/browse/LIMS-1397): Fix Client Title accessor to prevent catalog error when data is imported
- [LIMS-1996](https://jira.bikalabs.com/browse/LIMS-1996): On new system with no instrument data is difficult to get going.
- [LIMS-2005](https://jira.bikalabs.com/browse/LIMS-2005): Click on Validations tab of Instruments it give error
- [LIMS-1806](https://jira.bikalabs.com/browse/LIMS-1806): Instrument Interface. AQ2. Seal Analytical - Error
- [LIMS-2002](https://jira.bikalabs.com/browse/LIMS-2002): Error creating Analysis Requests from batch.
- [LIMS-1996](https://jira.bikalabs.com/browse/LIMS-1996): On new system with no instrument data it is difficult to get going. The warnings could be confusing
- [LIMS-1944](https://jira.bikalabs.com/browse/LIMS-1944): Prevent concurrent form submissions from clobbering each other's results
- [LIMS-1930](https://jira.bikalabs.com/browse/LIMS-1930): AssertionError: Having an orphan size, higher than batch size is undefined
- [LIMS-1959](https://jira.bikalabs.com/browse/LIMS-1959): Not possible to create an AR
- [LIMS-1956](https://jira.bikalabs.com/browse/LIMS-1956): Error upgrading to 319
- [HEALTH-279](https://jira.bikalabs.com/browse/HEALTH-279): AS IDs to be near top of page. Columns in AS list
- [LIMS-1625](https://jira.bikalabs.com/browse/LIMS-1625): Instrument tab titles and headers do not correspond
- [LIMS-1924](https://jira.bikalabs.com/browse/LIMS-1924): Instrument tab very miss-titled. Internal Calibration Tests
- [LIMS-1922](https://jira.bikalabs.com/browse/LIMS-1922): Instrument out of date typo and improvement
- [HEALTH-175](https://jira.bikalabs.com/browse/HEALTH-175): Supplier does not resolve on Instrument view page
- [LIMS-1887](https://jira.bikalabs.com/browse/LIMS-1887): uniquefield validator doesn't work properly
- [LIMS-1869](https://jira.bikalabs.com/browse/LIMS-1869): Not possible to create an Analysis Request
- [LIMS-1812](https://jira.bikalabs.com/browse/LIMS-1812): Use asynchronous requests for expanding categories in listings
- [LIMS-1811](https://jira.bikalabs.com/browse/LIMS-1811): Refactor AR Add form Javascript, and related code.
- [LIMS-2041](https://jira.bikalabs.com/browse/LIMS-2041): Resolve ${analysis_keyword) in instrument import alert.


### Download
- [Bika LIMS 3.1.9 at PyPI](https://pypi.python.org/pypi/bika.lims/3.1.9)
- [Python egg (Python 2.7)](https://pypi.python.org/packages/2.7/b/bika.lims/bika.lims-3.1.9-py2.7.egg) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=b50f2d2b8176e9124317f02e0a3a77bd))
- [Python egg (Python 2.6)](https://pypi.python.org/packages/2.6/b/bika.lims/bika.lims-3.1.9-py2.6.egg) ([md5](https://pypi.python.org/pypi?:action=show_md5&digest=76c92ac3ccbfa8ce0c2149a29c2614da))
- [Source code (zip)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.9.zip)
- [Source code (tar.gz)](https://github.com/bikalabs/Bika-LIMS/archive/3.1.9.tar.gz)
***

***

***

***
