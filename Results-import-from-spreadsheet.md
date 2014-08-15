Lab's often calculate results in spreadsheets and upload them into Bika via the import facility designed for importing instrument results. If proper version control is applied on the spreadsheet before uploading as attachments to the LIMS, the procedure seems to satisfy ISO 17025 auditors

Please see the manual, [Importing Instrument Results](http://demo.bikalabs.com/knowledge-centre/manual/bika-3-user-manual/instrument-import/importing-results)

_Bika does provide functionality to calculate results with, but if for any particular reason results need to be prepared in spreadsheets, it can be summarised in a spreadsheet in an 'instrument' format, and the instrument import functionality used for loading them_

_Labs should verify with local accreditation authorities that this method of data capture may be used but if tight version control on the spread sheets is maintained, spreadsheet data capture like this has been approved before_

The simplest file format to use for this, is the Winescan FT120's table of results per Analysis Request on rows, in columns for each test, identified by the Analysis Service's keyword, which must respond to that for them in the LIMS configuration

**[Winescan FT120 example file](https://gist.github.com/lemoene/d56a518a818fa7ea9f3a/raw/da82333ad3a351e3a47e1d151327c31557da0588/FOSSWinescanFT120.csv)**
Select the Foss Winescan FT120 in the import menu when uploading files in this format