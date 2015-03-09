You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · [GSoC 2015](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-2015) · Enhanced Android App
***

### Summary

Bika LIMS has some lab forms running on Android for labs without enough desk space for bigger hardware. There is also demand for a standalone app to use in remote sampling locations, e.g. health care, which can then be synced with on-line servers when the app gets in range. 

It should capture sampling point / patient details, take photos and GPS coordinates, and record results for field analyses.

With this in mind, the following are the modules to be implemented:

- Filtered listings
- Data capture
- Server sync
- Search via bar-code scanning
- More on-line functionality

The app is designed to mimic the structure and functionality of Bika LIMS where possible.

This is not a requirement; in future, this app will be detached from Bika LIMS, so code design should take this into account allowing adapters to be registered for item listings, schema discovery, create, update, and delete methods.  Bika labs must provide a set of these adapters for bikalabs/bika.* packages.

Existing code can be downloaded from:

**Expected results**: A functional app to capture remote data and sync it with Bika servers.

**Skills required**: Android SDK, HTML, [Python](http://python.org), [Plone](http://plone.org), [jQuery](http://www.jquery.com), CSS. Familiarity with Bika LIMS is a plus.

**Skill level**: High

**Mentors**: [J. Puiggené](http://github.com/xispa), [C. McKellar-Basset](http://github.com/rockfruit)

***

###Functional overview
