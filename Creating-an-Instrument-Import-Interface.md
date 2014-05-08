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

**[TO BE COMPLETED]**