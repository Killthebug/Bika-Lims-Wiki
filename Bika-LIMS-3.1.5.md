You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Changelog](https://github.com/bikalabs/Bika-LIMS/wiki/changelog) · Bika LIMS 3.1.5
***

Bika LIMS 3.1.5 is a new [minor release](https://github.com/bikalabs/Bika-LIMS/wiki/Release-cycle). Though mainly a bug-fix version of issues reported in 3.1.4 and 3.1.4.1, some small features and enhancements are included too.

**35 tickets have been closed!**

###Update instructions

Bika LIMS 3.1.5 is a minor release, and there is no need for further changes or modifications elsewhere in the Bika LIMS instance. If your instance to be updated has been customised, please review whether the customisations are affected by the update by checking the [CHANGELOG.txt file published with the release](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/3.1.5/CHANGELOG.txt).

See [Installing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst) and/or [upgrading Bika LIMS](https://github.com/bikalabs/Bika-LIMS/blob/3.1.5/INSTALL.rst)

###After you update
- Use the [users](http://lists.sourceforge.net/lists/listinfo/bika-users), [designers](https://groups.google.com/forum/?hl=en) and [developers](http://lists.sourceforge.net/lists/listinfo/bika-developers) lists or IRC (irc://freenode.net/#bika) if you have any question or feedback (free support),
- Please help us spread the word about Bika LIMS! Maybe you can write about the project on your blog, website, talk about Bika LIMS at conferences, or let your friends and colleagues know what is Bika LIMS. With your help we can grow the community!    
- To improve Bika LIMS in your language consider [contributing to translations](https://www.transifex.com/projects/p/bika-lims/),
- Log issues, feature requests, or bugs in the [Issue Tracker](http://jira.bikalabs.com/),
- Enable your Bika LIMS instance to [report bugs to us automatically](https://github.com/bikalabs/Bika-LIMS/blob/0c606e0/INSTALL.rst#log-errors-to-sentrybikalabscom).
- Contact any of the [Certified Professional Bika Service Providers](http://www.bikalims.org/support-and-service-provision) for any enquiry or to learn more about making the most of Bika LIMS.

### List of 35 tickets closed in Bika LIMS 3.1.5
- [LIMS-1082](https://jira.bikalabs.com/browse/LIMS-1082): Report Barcode. Was images for pdf/print reports etc
- [LIMS-1159](https://jira.bikalabs.com/browse/LIMS-1159): reapply fix for samplepoint visibility
- [LIMS-1325](https://jira.bikalabs.com/browse/LIMS-1325): WSTemplate loading incompatible reference analyses
- [LIMS-1333](https://jira.bikalabs.com/browse/LIMS-1333): Batch label replace with standard Plone keyword widget
- [LIMS-1335](https://jira.bikalabs.com/browse/LIMS-1335): Reference Definitions don't sort alphabetically on WS Template lay-outs
- [LIMS-1345](https://jira.bikalabs.com/browse/LIMS-1345): Analysis profiles don't sort
- [LIMS-1347](https://jira.bikalabs.com/browse/LIMS-1347): Analysis/AR background colour to be different to for Receive and To be Sampled
- [LIMS-1360](https://jira.bikalabs.com/browse/LIMS-1360): Number of analyses in ARs folder view
- [LIMS-1374](https://jira.bikalabs.com/browse/LIMS-1374): Auto label printing does not happen for an AR drop-down receive
- [LIMS-1377](https://jira.bikalabs.com/browse/LIMS-1377): Error when trying to publish after updating branch hotfix/next or develop
- [LIMS-1378](https://jira.bikalabs.com/browse/LIMS-1378): Add AR/Sample default fields to Batch
- [LIMS-1395](https://jira.bikalabs.com/browse/LIMS-1395): front page issue tracker url
- [LIMS-1402](https://jira.bikalabs.com/browse/LIMS-1402): If no date is chosen, it will never expire." not been accomplished
- [LIMS-1416](https://jira.bikalabs.com/browse/LIMS-1416): If a sample point has a default sample type the field is not pulled automatically during AR template creation
- [LIMS-1425](https://jira.bikalabs.com/browse/LIMS-1425): Verify Workflow (bika_listing) recursion
- added 'getusers' method to JSON API
- Added 'remove' method to JSON API
- Added AR 'Copy to new' action in more contexts
- Added basic handling of custom Sample Preparation Workflows
- Added decimal mark configuration for result reports
- Added help info regards to new templates creation
- Added IAcquireFieldDefaults - acquire field defaults through acquisition
- Added IATWidgetVisibility - runtime show/hide of AT edit/view widgets
- Added watermark on invalid reports
- Added watermark on provisional reports
- Alert panel when upgrades are available
- All relevant specification ranges are persisted when copying ARs or adding analyses
- Allow comma entry in numbers for e.g. German users
- Bika LIMS javascripts refactoring and optimization
- Fix ZeroDivisionError in variation calculation for DuplicateAnalysis
- Fixed spreadsheet load errors in Windows.
- Fixed template rendering errors in Windows
- JSONAPI update: always use field mutator if available
- JSONAPI: Added 'remove' and 'getusers' methods.
- Refactored ARSpecs, and added ResultsRange field to the AR