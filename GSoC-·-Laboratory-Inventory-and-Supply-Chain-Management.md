You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Inventory Management Module
***

### Purpose of this document

This document is used to establish the scope of the project and a common understanding for coders in the 2015 Google Summer of Code to build there proposal around. 

Document Convention: Note the page is written in the present tense describing the requirement as if it is there already.

### Background
While Bika LIMS already has the basics for Product and Sample References management, as well as some simple Supply Order functionality, this module has been neglected for too long.
In most cases the inventory management of lab products and reagents are managed by an external application (ERP) using Bika's JSON API, a Bika module will add better customised functionality, suitable for resource limited settings too, yet sophisticated enough to deal with best of breed requirements.

**Expected results**: Functional and integrated Bika Inventory Management

**Skills required**: [Python](http://python.org), [Plone](http://plone.org), [jQuery](http://jquery.com). Familiarity with Bika LIMS is a plus.

**Skill level**: Medium to high

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***


# Database
## Inventory categories
These should types of  inventory items should are managed:
Reagents
Reference Samples
Sampling containers
Sampling Kits
Analysis Kits
Kit components
Sampling containers – bottles, filters, zip-locks, etc.
Printer consumables – Ink, labels
Lab coats, safety glasses, shoes
First aid kit,  fire extinguishers
Cleaning materials
Canteen
## IIs. Inventory Items
At the bottom of the DB pyramid lives the Inventory items, the individual entities the system manages 
IIs often arrive batches of say 10 items, and this is resolved via a Batch Container and its individual Items.
See Storage management: The storage locations are managed at hierarchical addresses, e.g. a shelve position, in a fridge or cabinet, in a room,  is a container with Inventory Items (IIs) in it.
## Inventory Item attributes
Include but not limited to
Title, description, supplier, ordered by, date received, date opened, CAS number, location, expiry date, lab ID, supplier catalogue  ID, hazard rating, quantity, toxicity, health effects, first aid SOP, storage conditions, disposal SOP, spill-handling procedures, Material Safety Data Sheets (MSDS), relevant images, PDF and other files. Disposal date.

# Workflow
## Suppliers and Purchases
Ordering Inventory Items are done integrated with Bika's current Suppliers structure, authorised users create purchase orders, POs, in the UI. 
Orders are printed or emailed with status Pending, followed by e.g. Dispatched, Received, Stored. 
Courier details and links to online tracking in the order online is a must have.
**Phase II**
Approved suppliers and their contracts
Vendor catalogue integration
## Receipt and storage. Labeling
When new stock Shipment arrives, lab clerks check it in against pending orders, print barcodes labels, capture data such as expiration date, batch and item IDs. Vendor supplied labelling scanned in tracked to each individual container.
**Phase II**
Incomplete orders
Returned orders
## IM. Inventory Management
Tracking and maintaining sustainable inventory levels, monitor batches or individual items.
### Dynamic Record keeping
The quantity of chemical used, e.g. titration volumes, are used to maintain reagent stock levels, allowing for spillage.
Labs might also want to capture location of use, user and cost centre.
### Minimum stock level alerts
Each item is set up with a minimum stock level at which a re-order alert is raised for lab clerks and lab managers. It is also known as ‘Re-ordering level’. It is a point at which order for supply of material should be made: 
Reordering level is calculated with the formula:
Re-order level = max rate of consumption x max lead time
The alert, be it by email or on-line offers and easy hyper-linked re-order workflow.
### Expiry alerts
Most reagents and reference material have expiry dates. A  similar alert as for low stock level is raised, based on configurable period before expiry, in the next month say.
These expired or defect batches are immediately cancelled on expiry and not offered in the look-ups .
### Phase II. Stock loss alerts
Detection and reporting.
### Inventory QC
On the worksheets and and ARs where results are captured, the reagents Batch number is captured too.
Through their standard QC procedures, the lab might find that some reagent batches are defective. All analysis results that involved a defective batch of reagents go into quarantine. 
Verified and Published results are invalidated as per standard Bika invalidation workflow
### Phase II. Disposal
Empty and expired reagents are disposed of by SOP, kept in the system.
### Physical stock taking
For maintaining accurate stock levels, the LIMS offers:
Printable stock taking sheets, including barcodes for the item and storage location.
A layout optimised for data capturing on tablets with barcode scanners
**Phase II**
Use some of the popular statistical stock taking formulas & methods, e.g.  Random selection, ABS & VED analysis etc.
## Storage management
This function is shared with biobanking's Sample inventory management, which can go quite a distance...
When the lab clerk receives the shipments, they are stored at barcoded storage locations. 
The storage locations are managed at hierarchical addresses, e.g. 
an Inventory Item is stored
in a container
in a shelve position, 
in a fridge or cabinet, 
in a room. 
Storage conditions for the location, e.g. 4 deg C, must match that of its the item's specification.
The storage locations should be searchable for empty spaces.
**Phase II**
Graphical presentation of freezer drawers with shelve positions linked to the items in there.
Consolidation and optimisation of available space.
## Reporting
The inventory data are used to create regulations related reports, e.g. regarding hazardous air pollutant (HAP) and volatile organic compound (VOC) usage data.
Most of all, it offers real-time inventory data of actual or estimated stock levels, keeping track of where inventory items are and how much available, 
The LIMS generates reports listing inventory items by location, vendor, name, catalogue number and custom fields, 
Others report Amount of II used the last month or selected period, expenses forecast, How long stocks will cover lab requirement, Inventory valuation, Future demand estimates, etc.
# Regulations
Good Laboratory Practice (GLP) and Food and Drug Administration (FDA) Guidelines, including 21 CFR Part 11 requirements prescribe: An audit trail for every data change including the date/time stamp, what was modified, and who made the modifications. 
Phase II
Bika's Plone versioned document management is used to manage audits, preventative and corrective actions