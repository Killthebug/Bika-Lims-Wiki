You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Connecting Bika LIMS

***

### Table of Contents
1. [Introduction](#introduction)
2. [Authentication](#authentication)
3. [Reference Fields](#reference-fields)
4. [Querying Objects](#querying-objects)
5. [Creating Objects](#creating-objects)
6. [Updating Objects](#updating-objects)
7. [Removing Objects](#removing-objects)
8. [Workflow](#workflow)
9. [Catalogs](#catalogs)

***

### Introduction

Bika includes plone.jsonapi for reading, updating, and creating and deleting objects.  The JSON API is used internally for many AJAX requests, and also for implementing alternative interfaces.

A request is made to the '''/@@API''' url with certain parameters, and the server will return a JSON string, indicating success or failure, details about the call, and query results.

The return value will always contain the following keys:

    {
        "url": The fully qualified url that was used to locate the method,
        "_runtime": Time taken to execute request,
        "success": true if success,
        "error": true if failure
    }

Read more about the jsonapi at https://github.com/ramonski/plone.jsonapi.core

For more information on querying Plone's catalogs, read http://docs.plone.org/develop/plone/searching_and_indexing/query.html

### Authentication

Accessing the jsonapi URLs requires authentication.  The following python code logs in as the admin user.

```
top_level_url = ""
username = "admin"
password = "secret"
opener = urllib2.build_opener(urllib2.HTTPCookieProcessor())
params = urllib.urlencode({
    "form.submitted": 1,
    "pwd_empty": 0,
    "__ac_name": username,
    "__ac_password": password,
    "submit": "Log in"
})
f = opener.open('http://localhost:8080/Plone/login_form', params)
data = f.read()
f.close()

# From this point on, all methods are called with admin rights.

f = opener.open("http://localhost:8080/Plone/@@API/read?portal_type=Client")
data = json.loads(f.read())
```

### Reference Fields

When using the read method, reference values are passed back as object UIDs.

When updating or creating objects, there is a simple syntax for specifying the target objects of Reference Fields.  Request values for ReferenceFields are parsed to extract arguments which will be passed directly to the catalog search function, and the returned object will be used as the target.

The string that is used as the field value, should be a set of catalog query parameters separated by pipe "|" characters.  Each query parameter contains a keyword and a value, separated by a colon ":".

#### Example: Set the primary contact of an existing AR

Request:

    http://localhost:8080/Plone/@@API/update
        ?obj_path=/Plone/clients/client-1/W13-0001-R01
        &Contact=portal_type:Contact|getFullname:Sarel Seemonster

If the field is multiValued, there are two ways to send multiple queries for a single field: the fieldname can be specified multiple times, and suffixed with either ":list" or "[]".

#### Example: Set the CC Contacts of an existing AR

The following two requests should do the same thing:

    http://localhost:8080/Plone/@@API/update
        ?obj_path=/Plone/clients/client-1/W13-0001-R01
        &CCContact:list=portal_type:Contact|getFullname:Sarel Seemonster
        &CCContact:list=portal_type:Contact|getFullname:Rita Mohale

    http://localhost:8080/Plone/@@API/update
        ?obj_path=/Plone/clients/client-1/W13-0001-R01
        &CCContact[]=portal_type:Contact|getFullname:Sarel Seemonster
        &CCContact[]=portal_type:Contact|getFullname:Rita Mohale

### Querying objects

```
URL: @@API/read

Parameters:

- catalog_name

    If the objects to be located are indexed in a catalog other than the Plone
    default, specify it's name here.
    (default='portal_catalog').

- limit

    This is the sort_limit parameter, passed to the catalog in contentFilter.
    (default=none)

- sort_on

    This is the sort_on index name, placed into contentFilter verbatim.
    (default=id)

- sort_order

    set to 'reverse' or 'descending' to reverse sort order.
    (default="ascending")

- include_fields

    The objects, if any are found, will have only these fields' values returned.
    (default=not specified, show all.  Using the default is not recommended, as
     responses can grow quite large.)

- page_nr

    (default=0)

- page_size

    (default=10)
```

All other parameters placed in the request will be passed directly to the catalog query, inside the contentFilter argument.  This allows searching on any indexes defined in the catalog.

If the query executes successfully, the response object will contain additional attributes:

```
- total_objects

    The number of objects matched by the query

- first_object_nr

    The first object that was returned

- last_object_nr

    The last object that was returned

- objects

    The list of objects, as JSON values
```

#### Example: Get Samples in sample_received state

Request:

    http://localhost:8080/Plone/@@API/read
        ?portal_type=Sample
        &review_state=sample_received
        &include_fields=getPhysicalPath

Response:

    {
      ...,
      "total_objects":35,
      "first_object_nr":0,
      "last_object_nr":10,
      "objects":[
        {"path":"/Plone/clients/client-3/AP-0001"},
        {"path":"/Plone/clients/client-1/BAR-0001"},
        {"path":"/Plone/clients/client-5/BR-0001"},
        {"path":"/Plone/clients/client-1/BR-0002"},
        {"path":"/Plone/clients/client-3/CN-0001"},
        {"path":"/Plone/clients/client-4/CN-0002"},
        {"path":"/Plone/clients/client-1/CN-0003"},
        {"path":"/Plone/clients/client-3/CN-0004"},
        {"path":"/Plone/clients/client-2/DU-0001"},
        {"path":"/Plone/clients/client-2/DU-0002"}
      ],
    }

The batching machine has returned only the first ten results.

#### Querying Analysis Requests

When the query specifies a portal_type of AnalysisRequest, the response is modified to include all the Analyses contained in the AR, in a field called 'Analyses'.  This includes rejected/retested analyses, and their results.  The Analyses field is populated if 'Analyses' is included in the include_fields parameter, or if the include_fields parameter is not supplied.

### Creating objects

```
URL: @@API/create

Required parameters:

- obj_path

    The full database path of the parent folder inside which the new object will
    be created.  This can be discovered by looking at the 'path' entry in the
    result from a call to the read method.

- obj_type

    The portal_type of the new object to be created.

- All other request fields

    All other request parameters are assumed to be field name/value pairs, and
    if the corrosponding fields are found on the new object's schema, they will
    be set accordingly.  Any required fields must be present here.
```

### Example: Create a new AR Batch

Request:

    http://localhost:8080/Plone/@@API/create
        ?obj_path=/Plone/batches
        &obj_type=Batch
        &title=ATestBatch

### Example: Create a new Analysis in an existing AR

    http://localhost:8080/Plone/@@API/create
        ?obj_path=/Plone/clients/client-1/AP1-0001-R01
        &obj_type=Analysis
        &Service=portal_type:AnalysisService|title:Calcium

### Example: Create an AnalysisRequest

When an obj_type of AnalysisRequest is specified, the rules are slightly different, and the created objects include AnalysisRequest, Analysis, Sample, and SamplePartition.

- the obj_path field should be omitted.
- the Client field becomes a required field.

Request:

    http://localhost:8080/Plone/@@API/create
        ?obj_path=/Plone/clients/client-1/AP1-0001-R01
        &obj_type=AnalysisRequest
        &Client=portal_type:Client|id:client-1
        &Services:list=portal_type:AnalysisService|title:Calcium
        &Services:list=portal_type:AnalysisService|title:Copper
        &Services:list=portal_type:AnalysisService|title:Magnesium
        &SampleType=portal_type:SampleType|title:Apple Pulp
        &Contact=portal_type:Contact|getFullname:Rita Mohale
        &SamplingDate=2013-09-29

### Updating objects

```
URL: @@API/update

Required parameters:

- obj_path

    The path of the object to be modified.  (relative to the site root,
    without leading slash)
```

All other request parameters are assumed to be field name/value pairs, and if the corrosponding fields are found on the new object's schema, they will be set accordingly.

#### Example: Set result of a specific analysis

In the case of an analysis, the obj_path includes the Client ID, AR ID, and Analysis Service keyword for the specific analysis.  To set the Zinc value on a particular AR:

Request:

    http://localhost:8080/Plone/@@API/update
        ?obj_path=Plone/clients/client-3/CN-0001-R01/Zn
        &Result=10

#### Example: Assigning an AR to a batch

Request:

    http://localhost:8080/Plone/@@API/update
        ?obj_type=AnalysisRequest
        &id=H2O-0003-R01
        &Batch=portal_type:Batch,title:A%20Batch

Multiple updates can be executed in a single http request, by using the 'update_many' method.  This is a wrapper around the update method.  This function takes one parameter, called 'input_values', which is a json-encoded dictionary.  Each key is an obj_path, and each value is a dictionary containing key/value pairs to be set on the object.  The following input_values parameter will update two results:

    input_values={"/Plone/clients/client-5/BAR-0014-R01/DM": {"Result": 57},
                  "/Plone/clients/client-5/BAR-0014-R01/CaCO3": {"Result": 76},
                  "/Plone/clients/client-5/BAR-0014-R01/Moist": {"Result": 83}}

The result of the request will be a list of return values from the update method.

### Removing objects

```
URL: @@API/remove

Required parameters:

- UID

    The UID of the object to be removed.
    UID:list parameter key can be used multiple times for a list of UIDs.
```
### Workflow

In each call to the read method, the returned objects will have a 'review_state' value.  This is the state the object is in, on it's default workflow.  In the case that the object has several workflows attached, additional keys will appear for each workstate.  For example an AnalysisRequest object will contain 'review_state', 'cancelled_state', and 'worksheetanalysis_review_state' values.

The doActionFor method allows items to be transitioned between workflow states.  The request works the same as a 'read' request, but an extra parameter is required in the request:

    - action: The workflow transition to apply to found objects.

All other parameters are passed to the read method, and all objects found will have the workflow transition attempted.

### Catalogs