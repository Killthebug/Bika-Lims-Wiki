You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Froid. Free Open Instrument Middleware
***

### Summary

The Open Instrument Middleware is an idea of a free-standing instrument server to easily interface any laboratory instrument to any laboratory software. 
  
Once established, **Froid will be vendor neutral to attract as many interfaces possible**, all open sourced on the AGPL for protection. The current Bika LIMS interface module was designed with this in mind and can be used as a basis.

See the initial top level design at [Froid. Free Open Instrument Middleware. Fluffy and dated Functional Specification](https://drive.google.com/file/d/0B-ZQviPheiU-NkpHelZFeU5iTXc/view?usp=sharing)

Froid is a working title. Propose your own! Thus far other candidates are: Omar - Open Middleware, Opium Open Instrument Middleware 

The main objectives are:

- The software will be well documented and easy to maintain to encourage LIMS developers and instrument manufacturers to add interfaces to the system
- plugin interfaces should be as simple as possible to create, and to read.
- Resolve serial RS232 communications with laboratory instruments
- Froid must communicate bi-directionally with LIMS using a singular easy to understand file format, converting them to and from all possible instrument communication protocols.
- For better load balancing, and to allow more flexible deployment, Froid must be free-standing to allow for it to be deployed on its own hardware in high volume labs
- Froid's management and maintenance interface is to be web based for easy remote access
- The Froid internal data format must be defined as broadly as possible, to include imagery and data blobs, and with enough redundancy to take care of future interface developments
- Communication between FROID and the LIMS itself must be in line with standards-compliant secure communication protocols offering comprehensive data encryption.

**Expected results**: Free standing instrument server that can be maintained and integrated easily

**Skills required**: [Python](http://python.org),  understanding of RS232  and serial communications

**Skill level**: Medium to high

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***

### Purpose of this document

This is a discussion document to establish a common understanding, in layman's language, what the Free and Open Source Instrument Server, Froid, does. It is not a technical specification, but will be used to ensure
Froid fulfils basic functions.

### Convention

Note the document is written in the present tense, describing the requirement as if it is there already. Lower priority items are designated as **Phase II and III**.

For the purpose of this document a LIMS equals a LIS. But for Patients, Clinical Cases, Patient Privacy measures and regulatory extras added to the latter.

### Overview

#### Bidirectional Instrument Interfacing

Interface-able laboratory instruments come in all shapes and variants, old and new. Many export results as CSV only that are easily imported by Froid.

Next level, they can also import Sample ID lists corresponding to positions on the sample trays loaded into them.

If the instrument automatically scan barcodes from sample vials, a Froid import interface is redundant.

Many instruments do not do CSV and have more complex live interfaces on serial RS-232 communications, bi-directionally, and Froid resolves this too.

#### Goal

Froid (Free Open Instrument Middleware) will be manufacturer independent  for easy interfacing of laboratory instruments to sample, analysis and QC management System, LIMS and LIS at first. 

Froid is vendor neutral, well documented and easy to maintain, to encourage LIMS developers and instrument manufacturers alike to add interfaces to the system.

The current Bika LIMS interface module was designed with Froid in mind and can be used as a basis:

- [Supported Instruments](https://github.com/bikalabs/Bika-LIMS/wiki/Supported-instrument-interfaces)
- [Creating an Instrument Import Interface](https://github.com/bikalabs/Bika-LIMS/wiki/creating-an-instrument-import-interface)

#### Objective

Froid communicates bi-directionally, converting data to and from instrument and LIMS sources. In easy to understand standards-compliant secure communication protocols offering data encryption, provision for images and data blobs, and enough redundancy to take care of future interface developments.

Customisation in LIMS employing Froid is kept down to adjusting IO to one of the available adapters, or adding its own.

For better load balancing, Froid must be free-standing to allow for it to be deployed on its own hardware in high volume labs. But at the same time, run on low spec PCs or Raspberry Pi.

The Froid user interface for management and maintenance is web based for easy remote access.

#### License Considerations

Ideally one would like to see all Froid implementers add their interfaces to the free and open Froid pool. It would be unfair on FOSS contributors if Big Instrument/LIMS uses Froid and available free interfaces without
opening their own? If that is the objective, the AGPL might not be enough?

#### Top level Design

![Froid logical design](http://naralabs.com/images/froid-design.png)

For instruments not connected to PCs and capable of serial communication only, Ethernet converters are recommended.

These are not only cheaper than providing PCs for all instruments, but also don't require cabling if Wi-Fi with secure encryption is chosen.

They often feature on-board web servers which make remote support possible.

**Barcode scanning**

For instruments reading Sample Information from barcodes, they are produced on the LIMS and affixed to them. The instrument reads them as they go through and there is no need for Froid.

Users may also scan the samples manually with handheld scanners into Instrument UI while loading the samples.

For these instruments, Froid's function is reduced to mono-directional interfacing.

#### Technical Development Considerations

**a) Python**

Platform independent, but development in Python and Linux. Because we know how and already told Google so.

**b) Froid LIMS Protocol**

Special attention should be paid to Froid to LIMS communication. It should follow recognised standards with eventual LIS implementation with Patient privacy considerations.

Two different parts of of the Interface needs specification:

- Communication protocol
- Data format

There is, for instance, no point in insecurely serving CSV formatted data via USB storage.

Candidates:
- [JSON API](http://jsonapi.org/)
- [JSON RPC](http://json-rpc.org/), Remote Procedure Call Protocol
- [SOAP](http://en.wikipedia.org/wiki/SOAP), Simple Object Access Protocol
- [REST](http://en.wikipedia.org/wiki/Representational_state_transfer), Representational State Transfer

**c) Serial RS232 Communications**

One of the purposes of Froid is to resolve serial RS232 communications with laboratory instruments.

These follow protocols such as [ASTM](http://www.astm.org/), [CLSI](http://clsi.org/), [DICOM](http://en.wikipedia.org/wiki/DICOM) etc. Older instrument interfaces are often poorly documented or stray from standards. Others use proprietary protocols.

Examples of popular clinical instruments for resource limited settings are with specifications attached:

- [Becton Dickinson FACSCaliber. FACSLink](https://jira.bikalabs.com/browse/HEALTH-235)
- [Becton Dickinson MGIT 960](https://jira.bikalabs.com/browse/HEALTH-234)
- [Roche Cobas C 111](https://jira.bikalabs.com/browse/HEALTH-236), [C 311](https://jira.bikalabs.com/browse/HEALTH-237), [E 411](https://jira.bikalabs.com/browse/HEALTH-238)
- [SUIT Sysmex Universal Interface](https://jira.bikalabs.com/browse/HEALTH-239)
- [Thermo Fischer ASTM protocol](https://jira.bikalabs.com/browse/HEALTH-240)

**d) CSV (Comma Separated Values)**

Many instruments simply export results as CSV or TXT files and Froid parses the native file formats and post them in LIMS configured format and protocol to the system.

For Bika's current, see wiki:

- [Instrument Interfacing](https://github.com/bikalabs/Bika-LIMS/wiki/creating-an-instrument-import-interface)
- [Supported Instruments](https://github.com/bikalabs/Bika-LIMS/wiki/Supported-instrument-interfaces)

For the quickest solution this infrastructure can be maintained but be modernised.

#### Froid. Bi-directional Instrument Interfacing

**a) Worksheets to instruments for analysis**

Users assemble worksheets of IDs for the samples to be analysed in the LIMS, and post them to the selected instrument via Froid, selecting from a drop-down menu of measurement apparatus available.

From here on it depends on how the specific instrument functions. Loosely defined, it could be 'listening' to Froid, and the data simply streamed to it. Either way, Froid writes the data to the instrument when it
is ready to receive it.

Froid converts the sample information into the correct Instrument format and dispatch it to the connected destination device using it's configured protocol.

Once the analyst is satisfied the worksheet has been successfully transferred, he/she proceeds with analysing the samples.

LIMS and Instrument manufacturers are expected to add their own channels to Froid, using any of JSON, SOAP, XML etc.

**b) Results from instruments**

Once analysis is completed, the analyst exports it from the instrument. How this is done will depend on the individual instrument. It might happen automatically or the instrument operator has to action a 'send' function.

Instruments write their results to Froid, in the case of CSV file results to a designated folder Froid polls regularly. Froid converts the data to the LIMS' format and post it per designated protocol.

Froid logs this procedure but data validation is carried out in the LIMS where issues such as unknown IDs or keywords can be highlighted.

The best way to do this is to be determined after technical analysis. It might be easy to reconcile results and sample IDs in the LIMS.

The LIMS is configured for complete QC and that should not be a Froid function.

The process should be automated as far as possible – only when keyword inconsistencies or other import issues arise should user interaction be necessary.

**c) 'Fetching' results**

Some instruments, e.g. the BD MGIT 960 in GND mode, only issue results streams in response to a command from the LIMS Froid should therefore have the ability to issue commands to the instrument, and to receive data sets from the serial interface in response to these commands.

The user should be able to request the results from the LIMS without having to log into Froid itself. This will mean a "go ask the instrument" function in the LIMS.

**d) Format**

Note that instrument imports do not have to match worksheets in the LIMS, the sample and AR IDs are used to match the results to requested analyses regardless of whether they were grouped together on worksheets or not.

Many instruments produce analysis results that are reported with a number of interim values. In the case of the different spectrometers, the result itself is reported, say in ppm, as well as any combination of: dilution factor, reagent volume, peak number, peak height, mean, standard deviation, relative standard deviation, coefficients, etc.

These are then repeated per analyte on the sample while at the same time a differing number of data fields can be reported for the sample itself, e.g. temperature, number of data points (on the curve), starting peak width, peak threshold, etc.

The analysis batch itself could have a number of attributes too, seen before: reagent ID, batch no, electrode ID, serial no, column, etc.

**e) LIMS/LIS Interfaces**

Froid strives for a single interface per LIMS to keep work on the LIMS itself down to a minimum. The above structure could feature there too, where applicable.

#### Other LIMS considerations

The results received from Froid enter standard LIMS workflow and QC.

**a) Correcting problematic imports**

The LIMS could use a 2 phased import process and alert users to problematic import files, e.g. the use of non-compliant keywords, and allows for human interaction to fix issues via an easy to use GUI before data are submitted to the DB.

**b) Referencing import files**

For traceability purposes, analysis results imported from an instrument can be linked to the raw input data files where they originated from.

These files can be uploaded centrally, in the Instrument's folder, and then referenced where required on results or worksheet views.

#### Mirth Connect

The *Swiss Army knife of healthcare integration engines*, mature and well supported Open Source [Mirth Connect](http://www.mirthcorp.com/products/mirth-connect), facilitates HL7 messaging for EMR and other medical systems. It could be worth the while to investigate
as instrument server too.

> Some instruments need low-level harvesting of data, and to receive commands to issue results, there is no streaming as such. It needs to be investigated how Mirth would do this.

Paraphrased from [Wikipedia](http://en.wikipedia.org/wiki/Mirth_(software)):

> Mirth Connect is a cross-platform HL7 interface engine that enables bi-directional sending of HL7 messages between systems and applications over multiple transports.
>
> Mirth Connect uses a channel-based architecture to connect HIT systems and allow messages to be filtered,
transformed, and routed based on user-defined rules.
>
> Channels consist of inbound and outbound connectors, filters, and transformers. Multiple filters and a chain of transformers can be associated with a channel.
>
> Endpoints are used to configure connections and their protocol details. Source connectors are used to designate the type of listener to use for incoming messages, such as TCP/IP or a web service.
>
> Destination connectors are used to designate the destination of outgoing messages, such as an application server, a JMS queue, or a database.
>
> All messages and transactions are optionally logged to an internal database.
>
> Mirth Connect can also be configured to auto-generate HL7 acknowledgement responses (ACK).
>
> Mirth Connect supports messages over a variety of protocols, ibnlt: TCP/MLLP, Database IO, ODBC, File, JMS, SFTP, HTTP, SMTP, SOAP.
>
> Open architecture allows for the easy addition of custom and legacy interfaces.

#### Running Instruments

Some LIMS implementations will also require instruments to be managed from the LIMS itself, e.g. started up and initialised, shut down, cleaned etc.

This opens an additional spec for Froid: An open, modular interface structure that allows for easy extension for both data input (commands) and output (data streams).

This could be implemented in a second phase for Froid.

#### Instrument Maintenance

Most labs will manage instrument certifications, calibrations. Maintenance in their LIMS or ERP. Froid needs to pick alerts up from instruments though and report them to listening LIMS. Required dashboard in LIMS. Else just de-activate the Instrument.
