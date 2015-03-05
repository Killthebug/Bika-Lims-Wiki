You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Froid. Free Open Instrument Middleware
***

### Summary
**Description**

The Open Instrument Middleware is an idea of a free-standing instrument server to easily interface any laboratory instrument to any laboratory software. 
  
Once established, **Froid will be vendor neutral to attract as many interfaces possible**, all open sourced on the AGLP for protection. The current Bika LIMS interface module was designed with this in mind and can be used as a basis.

See the initial top level design at [Froid. Free Open Instrument Middleware. Fluffy and dated Functional Specification](https://drive.google.com/file/d/0B-ZQviPheiU-NkpHelZFeU5iTXc/view?usp=sharing)

Froid is a working title. Propose your own! Thus far other candidates are: Omar - Open Middleware, Opium Open Instrument Middleware 

The main objectives are:

- The software will be well documented and easy to maintain to encourage LIMS developers and instrument manufacturers to add interfaces to the system
- Resolve serial RS232 communications with laboratory instruments
- Froid must communicate bi-directionally with LIMS using a singular easy to understand file format,  converting them to and from all possible instrument communication protocols.
- For better load balancing, Froid must be free-standing to allow for it to be deployed on its own hardware in high volume labs
- Froid's management and maintenance interface is to be web based for easy remote access
- The Froid format must be defined as broadly as possible, to include imagery and data blobs, and with enough redundancy to take care of future interface developments
- Communication between FROID and the LIMS itself must be in line with standards-compliant secure communication protocols offering comprehensive data encryption.

**Expected results**: Free standing instrument server that can be maintained and integrated easily

**Skills required**: [Python](http://python.org),  understanding of RS232  and serial communications

**Skill level**: Medium to high

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***

## Functional overview

TODO