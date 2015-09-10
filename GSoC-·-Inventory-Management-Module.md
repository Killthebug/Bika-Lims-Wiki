You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Inventory Management Module
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
Note the page is written in the present tense describing the requirement as if it is there already. Lower priority items are designated as **Phase II**.

###Background
While Bika LIMS already has the basics for Product and Sample References management, as well as some simple Supply Order functionality, this module has been neglected for too long.

In most cases the inventory management of lab products and reagents are managed by an external application (ERP) using Bika's JSON API, a Bika module will add better customised functionality, suitable for resource limited settings too, yet sophisticated enough to deal with best of breed requirements.

###Entities

**Product Category**

Lab clerk and Lab manager can create Product categories based on their needs. Examples of product categories: Sampling Containers, Sampling Kits, Analysis Kits, Kit components, Printer consumables, etc.


**Product**

The Product entity represents an item that can be purchased from a supplier (vendor) at any time. The lab maintains a catalogue of products and each product includes (but not limited to) the following attributes: title, description, category, supplier, CAS number, supplier catalogue ID, hazard rating, quantity, toxicity, health effects, first aid SOP, storage conditions, disposal SOP, spill-handling procedures, Material Safety Data Sheets (MSDS), relevant images, PDF and other files.

A Product must be assigned to a Product Category.


**Product Item**

The product/inventory items are the individual entities the system manages. The product item represents a physical item of a Product purchased from a Supplier. A product/inventory item inherits all the attributes from a Product entity plus the following: order id, date received, date opened, location, expiry date, lab ID, disposal date, batch id.

Product items often arrives batches of say 10 items, and this is resolved via a Batch Container/Shipping package and its individual items.


**Supplier**

Already exists in Bika LIMS. A Supplier can have 0 or more Products assigned. A Product can only be assigned to one Supplier.


**Purchase Order**

A Purchase Order is an element that represents a list of products (and quantities) to be purchased from a Supplier. Only lab managers and lab clerks can create orders. An order must be assigned to a Supplier and once created, its default status is 'Pending' reception.

###Functional description

####a) Purchase Orders

**Status: [Resolved](https://jira.bikalabs.com/browse/LIMS-1837)**

Ordering Product Items are done integrated with Bika's current Suppliers structure, authorised users create purchase orders in the UI. Orders are printed and/or emailed to the supplier upon creation. The Orders can be printed and/or emailed at any time and its status (Pending, Dispatched, Received, Stored) will be displayed as well.

> **Phase II**
>
> Approved suppliers and their contacts. Vendor catalogue integration.


####b) Products reception and storage. Labelling

**Status: [Resolved](https://jira.bikalabs.com/browse/LIMS-1838)**

When new stock Shipment arrives, lab clerks check it in against pending orders, print barcodes labels, capture data such as expiration date, batch and item IDs. Vendor supplied labelling scanned in tracked to each
individual container. 

As requested by [ISO/IEC 17025](http://en.wikipedia.org/wiki/ISO/IEC_17025), each product must have a physical and technical evaluation before reception. The user must fill a form with the questions the lab has defined previously in Bika Setup. Upon reception, the Purchase Order status is transitioned from "Pending" or "Dispatched" to "Received", the appropriate Product Items are created, and therefore, the products stock from the inventory updated.

Products usually come packed in containers with different number of units, so the system automatically recalculates the number of individual units of product when the lab clerk receives an order.

If the stock shipment doesn't contain some of the products ordered or some of them are defective, the lab clerk can create a new order with the missing/defective items, print and/or email the supplier. The new order will refer to the previous one for tracking purposes.

####c) Storage management

**Status: [Resolved](https://jira.bikalabs.com/browse/LIMS-1839)**

When the lab clerk receives the shipments, they are stored at barcoded storage locations. The storage locations are managed at hierarchical addresses, e.g.

    a Product Item is stored
    in a container
    in a shelf position,
    in a fridge or cabinet,
    in a room.

Storage conditions for the location, e.g. 4 deg C, must match that of its the item's specification. The storage locations should be searchable for available and adjoining space.

> **Phase II**
>
> 2D Graphical presentation of freezer shelves with shelf positions linked to the items stored, and available positions indicated. Consolidation and optimisation of storage locations - 'de-fragmentation' of storage space.


####d) Stock control

**Status: [Open](https://jira.bikalabs.com/browse/LIMS-1840)**

**d.1) Dynamic Record keeping and manual reconcilliation**

The quantity of chemical used, e.g. titration volumes, are used to maintain reagent stock levels, allowing for spillage. Nevertheless, the lab manager can do an imbalance adjustment/reconcilliation of products manually. 

A Lab Technician or Lab Manager sets the Product Item (or Product Items) - or reagent(s) - taken for the analysis on results capture. The allowed Product(s) and units are defined at Analysis Service or Method level and a picklist of available Product Items is shown in the results entry form for its selection. Note the user assigns a specific Product Item to a test/analysis: he/she assigns a physical purchased product to be used on that analysis. As an example, if the Analysis Service or Method requires 10ml of "Acetone" (Product), the picklist on results entry will get populated with the available "Product Items" that fit with "Acetone", so the user assigns a physical purchased product (a Product Item) to that analysis (a "Bottle of 50ml of Acetone", with ID xxxx, Batch ID yyyy and expiration date YYYY-MM-DD that was ordered in the Order ZZZZ and received on YYYY-MM-DD).

This Analysis - Product Item relation is required for pushing all the analysis to quarantine in case of a defective batch. See f) Batch Control for detailed information.

A Lab Technician or Lab Manager can enter the amount of Product Item, say a reagent, used on results capture in accordance with the values set at Analysis Service or Method level. Note here the user enters the amount/volume of a selected Product Item: i.e. if the product is a bottle of 50ml of acetone and only 10ml are required for that Analysis Service, the remaining amount of acetone for that specific Product Item will decrease to 40ml, but this will not cause any effect to the Inventory unless the consumption of the total volume/amount of reagent.

> **Phase II**
>
> The procedure described above can also include a section for "lab made reagents/solutions". The stock control functionality will be the same as with Analysis Services, but instead of doing an analysis a lab reagent is prepared. The product description should contain all the needed materials/chemicals for its preparation. This way the stock control will work too for these items. Consider to create a new "Product type" to distinguish between purchased items and lab prepared materials. This new product type should be hierarchically below "Product type" as it will depend from purchased materials. Once the product is prepared the system should print its label, and make it available for selection (to use in Analysis Services or other reagent preparation) with all the expiration, traceability, ect. stuff that applies. To make it even more beautiful inside the product details there should be a space for instructions or preparation method.

**d.2) Low stock level alerts**

Each Product is set up with a minimum stock level at which a re-order alert is raised for lab clerks and lab managers. It is also known as 'Re-ordering level'. It is a point at which order for supply of material should be made.

Reordering level is calculated with the formula:

    Re-order level = max rate of consumption x max lead time

The alert, be it by email or on-line (portlet) offers an easy hyper-linked re-order workflow.

####e) Expiry alerts and product cancellation**

**Status: [Open](https://jira.bikalabs.com/browse/LIMS-1841)**

Most reagents and reference material have expiry dates. Lab clerk sets the expiry dates to Product Items upon reception of the purchase order. A similar alert as for low stock level is raised, based on configurable period before expiry, in the next month say.

These expired Product Items are immediately cancelled on expiry and not offered in the look-ups for selection.

> **Phase II**
>
> Stock loss alerts, detection and reporting.

####f) Batch control. Defective Batches

**Status: [Resolved](https://jira.bikalabs.com/browse/LIMS-1842)**

Lab clerk sets the supplier's batch id to Product Items upon reception of the purchase order. Product Items can be searched per batch id.

On the Worksheets and Analysis Requests where results are captured, the reagents/Product Items' batch numbers are captured too. Lab manager can set a batch as defective and list all analyses that involved a Product Item from a defective batch. Through their standard QC procedures, the lab might find that some reagent batches are defective and will decommission them. 

These defective batches are immediately taken out of stock, and not offered in LIMS look-ups for selection any further and alerts raised to all analysts and lab managers.

All analysis results that involved a defective batch of Product Items go into quarantine for retesting. Verified and Published results are invalidated as per standard Bika invalidation workflow.

If the decommissioning of the defect products or batches, results in minimum stock levels being breached, the replenishment alert and workflow kick off.

> **Phase II**
>
> Disposal, empty and expired reagents are disposed of by SOP, kept in the system.

####g) Physical stock taking

**Status: [Open](https://jira.bikalabs.com/browse/LIMS-1843)**

For maintaining accurate stock levels, the LIMS offers:

- Printable stock taking sheets, including barcodes for the Product Items and their expected storage locations.
- A layout optimised for data capturing on tablets through barcode scanning.

> **Phase II**
>
> Use some of the popular statistical stock taking formulas & methods, e.g. Random selection, ABS & VED analysis etc.

####h) Inventory reports

**Status: [Open](https://jira.bikalabs.com/browse/LIMS-1844)**

Most of all Bika Inventory management offers real-time inventory data of inventory stock levels, keeping track of where inventory items are and how much of it is available.

The LIMS generates reports listing inventory items by location, vendor, name, catalogue number and custom fields.

Other reports: Amount of Product Items used the last month or selected period, expenses forecast, How long stocks will cover lab requirement, Inventory valuation, Future demand estimates, etc.

> **Phase II**
>
> The inventory data could be used to create regulations related reports, e.g. regarding hazardous air pollutant (HAP) and volatile organic compound (VOC) usage data.

####i) Regulations

**Status: [Open](https://jira.bikalabs.com/browse/LIMS-1845)**

Good Laboratory Practice (GLP) and Food and Drug Administration (FDA) Guidelines, including 21 CFR Part 11 requirements prescribe are adhered to: audit trails for every data change including the date/time stamp, what was modified, and user name are captured.


> **Phase II**
> 
> Bika's Plone versioned document management is used to manage audits, preventive and corrective actions.
