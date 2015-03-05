You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Inventory Management Module
***

### Purpose of this document

This document is used to establish the scope of the project and a common understanding for coders in the 2015 Google Summer of Code to build there proposal around. 

Document Convention: Note the page is written in the present tense describing the requirement as if it is there already.

Background
While Bika LIMS already has the basics for Product and Sample References management, as well as some simple Supply Order functionality, this module has been neglected for too long.
In most cases the inventory management of lab products and reagents are managed by an external application (ERP) using Bika's JSON API, a Bika module will add better customised functionality, suitable for resource limited settings too, yet sophisticated enough to deal with best of breed requirements.

**Expected results**: Functional and integrated Bika Inventory Management

**Skills required**: [Python](http://python.org), [Plone](http://plone.org), [jQuery](http://jquery.com). Familiarity with Bika LIMS is a plus.

**Skill level**: Medium to high

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***


###Database
##Inventory categories
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
##IIs. Inventory Items
At the bottom of the DB pyramid lives the Inventory items, the individual entities the system manages 
IIs often arrive batches of say 10 items, and this is resolved via a Batch Container and its individual Items.
See Storage management: The storage locations are managed at hierarchical addresses, e.g. a shelve position, in a fridge or cabinet, in a room,  is a container with Inventory Items (IIs) in it.
##Inventory Item attributes
Include but not limited to
Title, description, supplier, ordered by, date received, date opened, CAS number, location, expiry date, lab ID, supplier catalogue  ID, hazard rating, quantity, toxicity, health effects, first aid SOP, storage conditions, disposal SOP, spill-handling procedures, Material Safety Data Sheets (MSDS), relevant images, PDF and other files. Disposal date.

The following are a summary of the objectives we'd like to achieve:

**a) Supplier Orders**

Right now, Bika LIMS allows to set-up Suppliers and Products, but we'd like to add the functionality of creating product orders. The lab manager or clerk would be able to register orders of products/reagents/whatever to a supplier. This order could be printed and eventually sent directly to the supplier contact via email. By default, after creating an order, its status should be 'pending'.

**b) Products reception and storage. Labelling**

The lab clerk will check the products received in the lab against the pending orders. The clerk should be able to receive the order (the status will change to 'received'), create the labels (barcodes) for the products and add them in the lab inventory. 

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
