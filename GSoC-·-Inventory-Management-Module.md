You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Inventory Management Module and Supply Chain Management
***

### Summary

While Bika LIMS already has the basics for Product and Sample References management, as well as some simple Supply Order functionality, this module has been neglected for too long.
In most cases the inventory management of lab products and reagents are managed by an external application (ERP) using Bika's JSON API, a Bika module will add better customised functionality, suitable for resource limited settings too, yet sophisticated enough to deal with best of breed requirements.

**Expected results**: Functional and integrated Bika Inventory Management

**Skills required**: [Python](http://python.org), [Plone](http://plone.org), [jQuery](http://jquery.com). Familiarity with Bika LIMS is a plus.

**Skill level**: Medium to high

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***

###Purpose of this document

This document is used to establish the scope of the project and a common understanding for coders in the 2015 Google Summer of Code to build there proposal around.

###Convention
Note the page is written in the present tense describing the requirement as if it is there already.

###Background
While Bika LIMS already has the basics for Product and Sample References management, as well as some simple Supply Order functionality, this module has been neglected for too long.

In most cases the inventory management of lab products and reagents are managed by an external application (ERP) using Bika's JSON API, a Bika module will add better customised functionality, suitable for resource limited settings too, yet sophisticated enough to deal with best of breed requirements.

###Entities

**Product Category**

Lab clerk and Lab manager can create Product categories based on their needs. Examples of product categories: Reference Samples, Sampling Containers, Sampling Kits, Analysis Kits, Kit components, Sampling Kits, Analysis Kits, Kit components, Sampling containers, Printer consumables, etc.


**Product**

The Product entity represents an item that can be purchased to a supplier (vendor) at any time. The lab maintains a catalogue of products and every product includes (but not limited to) the following attributes: title, description, category, supplier, CAS number, supplier catalogue ID, hazard rating, quantity, toxicity, health effects, first aid SOP, storage conditions, disposal SOP, spill-handling procedures, Material Safety Data Sheets (MSDS), relevant images, PDF and other files.

A Product must be assigned to a Product Category.


**Product Item**

The product/inventory items are the individual entities the system manages. The product item represents a physical item of a Product purchased to a Supplier. A product/inventory item inherits all the attributes from a Product entity plus the following: order id, date received, date opened, location, expiry date, lab ID, disposal date, batch id.

Product items often arrives batches of say 10 items, and this is resolved via a Batch Container/Shipping package and its individual items.


**Supplier**

Already exists in Bika LIMS. A Supplier can have 0 or more Products assigned. A Product can only be assigned to one Supplier.


**Purchase Order**

A Purchase Order is an element that represents a list of products (and quantities) to be purchased to a Supplier. Only lab managers and lab clerks can create orders. An order must to be assigned to a Supplier and once created, its default status is 'Pending' (of reception).

###Functional description

**a) Purchase Orders**

Ordering Product Items are done integrated with Bika's current Suppliers structure, authorised users create purchase orders in the UI. Orders are printed and/or emailed to the supplier upon creation. The Orders can be printed and/or emailed at any time and its status (Pending, Dispatched, Received, Stored) will be displayed as well.

*Further development*: Approved supplied and their contracts. Vendor catalogue integration.


**b) Products reception and storage. Labelling**

When new stock Shipment arrives, lab clerks check it in against pending orders, print barcodes labels, capture data such as expiration date, batch and item IDs. Vendor supplied labelling scanned in tracked to each
individual container. Upon reception, the Purchase Order status is transitioned from "Pending" or "Dispatched" to "Received", the appropriate Product Items are created, and therefore, the products stock from the inventory updated.

Products usually come packed in containers with different number of units, so the system automatically recalculates the number of individual units of product when the lab clerk receives an order.

If the stock shipment doesn't contain some of the products ordered or some of them are defective, the lab clerk can create a new order with the missing/defective items, print and/or email the supplier. The new order will refer to the previous one for tracking purposes.

**c) Storage management**

When the lab clerk receives the shipments, they are stored at barcoded storage locations. The storage locations are managed at hierarchical addresses, e.g.

    a Product Item is stored
    in a container
    in a shelve position,
    in a fridge or cabinet,
    in a room.

Storage conditions for the location, e.g. 4 deg C, must match that of its the item's specification. The storage locations should be searchable for empty spaces.

*Further development*: Graphical presentation of freezer drawers with shelve positions linked to the items in there. Consolidation and optimisation of available space.

**e) Stock control**

e.1) Dynamic Record keeping and manual reconcilliation

The quantity of chemical used, e.g. titration volumes, are used to maintain reagent stock levels, allowing for spillage. Nevertheless, the lab manager can do an imbalance adjustment/reconcilliation of products manually. 

e.2) Minimum stock level alerts

Each Product is set up with a minimum stock level at which a re-order alert is raised for lab clerks and lab managers. It is also known as 'Re-ordering level'. It is a point at which order for supply of material should be made.

Reordering level is calculated with the formula:

    Re-order level = max rate of consumption x max lead time

The alert, be it by email or on-line (portlet) offers an easy hyper-linked re-order workflow.

**f) Expiry alerts and product cancellation**

Most reagents and reference material have expiry dates. Lab clerk sets the expiry dates to Product Items upon reception of the purchase order. A similar alert as for low stock level is raised, based on configurable period before expiry, in the next month say.

These expired Product Items are immediately cancelled on expiry and not offered in the look-ups.

*Further development*: Stock loss alerts, detection and reporting.

**f) Batch control**

Lab clerk sets the supplier's batch id to Product Items upon reception of the purchase order. Product Items can be searched per batch id.

Lab manager can set a batch as deffective and list all analyses that involved a Product Item from a deffective batch. All the analysis results go into quarantine. Moreover, the defective Product Items are immediately cancelled and not offered in the look-ups.

*Further development*: Disposal, empty and expired reagents are disposed of by SOP, kept in the system.

**g) Physical stock taking**

For maintaining accurate stock levels, the LIMS offers:

- Printable stock taking sheets, including barcodes for the item and storage location.
- A layout optimised for data capturing on tablets with barcode scanners

*Further development*: Use some of the popular statistical stock taking formulas & methods, e.g. Random selection, ABS & VED analysis etc.

**i) Inventory reports**

The inventory data are used to create regulations related reports, e.g. regarding hazardous air pollutant (HAP) and volatile organic compound (VOC) usage data.

Most of all, it offers real-time inventory data of actual or estimated stock levels, keeping track of where inventory items are and how much available.

The LIMS generates reports listing inventory items by location, vendor, name, catalogue number and custom fields.

Other reports: Amount of Product Items used the last month or selected period, expenses forecast, How long stocks will cover lab requirement, Inventory valuation, Future demand estimates, etc.

**j) Regulations**

Good Laboratory Practice (GLP) and Food and Drug Administration (FDA) Guidelines, including 21 CFR Part 11 requirements prescribe: An audit trail for every data change including the date/time stamp, what was modified,
and who made the modifications.

*Further development*: Bika's Plone versioned document management is used to manage audits, preventive and corrective actions.

