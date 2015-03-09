You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Froid. Free Open Instrument Middleware
***

### Summary
**Description**

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

## Functional overview

TODO

```

From Froid's perspective, I'd expect to see pluggable inputs and outputs, and a core machine to route and translate between these too.  RS232 inputs/outputs, and the storage/LIMS/ERP in use should be part of this set of handlers.


 A set of generic/example input/outputs should be created during the first round, which should be simple enough to allow the set of plugins to grow immediately they are seen by capable developers.  The simplicity of the core allows more time for additional items, and allows these features to stand in singular, easily-readable code.

- Configuration/command line management
- Verbose control of logging/debug for plugin/translator development
- Translators for some common use-cases, eg; file-drops, manual CSV im/exports
- Web and/or command-line management interface.
- abstractions for dealing with serial data

At least from my own opinion, a great emphasis should be placed on code simplicity, for the purpose of easing the developers' task who will be adding modules.

Outside of high level specs, I remember Jordi sent a very well thought out email but I can't seem to find it now.. @xispa, do you remember/have that somewhere?




Not sure if the below information about supported instruments and creating an instrument interface is relevant


Instrument Interfaces
https://github.com/bikalabs/Bika-LIMS/wiki/Supported-instrument-interfaces


Creating an Instrument Import Interface
https://github.com/bikalabs/Bika-LIMS/wiki/creating-an-instrument-import-interface

```