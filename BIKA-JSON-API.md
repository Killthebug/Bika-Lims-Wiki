You are here: [Home](https://github.com/bikalabs/Bika-LIMS/wiki) · Connecting Bika LIMS
***

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

## Authentication

Accessing the jsonapi URLs requires authentication.  The following python code demonstrates authenticating as the admin user, using cookies to store auth token:

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

    # From this point on, methods can be called using opener:

    f = opener.open("http://localhost:8080/Plone/@@API/read?portal_type=Client")
    data = json.loads(f.read())

## Querying objects

URL: @@API/read

There are some special request parameters used for querying objects:

- catalog_name 

    If the objects to be located are indexed in another catalog, specify it's name here.  (default='portal_catalog')

- limit 

    This is the sort_limit parameter, passed to the catalog in contentFilter.  (default=none)

- sort_on 

    This is the sort_on index name, placed into contentFilter verbatim.  (default=id)

- sort_order 

    set to 'reverse' or 'descending' to reverse sort order.  (default="ascending")

- include_fields 

    The objects, if any are found, will have only these fields' values returned.  (default=not specified, show all.  Using the default is not recommended, as responses can grow quite large.)

- page_nr 

    (default=0)

- page_size 

    (default=10)

All other parameters placed in the request will be passed directly to the catalog query, inside the contentFilter argument.  This allows searching on any indexes defined in the catalog.  For more information on querying the catalogs, read http://developer.plone.org/searching_and_indexing/query.html

If the query executes successfully, the response object will contain additional attributes:

- total_objects   

    The number of objects matched by the query

- first_object_nr 

    The first object that was returned

- last_object_nr  

    The last object that was returned

- objects         

    The list of objects, as JSON values


### Example: Get Samples in sample_received state

Request:

    http://localhost:8080/Plone/@@API/read
        ?portal_type=Sample[[br]]
        &review_state=sample_received[[br]]
        &include_fields=getPhysicalPath

Response:

    {
      "url":"http://localhost:8080/Plone/@@API/read",
      "success":true,
      "error":false,
      "_runtime":0.004884958267211914
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

## Querying Analysis Requests

When the query specifies a portal_type of AnalysisRequest, the response is modified to include all the Analyses contained in the AR, in a field called 'Analyses'.  This includes rejected/retested analyses, and their results.  The Analyses field is populated if 'Analyses' is included in the include_fields parameter, or if the include_fields parameter is not supplied.

## Creating new objects

URL: @@API/create

Required parameters:

- obj_path

    The path of the parent folder inside which the new object will be created

- obj_type

    The portal_type of the new object

All other request parameters are assumed to be field name/value pairs, and if the corrosponding fields are found on the new object's schema, they will be set accordingly.

### Example: Create a new AR Batch

Request:

    http://localhost:8080/Plone/@@API/create
        ?obj_path=/batches[[br]]
        &obj_type=Batch[[br]]
        &title=ATestBatch

Response:

    {
        "url":"http://localhost:8080/Plone/@@API/create",
        "_runtime":0.13148093223571777,
        "obj_id":"B-003",
        "success":true,
        "error":false
    }

## Updating existing objects

URL: @@API/update

Required parameters:

- obj_path

    The path of the object to be modified.  (relative to the site root, without leading slash)

All other request parameters are assumed to be field name/value pairs, and if the corrosponding fields are found on the new object's schema, they will be set accordingly.

### Example: Set result of a specific analysis

In the case of an analysis, the obj_path includes the Client ID, AR ID, and Analysis Service keyword for the specific analysis.  To set the Zinc value on a particular AR:

Request:

    http://localhost:8080/Plone/@@API/update
        ?obj_path=clients/client-3/CN-0001-R01/Zn[[br]]
        &Result=10

Response:

    {
        "url": "http://localhost:8080/Plone/@@API/update", 
        "_runtime": 0.10121297836303711,
        "objects": [], 
        "success": true,
        "error": false
    }

Multiple updates can be executed in a single http request, by using the 'update_many' method.  This is a wrapper around the update method.  This function takes one parameter, called 'input_values', which is a json-encoded dictionary.  Each key is an obj_path, and each value is a dictionary containing key/value pairs to be set on the object.  The following input_values parameter will update two results:

    input_values={"/Plone/clients/client-5/BAR-0014-R01/DM": {"Result": 57},
                  "/Plone/clients/client-5/BAR-0014-R01/CaCO3": {"Result": 76}, 
                  "/Plone/clients/client-5/BAR-0014-R01/Moist": {"Result": 83}}

The result of the request will be a list of return values from the update method.

## Reference Fields

When using the read method, reference values are passed back as object UIDs.

When using update method, there is a simple encoded-string query format used to specify the target object (or list of objects, if field has multiValued=True) to be set as the reference field's value.  It consists of key:value pairs, separated with "|", and these values are passed directly to the catalog.

### Example: Assigning an AR to a batch

Request:

    http://localhost:8080/Plone/@@API/update
        ?obj_type=AnalysisRequest[[br]]
        &id=H2O-0003-R01[[br]]
        &Batch=portal_type:Batch,title:A non-existent batch

Response:

    {
        "_runtime": 0.008334875106811523, 
        "success": false, 
        "error": "Can't resolve reference: Batch"}
    }

## Doing workflow transitions

The doActionFor method allows items to be transitioned between workflow states.  The request works the same as a 'read' request, but an extra parameter is required in the request:

    - action: The workflow transition to apply to found objects.

All other parameters are passed to the read method, and all objects found will have the workflow transition attempted.
