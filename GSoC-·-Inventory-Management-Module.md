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

The product/inventory items are the individual entities the system manages. The product item represents a physical item of a Product purchased to a Supplier. A product/inventory item inherits all the attributes from a Product entity plus the following: order id, date received, date opened, location, expiry date, lab ID, disposal date.

Product items often arrives batches of say 10 items, and this is resolved via a Batch Container/Shipping package and its individual items.


**Supplier**

Already exists in Bika LIMS. A Supplier can have 0 or more Products assigned. A Product can only be assigned to one Supplier.


**Purchase Order**

A Purchase Order is an element that represents a list of products (and quantities) to be purchased to a Supplier. Only lab managers and lab clerks can create orders. An order must to be assigned to a Supplier and once created, its default status is 'Pending' (of reception).


###Functional description

The following are a summary of the objectives we'd like to achieve:

**a) Purchase Orders**

Ordering Product Items are done integrated with Bika's current Suppliers structure, authorised users create purchase orders in the UI. Orders are printed and/or emailed to the supplier upon creation. The Orders can be printed and/or emailed at any time and its status (Pending, Dispatched, Received, Stored) will be displayed as well.


**b) Products reception and storage. Labelling**
When new stock Shipment arrives, lab clerks check it in against pending orders, print barcodes labels, capture data such as expiration date, batch and item IDs. Vendor supplied labelling scanned in tracked to each
individual container. Upon reception, the Purchase Order status is transitioned from "Pending" or "Dispatched" to "Received".

**c) Container structure for sub-products**

The hard part here is that the products usually come packed in boxes of different number of units, so there should be a way to easily recalculate the number of individual units of product in the inventory when the lab clerk receives an order.

**d) Storage management**

When the lab clerk receives the products/reagents and labels them, there should be some way to classify and store them to well-identified (and registered in Bika LIMS) places (shelves, cabinets, fridges, etc.).

**e) Stock control for reagents and products, including reference samples; min/max alerts and imbalance adjustment, 1th a month, etc.**

The stock control must allow the lab manager to do imbalance adjustment of products manually. Adding a "minimum number of units" for product would be nice, cause then we could add a portlet displaying the products for which a new supplier order must be created.

**f) Batch controls, removal of defective batches**

Imagine the lab receives a batch of 100 units of product A and starts to use them for analyses. A week later, the supplier calls the lab to notify that batch of product A was defective (i.e. vials have traces of oil inside): the lab cannot be sure that the results obtained by using any of the product A are correct and is forced to invalidate all the analyses performed. So, the lab manager has to know which analyses have been performed by using products coming from that batch. The number of batch must be linked with the identifier of the product and at the same time, the system should provide a mechanism to list (and eventually retract) all analyses in which that product was used.

**g) Expiry control, alerts and product auto-removal**

Every product might have an expiry date. There should be a mechanism to easily remove the expired products from the inventory and detect (and eventually retract) analyses for which an expired product was used. A portlet displaying the products that will expire in the following month or so would be nice too.

**h) Stock monitoring through analyses tracking**

A hard one too. A lab analysis/test may require the use of a well-defined amount of reagent/product. The idea is to set the amount of reagent/product at Method or Analysis Service (Bika LIMS objects) level and every time a result is set for that analysis, the system will update the available amount of product in the inventory. Yea.. this is one is very tricky!

**i) Inventory reports**

Number/amount of product/s used the last month, expenses forcast, etc.