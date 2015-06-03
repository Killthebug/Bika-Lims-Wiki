You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · User Documentation Bika · Supported Instrument Interfaces
***
### Table of Contents
1. [Introduction](#introduction)
2. [Supported instruments](#supported-instruments)
  1. [Bika LIMS 3.1.8]()
  2. [Bika LIMS 3.1.7]()
  3. [Bika LIMS 3.1.6]()
3. [Coming up / Under way](#Coming-up-/-under-way)

***

### Introduction

The introduction of analyses results into Bika LIMS can be performed manually, but also automatically by using the results files generated directly by equipment or middleware software. An **instrument results file import interface** is a small part of code that parses and imports the results from those instrument-specific files into Bika LIMS. That prevents from results being entered manually by the user.

### Supported instruments
#### **Bika LIMS 3.1.8**
released on 2015-06-03 · [Read more](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-3.1.8)

Supported instruments in Bika LIMS 3.1.7 and:

- [Horiba JY ICP](https://jira.bikalabs.com/browse/LIMS-1805)
- [Seal Analytical AQ2](https://jira.bikalabs.com/browse/LIMS-1806)
- [Scil Vet abc Plus](https://jira.bikalabs.com/browse/LIMS-1771)
- [Thermo Multiskan™ GO Microplate Spectrophotometer](https://jira.bikalabs.com/browse/LIMS-1773)
- [VetScan VS2](https://jira.bikalabs.com/browse/LIMS-1772)

#### **Bika LIMS 3.1.7**
released on 2015-02-26 · [Read more](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-3.1.7)

Supported instruments in Bika LIMS 3.1.6 and:

- [Beckman Coulter Access 2](https://jira.bikalabs.com/browse/LIMS-1569)
- [BioDrop &micro;Lite](https://jira.bikalabs.com/browse/LIMS-1604)
- [Life technologies Qubit&reg;](https://jira.bikalabs.com/browse/LIMS-1603)
- [Roche Cobas Taqman 48](https://jira.bikalabs.com/browse/LIMS-1570)
- [Sysmex XS-1000i](https://jira.bikalabs.com/browse/LIMS-1571)
- [Sysmex XS-500i](https://jira.bikalabs.com/browse/LIMS-1572)
- [TESCAN TIMA](https://jira.bikalabs.com/browse/LIMS-1605)
- [Thermo Arena 20XT](https://jira.bikalabs.com/browse/LIMS-1575)


#### **Bika LIMS 3.1.6**
released on 2014-12-17 · [Read more](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-3.1.6)

- Agilent's Masshunter Quantitative. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/agilent/masshunter/quantitative.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/agilent/masshunter/samples)
- Alere PIMA Beads. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/alere/pima/beads.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/alere/pima/samples)
- Alere PIMA CD4. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/alere/pima/cd4.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/alere/pima/samples)
- FOSS Fiastar. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/fiastar/fiastar.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/fiastar)
- FOSS Winescan FT120. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/winescan/ft120.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/winescan/samples)
- FOSS Winescan Auto. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/winescan/__init__.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/foss/winescan/samples)
- PANalytical Omnia Axios XRF. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/panalytical/omnia/__init__.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/panalytical/omnia/samples)
- Thermoscientific Gallery Ts9861. [interface](https://github.com/bikalabs/Bika-LIMS/blob/hotfix/3.1.7/bika/lims/exportimport/instruments/thermoscientific/gallery/Ts9861x.py) · [samples](https://github.com/bikalabs/Bika-LIMS/tree/hotfix/3.1.7/bika/lims/exportimport/instruments/thermoscientific/gallery/samples)

### Coming up / Under way
- [Agilent 6140 UHPLC-LCMS](https://jira.bikalabs.com/browse/HEALTH-259)
- [Agilent 7700 ICP-MCS](https://jira.bikalabs.com/browse/LIMS-1588)
- [BD FACSCalibur](https://jira.bikalabs.com/browse/HEALTH-235)
- [Becton Dickinson MGIT 960](https://jira.bikalabs.com/browse/HEALTH-234)
- [Bruker S1 Titan Handheld XRF](https://jira.bikalabs.com/browse/LIMS-1577)
- [Roche Cobas C 111](https://jira.bikalabs.com/browse/HEALTH-236)
- [Roche Cobas C 311](https://jira.bikalabs.com/browse/HEALTH-237)
- [Roche Cobas E 411](https://jira.bikalabs.com/browse/HEALTH-238)
- [Thermo iCAP 6200 ICP-AES](https://jira.bikalabs.com/browse/LIMS-1589)
- [Thermo Scientific Phadia 100](https://jira.bikalabs.com/browse/HEALTH-229)
- [Varion AA 240FS](https://jira.bikalabs.com/browse/LIMS-1433)

**Want your instrument to be included here?**

[Code your own instrument interface](https://github.com/bikalabs/Bika-LIMS/wiki/creating-an-instrument-import-interface) and [share it](https://github.com/bikalabs/Bika-LIMS/wiki/Bika-LIMS-Developer-Guidelines), [add it to the tracker](https://jira.bikalabs.com/browse/LIMS-1573) or [select the feature/enhancement you want to achieve and become a sponsor](https://jira.bikalabs.com/issues/?jql=project%20in%20%28HEALTH%2C%20LIMS%29%20AND%20status%20in%20%28Open%2C%20Reopened%29%20AND%20%20type%20in%20%28Improvement%2C%20%22New%20Feature%22%29%20AND%20%28fixversion%20is%20EMPTY%29%20ORDER%20BY%20Rank%20ASC%2C%20priority%20DESC%2C%20updated%20DESC).