You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Developing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/wiki/Developing-Bika-LIMS) · Creating an Instrument Import Interface
***
### Table of Contents
1. [Introduction](#introduction)
2. [File formats and structure](#file-formats-and-structure)
3. [Architecture concepts](#architecture-concepts)
4. [Creating the template](#creating-the-template)
5. [Creating the parser](#creating-the-parser)
6. [Creating the controller](#creating-the-controller)
7. [Registering the new interface into the system](#registering-the-new-interface-into-the-system)
8. [Share your interface](#share-your-interface)

***

### Introduction

The introduction of analyses results into Bika LIMS can be performed manually, but also automatically by using the results files generated directly by equipment or middleware software. An **instrument results file import interface** is a small part of code that parses and imports the results from those instrument-specific files into Bika LIMS. That prevents from results being entered manually by the user.

Check the list of [instruments currently supported](https://github.com/bikalabs/Bika-LIMS/wiki/Supported-instrument-interfaces)

### File formats and structure

The results file format and structure depends on each instrument, therefore each import interface must be developed specifically for each result file. Comma Separated Values (CSV) file format is one of the most common formats currently used, as well as Tab Separated Values (TSV) file format.

Excerpt of a CSV file from WineScan Auto:

```
Sample Id,,,Ash,Ca,Ethanol,VolatileAcid,Info,ResultType,BottleType,Remark
AR-01177-01,,,0.9905,22.31,14.11,2.95,Mean,Normal,Normal,
Sample Id,,,Ash,Ca,Ethanol,VolatileAcid,Info,ResultType,BottleType,Remark
AR-01175-01,,,0.9936,31.49,14.38,2.7,Mean,Normal,Normal,
[...]
```

Excerpt of a TSV file from Dionex instrument:

```
Sample	Sample Name	Time 	Amount 	Amount 	Amount 	Amount 	Amount 	Amount 	Amount 
No.			µg/sample	µg/sample	µg/sample	µg/sample	µg/sample	µg/sample	µg/sample
			Fluoruro	Cloruro	Nitrito	Bromuro	Nitrato	Fosfato	Sulfato
			CD_1	CD_1	CD_1	CD_1	CD_1	CD_1	CD_1
1	Detection	04.09.08 12:16	0.5826	0.9929	1.0386  	1.0164	1.1478	0.9466	3.3877
2	STD. Low	04.09.08 12:36	1.1564	2.0817	2.2007  	2.1899	2.2176	2.1413	2.3749
3	STD. Mid	04.09.08 12:56	3.6420	7.1617	7.2616  	7.1709	7.2191	7.1919	7.0979
4	STD. High	04.09.08 13:16	6.3377	12.8123	12.7240  	12.7828	12.7352	12.7869	12.5525
5	Blank	04.09.08 13:36	n.a.	0.0460	n.a.	n.a.	n.a.	n.a.	n.a.
6	6167	05.27.08 17:25	0.1124	9.1143	0.5806  	n.a.	0.7345	n.a.	1.3049
[...]
```

Bika LIMS makes easy to develop parsers for those file types thanks to built-in generic parsers. The development of specific-instrument interfaces is only a matter of taking advantage of Object Oriented polymorphism.

### Architecture concepts

All the instrument import logic and related classes are under the [```bika.lims.exportimport.instruments```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments) package. Inside this package, [```__init__.py```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/__init__.py) and [```resultsimport.py```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py) are the most important classes involved in parsing and importing the results. Besides, a hierarchy of packages following the ```<manufacturer>.<instrument_model>``` rule are placed here and contains both the controller classes for the instrument-specific results import forms and the form template. As an example, the import interface for Agilent's Masshunter quantitative results file comprises the following classes:

- [The template](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/agilent/masshunter/quantitative_import.pt)
- [The parser](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/agilent/masshunter/quantitative.py#L101)
- [The controller](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/agilent/masshunter/quantitative.py)

In fact, most of the import interfaces can be done easily by adding these three classes.

### Creating the template
TAL is the template language used Plone. TAL is an XML-based language, which adds programming logic to XML attributes. The [TAL Reference Guide](http://www.owlfish.com/software/simpleTAL/tal-guide.html) is a good starting point to know how it works. Also, you might check [Plone's Templates Basics](http://docs.simplesconsultoria.com.br/developermanual/templates_css_and_javascripts/template_basics.html) for further information.

Even though it may seem complex, the templates used for instrument import forms are quite easy and basic HTML knowledge would be enough to develop your own interface. The following image shows what the template for the FOSS Winescan Auto results import form looks like:

![FOSS Winescan Auto results import form](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/develop/docs/screenshots/instrument_import_view.png)

The following are the basic fields an instrument import template might have:

- **File**: the input type element for the results file upload.

- **Format**: the file formats and file versions that Bika LIMS accepts for this instrument and model. If the results file specs change in future, the new version might be added here, so for a given instrument, more than one format will be available (i.e. CSV v0.93, CSV v1.0, CSV v1.2, etc.)

- **Analyisis Requests state**: allows the user to set if the results must only be saved if their Analysis Request has the state *Received* or *Received and to be verified*

- **Results override**: allows the user to set the rules the importer will follow if a result has already been set in the system.

- **Instrument**: allows the user to set the instrument to which the results will be linked if the file contains calibration tests (the identifiers are Reference Sample IDs).

### Creating the parser
The parser is the class responsible for parsing the results file. Any parser must inherit from [```InstrumentResultsFileParser```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py#L14) or from any of its child classes and override its methods. [```InstrumentCSVResultsFileParser```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py#L187) is the most commonly used class to be inherited from, which is a child from ```InstrumentFileParser```. As the name indicates, this class provides methods to read and parse CSV-type files.

In most cases, overriding the method [```_parseline(self, line)```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py#L220) would be enough for a fully functional importer interface:

```python
def _parseline(self, line):
    """ Parses a line from the input CSV file and populates rawresults
        (look at getRawResults comment)
        returns -1 if critical error found and parser must end
        returns the number of lines to be jumped in next read. If 0, the
        parser reads the next line as usual
    """
    raise NotImplementedError
```

The method will be called by the parent class every time a new line is reached. The logic to be implemented in this method must achieve the following:

**a) Split the line, retrieve the data and fill a key,value dictionary**.

  As an example, for a line

```
    QC13-0002-001.d,D2,274638,0.0212,0.914,1.9531,98.19,,
```
  with header
```
    Data File,Compound,ISTD Resp,Resp Ratio, Final Conc,Exp Conc,Accuracy,Remarks
```
  a dictionary might be created as follows:
```
    {'D2': {'DefaultResult': 'Final Conc',
            'Remarks': '',
            'Resp': '5816',
            'ISTD Resp': '274638',
            'Resp Ratio': '0.0212',
            'Final Conc': '0.9145',
            'Exp Conc': '1.9531',
            'Accuracy': '98.19' }}
```
  Where ```D2``` is an Analysis Service Keyword and the keys from the inner dictionary are the result and values to be saved for that Analysis. By the default, the importer will use the field specified by the 'DefaultResult' key as the default value for the analyses. Nevertheless, the importer will look for the rest of values to find matches with interim fields (if exist for that Analysis Service).

**b) Add the previous dictionary to 'rawresults'** by using the method [```_addRawResult(self, resid, values={}, override=False)```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py#L57):

```
  self._addRawResult('QC13-0002-001', rawdict, False)
```

where:
  - *resid*: is the Identifier of the Analysis Request, Sample, Reference Sample, etc.
  - *rawdict*: is the dictionary of values created in the first step
  - *override*: action to take if another rawresult has been already added for the same resid and analysis.


**c) Return an integer value**:
  - 0: If the parser should follow the next line.
  - 1..n: If the parser should jump n lines before calling _parseline again.
  - -1: If the parser failed due to a critical error. The import will be aborted.


Excerpt of [```WinescanCSVParser```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/foss/winescan/__init__.py)
```python
def _parseline(self, line):
    # Sample Id,,,Ash,Ca,Ethanol,ReducingSugar,VolatileAcid,TotalAcid
    if line.startswith('Sample Id'):
        self.currentheader = [token.strip() for token in line.split(',')]
        return 0

    if self.currentheader:
        # AR-01177-01,,,0.9905,22.31,14.11,2.95,0.25,5.11,3.54,3.26,-0.36
        splitted = [token.strip() for token in line.split(',')]
        resid = splitted[0]
        if not resid:
            self.err(_("No Sample ID found, line %s") % self._numline)
            self.currentHeader = None
            return 0

        duplicated = []
        values = {}
        remarks = ''
        for idx, result in enumerate(splitted):
            if idx == 0:
                continue

            if len(self.currentheader) <= idx:
                self.err(_("Orphan value in column %s, line %s") \
                         % (str(idx + 1), self._numline))
                continue

            keyword = self.currentheader[idx]

            if not result and not keyword:
                continue

            if result and not keyword:
                self.err(_("Orphan value in column %s, line %s") \
                         % (str(idx + 1), self._numline))
                continue

            # Allow Bika to manage the Remark as an analysis Remark instead
            # of a regular result. Remarks field will be set for all
            # Analysis keywords.
            if keyword == 'Remark':
                remarks = result
                continue

            if not result:
                self.warn(_("Empty result for %s, column %s, line %s") % \
                          (keyword, str(idx + 1), self._numline))

            if keyword in values.keys():
                self.err(_("Duplicated result for '%s', line %s") \
                         % (keyword, self._numline))
                duplicated.append(keyword)
                continue

            values[keyword] = {'DefaultResult': keyword,
                               'Remarks': remarks,
                               keyword: result}

        # Remove duplicated results
        outvals = {key: value for key, value in values.items() \
                   if key not in duplicated}

        # add result
        self._addRawResult(resid, outvals, True)
        self.currentHeader = None
        return 0

    self.err(_("No header found"))
    return 0
```

You may notice that in this case, some additional data checks are performed: detection of duplicate records, empty results, orphan values, etc. The [```Logger```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/logger.py) top-level class in the hierarchy also provides some useful methods:

```
 err(self, msg, numline=None, line=None)
 warn(self, msg, numline=None, line=None) 
 log(self, msg, numline=None, line=None)
```
where:
- *msg*: the message to be displayed
- *numline*: the affected number of line from the file being parsed
- *line*: the line string itself

All this information is displayed in the web page after the submission is done.

#### Where should the parser be placed?
As mentioned above a package following the rule ```bika.lims.exportimport.instruments.<manufacturer>.<model>``` should be created. The parser classes are usually defined inside the ```__init__.py``` file from that package. See [```WinescanCSVParser```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/foss/winescan/__init__.py) to see what it looks like.

### Creating the controller
The controller manages the submission of the template, acquires the request values, initializes the parser to be used for the specified file and executes the importer.

The controller consists of an ```Import(context, request)``` method. This is the method that will be fired when the user submits the form. Besides, a global variable called ```title``` must be declared. Its value will be used on the 'Instruments' selection list for the specific form being rendered on the fly.

Below, the main logic to be implemented in the controller:

```python
from bika.lims.exportimport.instruments.resultsimport import AnalysisResultsImporter
import json
import traceback

# Declare the title to be used in the 'Instrument' selector for the
# template being rendered on the fly
title = "<Manufacturer> - <Model> - Your awesome importer interface"

def Import(context, request):
    # Some logic here to retrieve the request values and the inputfile
    # [....]
    infile = request.form['file-to-submit']

    # Creates the specific-parser
    parser = YourOwnFileParser(infile)

    # Fire the import process
    importer = AnalysisResultsImporter(parser, context)
    try:
        importer.process()
    except:
        tbex = traceback.format_exc()
    errors = importer.errors
    logs = importer.logs
    warns = importer.warns
    if tbex:
        errors.append(tbex)

    # Display the results
    results = {'errors': errors, 'log': logs, 'warns': warns}
    return json.dumps(results)
```

And thats all!

The ```importer.process()``` does all the work: it runs the parser and saves the data retrieved into Bika LIMS.

Notice that you can also use an specific Importer instead of the generic [```AnalysisResultsImporter```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/resultsimport.py#L230), but it's not recommended unless you need very special features not already provided by this. 

### Registering the new interface into the system
The last step is to register the interface in the system, for which you only need to add the path to your new package in [```bika.lims.exportimport.instruments.__init__.py```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/__init__.py):

```python
from <manufacturer>.<model> import <your_awesome_importer_interface>

__all__ = ['generic.xml',
           'agilent.masshunter.quantitative',
           'foss.fiastar.fiastar',
           'foss.winescan.auto',
           'foss.winescan.ft120',
           'thermoscientific.gallery.Ts9861x',
           '<manufacturer>.<model>.<your_awesome_importer_interface>']

```
### Share your interface
Bika LIMS is an Open Source project and your contributions are welcome. Do a [pull request](https://github.com/bikalabs/Bika-LIMS/pulls) of your code and benefit all the community of users. If you don't know how to do this, you can either send your code to the [developers list](http://lists.sourceforge.net/lists/listinfo/bika-developers).