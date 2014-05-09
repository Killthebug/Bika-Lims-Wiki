You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [Developing Bika LIMS](https://github.com/bikalabs/Bika-LIMS/wiki/Developing-Bika-LIMS) · Creating an Instrument Import Interface
***
### Introduction

The introduction of analyses results into Bika LIMS can be performed manually, but also automatically by using the results files generated directly by equipment or middleware software. An **instrument results file import interface** is a small part of code that parses and imports the results from those instrument-specific files into Bika LIMS. That prevents from results being entered manually by the user.

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

- The controller: [```bika.lims.exportimport.instruments.agilent.masshunter.quantitative.py```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/agilent/masshunter/quantitative.py)

- The template: [```bika.lims.exportimport.instruments.agilent.masshunter.quantitative_import.pt```](https://github.com/bikalabs/Bika-LIMS/blob/develop/bika/lims/exportimport/instruments/agilent/masshunter/quantitative_import.pt)

In fact, most of the import interfaces can be done easily by adding two classes (the controller and the template).

### Creating the template
TAL is template language used Plone. TAL is XML based language, which puts programming logic to XML attributes. The [TAL Reference Guide](http://www.owlfish.com/software/simpleTAL/tal-guide.html) is a good starting point to know how works. Also, you might check the [Plone's Templates Basics](http://docs.simplesconsultoria.com.br/developermanual/templates_css_and_javascripts/template_basics.html) for further information.

Even though, the templates used for instrument import forms are quite easy and basic HTML knowledge would be enough to develop your own interface. The following image shows how the template for FOSS Winescan Auto results import form looks like:

![FOSS Winescan Auto results import form](https://raw.githubusercontent.com/bikalabs/Bika-LIMS/develop/docs/screenshots/instrument_import_view.png)

The following are the basic fields an instrument import template might have:

- **File**: the input type element for the results file upload.

- **Format**: the file formats and file versions that Bika LIMS accepts for this instrument and model. If the results file specs changes in future, the new version might be added here, so for a given instrument, more than one formats will be available (i.e. CSV v0.93, CSV v1.0, CSV v1.2, etc.)

- **Analyisis Requests state**: allows the user to set if the results must only be saved if their Analysis Request has the state *Received* or *Received and to be verified*

- **Results override**: allows the user to set the rules the importer will follow if a result has already been set in the system.

- **Instrument**: allows the user to set the instrument to which the results will be linked if the file contains calibration tests (the identifiers are Reference Sample IDs).

### Creating the controller
- Generic parsers: ```InstrumentResultsFileParser``` and ```InstrumentCSVResultsFileParser```
- Generic importer: ```AnalysisResultsImporter```
[TO BE COMPLETED]

### Registering the new interface into the system
[TO BE COMPLETED]

### Testing
[TO BE COMPLETED]