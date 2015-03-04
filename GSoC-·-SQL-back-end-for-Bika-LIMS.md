You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · SQL back-end for Bika LIMS
***

### Summary

**Description**

Bika LIMS uses Plone's native and powerful object-oriented database ZODB as default Database Management System and provides a JSON-API to integrate the system with others, mainly ERPs.

Some peers use relational databases such as PostgreSQL and labs would find Bika LIMS easier to integrate if it supported SQL databases. Lab managers also prefer data in tabular format they can access with their SQL query tools of choice.

**Expected results**: Bika LIMS with SQL support for PostgreSQL

**Skills required**: [Python](http://python.org), [Plone](http://plone.org), [RelStorage](https://pypi.python.org/pypi/RelStorage), [SQLAlchemy](http://www.sqlalchemy.org/), SQL ([PostgreSQL](http://www.postgresql.org/), [mySQL](http://www.mysql.com/), etc.) Familiarity with Bika LIMS is a plus.

**Skill level**: High

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***

### Additional information

Bika LIMS is a Laboratory Information Management System (http://en.wikipedia.org/wiki/Laboratory_information_management_system) and all the ideas are meant to be improvements and/or new functionalities. Bika LIMS is an add-on for Plone and takes advantage of all the features and capabilities of the later. 

The idea can be done by following two different approaches:

1. spread bits of code everywhere inside Bika LIMS, including tests
        
2. create a specific Plone-based add-on (i.e. bika.lims.sql) for Bika LIMS with tests
        
Obviously, the option 2 is the preferred one.

In both cases, the objective is to communicate Bika LIMS with SQL. This being said, there are different levels to which Bika LIMS may be connected. From lower complexity to higher complexity:

**a) SQL-mirror**

Save Bika LIMS objects in a relational database at runtime for further use by other applications, such as ERPs. Bika LIMS will still use ZODB as the main storage database, so Bika LIMS will not be affected by changes to that database.
        
**b) SQL-mirror with some additional capabilities**

The same as option a), but in this case, Bika LIMS can take advantage of this SQL database to improve some behaviors or adopt new functionalities (fast searches, etc.). Allow bi-directional communication of Bika LIMS with other systems by a CSV import tool to update objects from Bika LIMS regularly. The latter can already be done by using the Bika LIMS JSON API, but in some cases, the import-export CSV is the preferred option.
        
**c) Bi-directional SQL**

Allow to Bika LIMS use SQL as the main storage database. This is, retrieve, save and interact with objects from a relational database by using an ORM (like SQLAlchemy) instead of using ZODB. This is the hardest part and we are not sure at all if could be feasible because there is currently no solution that provides a clear translation between simple-data-types in SQL and the fields in Plone.
        
All options a), b) and c) need to be investigated and we'll be really happy if you write a proposal!

From an earlier discussion in the devs-list:

```
To sum up, here's the starting points that I'm aware of:
    https://github.com/lck/collective.contentmirror
    https://github.com/hexsprite/plock
    https://pypi.python.org/pypi/collective.lead
        
And for lightweight storage in ZODB:
    https://pypi.python.org/pypi/souper
```
        