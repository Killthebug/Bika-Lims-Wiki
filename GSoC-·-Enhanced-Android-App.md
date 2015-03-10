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

### Purpose of this document

This document is used to discuss and establish the scope of the project and a common understanding for coders in the 2015 Google Summer of Code to build their proposals around.

It is live and will morph as more user and technical feedback are incorporated.

### Document Convention

Note the page is written in the present tense describing the requirement as if it is there already. 

**Lower priority items are designated as [Phase II](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-%C2%B7-Enhanced-Android-App#phase-ii-health-samp) and [III](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-%C2%B7-Enhanced-Android-App#phase-iii-phantasies)**.

### Background

Like the instrument server Froid, Samp the Android app will be open and easy to interface any LIMS or LIS to, be Free and Open Source, protected on the AGPL, with the intention to attract both FOSS and Proprietary contributions.

Native Android is the best fit. And it needs a name - working title is [**Samp**](http://en.wikipedia.org/wiki/Samp) -, and it might just stick. Thank you universe. 

Samp resolves data capturing in remote locations where connectivity is not guaranteed, server syncing when in reach. Some sampling areas in health care, geochemistry and environmental management are really remote.

The complexity brought about by adding Patients and their data will see that addressed in a follow-up project after initial LIMS requirements are fulfilled. See [Health Samp](https://github.com/bikalabs/Bika-LIMS/wiki/GSoC-%C2%B7-Enhanced-Android-App#phase-ii-health-samp) the paragraph.

Samp thus applies in disciplines like Water Quality and Environmental Management, Geological and other Surveys at first, for capturing sampling and field results for samples of water, soil, air, drill cores etc.

Including but not limited to: Images and GPS coordinates from the phone; barcodes for Container and Kit IDs, field results such as temperature, pH, turbidity, humidity etc.

### Server-Android comms

[Bika's JSON API](https://github.com/bikalabs/Bika-LIMS/wiki/BIKA-JSON-API
) handles text already and for binary data [JSON-Binary functionality](http://stackoverflow.com/questions/1443158/binary-data-in-json-string-something-better-than-base64) can be added either side.

### Functional overview

**a) Server download**

Data to assist Sampling are downloaded to the device (phone, tablet, etc.) before field trips:

- Packing list - Pre-labelled containers and kits
- Sampling rounds including pre-created ARs, Sample point list
- Sampling Instructions
- GPS way-point for navigation integration

Only by authorised users - designated sampler and device.

**b) Label printing**

On-site WiFi/Bluetooth printing to portable printers. Most containers and kits will be pre-labelled at the lab, but not all, and new ones might be required.

**c) Sampling**

Google Maps or OpenStreetMap integration assists samplers navigating to sample points.

After scanning a Container or Kit ID, Sampling Round or AR barcode or Field Analysis Worksheet, Samp redirects to the object's view for data capturing.

Photos can be captured, say of dry or polluted rivers as Sample Point conditions, including remarks to each image.

Samp captures GPS Coords, including Elevation & Map Projection, of where samples are taken.

**d) Field analyses**

Samp allows authorised users to capture field analysis results, e.g. Water temperature, pH, turbidity, Conductivity, etc. and Kit results where applicable, as well as free text remarks.

Field analysis Worksheets for 10" devices will be handy.

Users authorised to do so, should also be able to add Sampling Points, Samples and ARs in the field. This functionality already exists in the current Bika App running on the [Motorola Xoom](http://en.wikipedia.org/wiki/Motorola_Xoom).


**e) Server upload**

Only by authorised users - designated sampler and device only. When the sampler returns to base, he/she'll have all the captured data on the device. Samp connects and load the data to the server and is done.

The data is automatically deleted from the device after a configurable period of time after a successful upload or download.


***

### Phase II. Health Samp

The real potential of Samp lies in expanding it into health care and resource limited settings, e.g. remote clinics where situations can be as acute as seeing samples and data shipped on memory cards to labs to allow the device and printer to stay on-site.

Health Samp adds Patients, maybe Doctors and Clinical Cases too, and that needs more security to protect Patient privacy.

Biobank Sampling is another use case with the accent on positive patient identification and, importantly, Patient Consent confirmation.

Veterinary use is another, fighting the dreaded Flying Swine flu in the Australian outback say.

e-Medicine use cases like home diagnosis for 1st world patients should not be excluded,

**a) Regulations**

Patient Privacy needs to be protected at all times as prescribed by ISO 15189 and CLIA, and data transfers to be done per industry protocol, e.g. HL7.

**b) Patient IDs**

Patients who lost their IDs can be identified, if not from alternative IDs such as drivers licenses, from images on the device DB: head and shoulder photos, but also of unique features such as tattoos, ethnic markings and scars.

These patients can be re-issued with (temporary) barcoded IDs. Users with sufficient authorisation, should also be able to create new Patients.

**c) Field results. Symptoms**

Data captured, ibnlt: Patient ID scans, results such as Temperature and Blood pressure, and from Kits.

In settings with very low doctor to patient rations, information that can assist them on their Cases should also be captured, e.g.

- Patient Condition: Weight, Pregnant Y/N, Hours fasting
- Symptoms identifiable by the care provider doing the sampling
- Whether Patients responded to messages to report for sampling, they might be sick at home

***

### Phase III. Phantasies

Server Data Imports could be explored to include functionality for syncing duplicate Patients say, offering the user options off the database to take care of typos etc, as well as integrity checks on binary data.

The most exiting actually would be to use the device for advanced sampling, integrating Bluetooth devices but also using the camera with macro lenses and filters.

Doctors claim video can help them diagnose stroke symptoms say, keep this option open, but it is more suitable for 1 st world, fast connectivity and hires video.