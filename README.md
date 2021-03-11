# tableau_tools README
------

tableau_tools was written by Bryant Howell (bhowell@tableau.com) and is documented mainly at tableauandbehold.com. The main repository for tableau_tools is https://github.com/bryantbhowell/tableau_tools/ . It is owned by Tableau Software but is not an officially supported library. If you have questions or issues with the library, please use the GitHub site or e-mail Bryant Howell directly. Tableau Support will not answer questions regarding the code.

tableau_tools is intended to be a simple-to-use library to handle all Tableau Server needs. The tableau_rest_api sub-package is a complete implementation of the Tableau Server REST API (https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api.htm ). The tableau_documents sub-package works directly with Tableau files to do manipulations necessary for making programmatic changes. There is an examples folder filled with scripts that do the most common Tableau Server administrative challenges. 

How to Install:
The latest version should always be available on PyPi (https://pypi.python.org). This means you can install or upgrade via the standard pip tool using the following command

`pip install tableau_tools`

or

`pip install tableau_tools --upgrade`

If installing on Linux or macOS, you made need to run those commands using sudo.

If you are new to Python and using Windows, once you have installed Python, you should add to your Windows PATH variable so that you can call Python and pip from any directory. If you don't know what the PATH is, the following explains how to add to it: https://www.howtogeek.com/118594/how-to-edit-your-system-path-for-easy-command-line-access/

The necessary additions to the PATH are (adjust if you are using a Python 3 environment):

`;C:\Python37;C:\Python37\Scripts`

If you are installing on Windows and getting issues related to SSL, it's possible that you have a corporate proxy. A method around this is to use Fiddler, which by default opens up a proxy on `127.0.0.1, port 8888`. Once Fiddler is open and running, you can do the following command, which tells pip to use the Fiddler proxy, and to trusted pypi.python.org regardless of any SSL certificate issues.

    set http_proxy=127.0.0.1:8888
    pip install --proxy 127.0.0.1:8888 --trusted-host pypi.python.org tableau_tools

(A bit more explanation on the SSL issues here https://stackoverflow.com/questions/25981703/pip-install-fails-with-connection-error-ssl-certificate-verify-failed-certi)

**Notes on Getting Started**:
Some of the methods return Element objects which can be manipulated via standard ElementTree methods. 

tableau_tools was *programmed using PyCharm and works very well in that IDE. It is highly recommended if you are going to code with tableau_tools.*


## **Version history** 
------
* 1.5.2: works with 9.1 and before. Python 2.7 compatible
* 2.0.0+: works with 9.2 (and previous versions as well). Python 2.7 compatible
* 2.1.0+: works with 9.3 (previous versions as well). Python 2.7 compatible
* 3.0.0+: tableau_rest_api library refactored and added to new tableau_tools package
* 4.0.0: Big rewrite to simplify and improve. You will need to update your scripts most likely.
* 4.3.0: 10.5 (API 2.8) compatibility, 100% coverage of all features in the spec, and refactoring in the code itself.
*  README vastly updated to cover all topics.
* 4.4.0: A lot of improvements to the tableau_documents library and its documentation
* 4.5.0: 2018.1 (API 3.0) compatibility. All requests for a given connection using a single HTTP session, and other improvements to the RestXmlRequest class.
* 4.7.0: Dropping API 2.0 (Tableau 9.0) compatibility. Any method that is overwritten in a later version will not be updated in the TableauRestApiConnection class going forward. Also implemented a direct_xml_request parameter for Add and Update methods, allowing direct submission of an ElementTree.Element request to the endpoints, particularly for replication.
* 4.8.0 Introduces the RestJsonRequest object and _json plural querying methods for passing JSON responses to other systems
* 4.9.0 API 3.3 (2019.1) compatibility, as well as ability to swap in static files using TableauDocument and other bug fixes.
* 5.0.0 Python 3.6+ native rewrite. Completely re-organized in the backend, with two models for accessing Rest API methods: TableauRestApiConnection (backwards-compatible) and TableauServerRest, with subclasses grouping the methods.

--- Table(au) of Contents ---
------

- [0. Installing and Getting Started](#0-installing-and-getting-started)
  * [0.0 Examples](#00-examples)
  * [0.1 Importing tableau_tools library](#01-importing-tableau_tools-library)
  * [0.2 Logger class](#02-logger-class)
  * [0.3 TableauBase class](#03-tableaubase-class)
  * [0.4 tableau_exceptions](#04-tableau-exceptions)
  * [0.5 ElementTree.Element for XML handling](#05-elementtree)
- [1. tableau_rest_api sub-package](#1-tableau-rest-api-sub-package)
  * [1.1 Connecting](#11-connecting)
  	+ [1.1.1 TableauRestApiConnection & TableauServerRest classes](#111-tableaurestapiconnection-and-tableauserverrest-classes)
    + [1.1.2 Enabling Logging](#112-enabling-logging)
    + [1.1.3 Signing in](#113-signing-in)
    + [1.1.4 Connecting to multiple sites](#114-connecting-to-multiple-sites)
    + [1.1.5 Signout, Killing Other Sessions, and Swapping Tokens](#115-signout-killing-other-sessions-swapping-tokens)
  * [1.2 Basics and Querying](#12-basics-and-querying)
  	+ [1.2.0 ElementTree.Element XML Responses](#120-elementree-element-xml-responses)
  	  - [1.2.0.1 Underlying Base Methods](#1201-underlying-base-methods)
  	+ [1.2.1 LUIDs - Locally Unique IDentifiers](#121-luids---locally-unique-identifiers)
    + [1.2.2 Plural querying methods](#122-plural-querying-methods)
      - [1.2.2.1 Filtering and Sorting](#1221-filtering-and-sorting-tableau-server-93)
      - [1.2.2.2 Fields](#1222-fields-api-25)
    + [1.2.3 LUID Lookup Methods](#123-luid-lookup-methods)
    + [1.2.4 Singular querying methods](#124-singular-querying-methods)
    + [1.2.5 Querying Permissions](#125-querying-permissions)
    + [1.2.6 "Download" and "Save" methods](#126--download--and--save--methods)
  * [1.3 Administrative Actions (adding, removing, and syncing)](#13-administrative-actions-adding-removing-and-syncing)
    + [1.3.1 Adding Users](#131-adding-users)
    + [1.3.2 Create Methods for other content types](#132-create-methods-for-other-content-types)
      - [1.3.2.1 direct_xml_request arguments on ADD / CREATE methods for duplicating site information](#1321-direct-xml-request-arguments-on-ADD-CREATE-methods)
    + [1.3.3 Adding users to a Group](#133-adding-users-to-a-group)
    + [1.3.4 Update Methods](#134-update-methods)
    + [1.3.5 Deleting / Removing Content](#135-deleting--removing-content)
    + [1.3.6 Deleting a Site](#136-deleting-a-site)
    + [1.3.7 Schedules (Extract and Subscriptions)](#137-schedules-extract-and-subscriptions)
    + [1.3.8 Subscriptions](#138-subscriptions-api-23)
  * [1.4 Permissions](#14-permissions)
    + [1.4.1 PublishedContent Classes (Project, Workbook, Datasource, Flow, Database, Table)](#141-publishedcontent-classes-project20project21-workbook-datasource)
    + [1.4.2 Permissions Classes](#142-permissions-classes)
    + [1.4.2 Setting Capabilities](#142-setting-capabilities)
    + [1.4.2 Permissions Setting](#142-permissions-setting)
    + [1.4.3 Reusing Permissions Objects](#143-reusing-permissions-objects)
    + [1.4.4 Replicating Permissions from One Site to Another](#144-replicating-permissions-from-one-site-to-another)
  * [1.5 Publishing Content](#15-publishing-content)
    + [1.5.1 Publishing a Workbook or Datasource](#151-publishing-a-workbook-or-datasource)
    + [1.5.2 Workbook and Datasource Revisions](#152-workbook-and-datasource-revisions-23)
    + [1.5.3 Asynchronous Publishing (API 3.0+)](#153-asynchronous-publishing-api-30)
  * [1.6 Refreshing Extracts](#16-refreshing-extracts-tableau-103-api-26)
    + [1.6.1 Running an Extract Refresh Schedule](#161-running-an-extract-refresh-schedule-tableau-103-api-26)
    + [1.6.2 Running an Extract Refresh (no schedule) (10.5/ API 2.8)](#162-running-an-extract-refresh--no-schedule-105-api-28)
    + [1.6.3 Putting Published Content on an Extract Schedule (10.5+)](#163-putting-published-content-on-an-extract-schedule-105)
  * [1.7 Data Driven Alerts (2018.3+)](#17-data-drive-alerts)
  * [1.8 Tableau Prep Flows (2019.1+)](#18-tableau-prep-flows)
  * [1.9 Favorites](#19-favorites)
  * [1.10 Metadata (2019.3+)](#110-metadata)
  * [1.11 Webhooks (2019.4+)](#111-webhooks)
- [2 tableau_documents: Modifying Tableau Documents (for Template Publishing)](#2-tableau-documents-modifying-tableau-documents-for-template-publishing)
  * [2.0 Getting Started with tableau_documents: TableauFileOpener class](#20-getting-started-with-tableau-documents) 
  * [2.1 tableau_documents basic model](#21-tableau-documents-basic-model)
  * [2.2 TableauXmlFile Classes (TDS, TWB, TFL)](#22-tableau-xml-file-classes)
  * [2.3 TableauPackagedFile Classes (TDSX, TWBX, TFLX](#23-tableaudocument-class)
  	+ [2.3.1 Replacing Static Data Files](#231-replacing-static-data-files)
  * [2.4 TableauDatasource Class and the DatasourceFileInterface](#24-tableaudatasource-class)
    + [2.4.1 Iterating through .datasources](#241-iterating-through-datasources)
    + [2.4.2 Published Datasources in a Workbook](#242-published-datasources-in-a-workbook)
    + [2.4.3 TableauConnection Class](#243-tableauconnection-class)
    + [2.4.4 TableauColumns Class](#244-tableaucolumns-class)
    + [2.4.5 Modifying Table JOIN Structure in a Datasource](#211-modifying-table-join-structure-in-a-datasource)
      - [2.4.5.1 Database Table Relations](#2451-database-table-relations)
      - [2.4.5.2 Custom SQL Relations](#2452-custom-sql-relations)
      - [2.4.5.3 Stored Procedure Relations](#2453-stored-procedure-relations)
    + [2.4.6 Adding Data Source Filters to an Existing Data Source](#246-adding-data-source-filters-to-an-existing-data-source)
    + [2.4.7 Defining Calculated Fields Programmatically](#247-defining-calculated-fields-programmatically)
  * [2.5 TableauWorkbook Class](#25-tableauworkbook-class)
    + [2.5.1 Creating and Modifying Parameters](#251-creating-and-modifying-parameters)
      - [2.5.1.1 TableauParameter class](#2511-tableauparameter-class)
- [3 tabcmd](#3-tabcmd)
  * [3.1 Tabcmd Class](#31-tabcmd-class)
  * [3.2 Triggering an Extract Refresh](#32-triggering-an-extract-refresh)
- [4 tableau_repository](#4-tableau-repository)
  * [4.1 TableauRepository Class](#41-tableaurepository-class)
  * [4.2 query() Method](#42-query---method)
  * [4.3 Querying (and killing) Sessions](#43-querying--and-killing--sessions)
- [5 Internal Library Workings](#5-internals)
  * [5.1 tableau_tools Library Structure](#51-tableau-tools-library-structure)

## 0. Getting Started
### 0.0 Examples 
Within the installed package, there is an examples sub-directory with a bunch of functional examples as well as ones that start with "test_suite" that show how to use almost any method in the library.

It may be easier just to find them all on GitHub at https://github.com/bryantbhowell/tableau_tools/tree/5.0.0/examples , and then download them to your own local directories and start trying them out.

### 0.1 Importing tableau_tools library into your scripts
If you just import tableau_tools per the following, you will have access to the tableau_rest_api sub-package:

    from tableau_tools import *

If you need to use tableau_documents, use the following import statements:

    from tableau_tools import *
    from tableau_tools.tableau_documents import *

### 0.2 Logger class
The Logger class implements useful and verbose logging to a plain text file that all of the other objects can use. You declare a single Logger object, then pass it to the other objects, resulting in a single continuous log file of all actions.

`Logger(filename)`

If you want to log something in your script into this log, you can call

`Logger.log(l)`

where l is a string. You do not need to add a "\n", it will be added automatically.

The Logger class, starting in tableau_tools 5.1, has multiple options to show different levels of response.

By default, the Logger will only show the HTTP requests with URI, along the chain of nested methods used to perform the actions.

`Logger.enable_request_logging()`

will display the string version of the XML requests sent along with the HTTP requests.

`Logger.enable_response_logging()`

will display the string version of all XML responses in the logs. This is far more verbose, so is only suggested when you are encountering errors based on expectations of what should be in the response.

`Logger.enable_debug_level()`

makes the Logger indent the lines of the log, so that you can see the nesting of the actions that happen more easily. This is what the logs looked like in previous version of tableau_tools, but now it must be turned on if you want that mode.

### 0.3 TableauRestXml class
There is a class called TableauRestXml which holds static methods and properties that are useful on any Tableau REST XML request or response.

TableauServerRest and TableauRestApiConnection both inherit from this class so you can call any of the methods from one of those objects rather than calling it directly.

### 0.4 tableau_exceptions
The tableau_exceptions file defines a variety of Exceptions that are specific to Tableau, particularly the REST API. They are not very complex, and most simply include a msg property that will clarify the problem if logged

### 0.5 ElementTree.Element for XML handling
All XML in tableau_tools is handled through ElementTree. It is aliased as ET per the standard Python documentation (https://docs.python.org/3/library/xml.etree.elementtree.html) . If you see a return type of ET.Element, that means you are dealing with an ElementTree.Element object -- basically the raw response from the Tableau REST API, or some kind of slice of one.

Because they are ElementTree.Element objects, you can use the `.find()` and `findall()` methods to do a limited sub-set of XPath queries on the responses. XPath isn't the easiest thing to do, and often the Tableau Server REST API has direct filtering mechanisms available in the requests. If you see a parameter on a method that appears to limit the result set of a request, please try that first as it is will implement the fastest and most correct algorithm known to the tableau_tools authors. 

## 1. tableau_rest_api sub-package

`tableau_tools.tableau_rest_api` sub-package is designed to fully implement every feature in every version of the Tableau Server REST API. As much as possible, every action that is available in the reference guide here

https://onlinehelp.tableau.com/current/api/rest_api/en-us/REST/rest_api_ref.htm#API_Reference

is implemented as a method using the same name. For example, the action listed as "Get Users in Group" is implemented as `TableauRestApiConnection.get_users_in_group()` . There are a few places where there are deviations from this pattern, but in general you can start typing based on what you see in the reference and find the method implementing it.


### 1.1 Connecting
#### 1.1.1 TableauRestApiConnection & TableauServerRest classes
tableau_tools 5.0+ implements two object types for accessing the methods of the Tableau Server REST API: TableauRestApiConnection and TableauServerRest.

The base TableauRestApiConnection and TableauServerRest classes implements the 2.6 version of the REST API, equivalent to Tableau 10. If you are using a version of Tableau older than this, please look into upgrading as soon as possible, as they are out of support. Then later versions of the API are represented by appending the API version to the class name. For example, if you want to use the API for Tableau Server 2018.1 (API Version 3.0), you would instantiate a TableauRestApiConnection30 or TableauServerRest30 object. In general, you can always use an older version of the API with a newer version of Tableau Server, unless you know of a major change in how something behaves, or want to use the latest features.

The first set of objects, which are compatible with scripts from the 4.0 series of tableau_tools, are the TableauRestApiConnection classes. These implement all methods directly on the TableauRestApiConnection class.


`TableauRestApiConnection(server, username, password, site_content_url=""): 10.3`

`TableauRestApiConnection27: 10.4`

`TableauRestApiConnection28: 10.5`

`TableauRestApiConnection30: 2018.1`

`TableauRestApiConnection31: 2018.2`

`TableauRestApiConnection32: 2018.3`

`TableauRestApiConnection33: 2019.1`

`TableauRestApiConnection34: 2019.2`

`TableauRestApiConnection35: 2019.3`

`TableauRestApiConnection36(server, username=None, password=None, site_content_url="", pat_name=None, pat_secret=None): 2019.4`

The second set of objects, new in tableau_tools 5.0, are the TableauServerRest objects. These group the available methods into related sub-objects, for ease of organization. Basic functionalities, including the translation between real names and LUIDs, are still on the base class itself. For example, you still use `TableauServerRest.signin()` , but you would use `TableauServerRest.projects.update_project()` rather than `TableauRestApiConnection.update_project()` . 

The TableauServerRest classes are:

`TableauServerRest(server, username, password, site_content_url=""): 10.3`

`TableauServerRest27: 10.4`

`TableauServerRest28: 10.5`

`TableauServerRest30: 2018.1`

`TableauServerRest31: 2018.2`

`TableauServerRest32: 2018.3`

`TableauServerRest33: 2019.1`

`TableauServerRest34: 2019.2`

`TableauServerRest35: 2019.3`

`TableauServerRest36(server, username=None, password=None, site_content_url="", pat_name=None, pat_secret=None): 2019.4`

You need to initialize at least one object of either of the two class types. 
Ex.:

    t = TableauRestApiConnection33("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    
    # or 
    
    t = TableauServerRest36(server="http://127.0.0.1", pat_name="ripFatPat", pat_secret="qlE1g9MMh9vbrjjg==:rZTHhPpP2tUW1kfn4tjg8", site_content_url="fifth_ward")
    
The actual methods, whether attached directly or as sub-classes, are implemented in the same class files. This means that the two object types are equivalent -- they are calling the same code in the end, and yo'll ever have to choose between the two styles to get a particular feature.

#### 1.1.2 Enabling Logging 

    logger = Logger("log_file.txt")
    TableauRestApiConnection.enable_logging(logger) 
    # or 
    TableauServerRest.enable_logging(logger)

#### 1.1.3 Signing in
The TableauRestApiConnection doesn't actually sign in and create a session until you make a `signin()` call

Ex.

    t = TableauRestApiConnection33("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    # or
      t = TableauServerRest33("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    t.signin()
    logger = Logger("log_file.txt")
    t.enable_logging(logger)

Now that you are signed-in, the object will hold all of the session state information and can be used to make any number of calls to that Site. 

You can signin AS another user as well, through impersonation. You must first get that user's LUID for the given site you want to sign into:

    t = TableauServerRest("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    # Most LUID lookups live in the main object of TableauServerRest
    t.signin()
    user_luid = t.query_user_luid('a.user')
    t2 = TableauServerRest("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    t2.signin(user_luid_to_impersonate=user_luid)
    
Starting in API 3.6 (2019.4+), you can use a Personal Access Token to signin rather than username and password. Simply use the `pat_name` and `pat_secret` optional parameters rather than `username` and `password` in the `signin()` method. One note: as of this writing, you cannot do an impersonation signin when using PAT instead of username/password credentials. But always check the most recent documentation of the Tableau Server REST API to see if things have changed.

#### 1.1.4 Connecting to multiple sites
The Tableau REST API only allows a session to a single Site at a time. To deal with multiple sites, you can create multiple objects representing each site. To sign in to a site, you need the `site_content_url`, which is the portion of the URL that represents the Site. 

    TableauRestApiConnection.query_all_site_content_urls()
    # or 
    TableauServerRest.sites.query_all_site_content_urls()
 

returns an list that can be iterated over. You must sign-in to one site first to get this list however. So if you wanted to do an action to all sites, do the following:

    default = TableauRestApiConnection34("http://127.0.0.1", "admin", "adminsp@ssw0rd")
    default.signin()
    site_content_urls = default.query_all_site_content_urls()
    
    for site_content_url in site_content_urls:
        t = TableauRestApiConnection34("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url=site_content_url)
        t.signin()
        ...

### 1.1.5 Signout, Killing Other Sessions, and Swapping Tokens
The `signout()` method called by itself will send the Sign Out REST API command to the server for the username that the object was created as. It also implements an optional parameter that lets you sign out a different session:

`t.signout(session_token='buerojh8b4L2-b208guDSVD!--038bSVE')`

Depending on how your Tableau Server is connected, you may be able to use session values retrieved from the Tableau Server Repository in this function to sign-out (i.e. kill) regular user sessions or other REST API sessions you want to end. 

The REST API session you are connected to is represented by a token, which the object stores internally after a successful `.signon()` . There may be situations where you would like to retrieve that token, or even swap in different tokens, so that the object doesn't have to be recreated just to send a message. One example of this is a RESTful web service that must impersonate multiple users, and otherwise would generate a new object for each user/site connection. 

Swapping in a different session into the main objects requires more than just the session_token. If you need to do this, use `swap_token(site_luid: str, user_luid: str, token: str)`

You can get all of the values from the previous object, if it has been signed in. You also must have signed in to the second object at least once, because that initializes much of the other internal properties correctly.:
    
    t = TableauRestApiConnection(server='http://127.0.0.1', username='usrnm', password='nowthatsabigpass', site_content_url='some_site')
    t.signin()
    first_site_luid = t.site_luid
    first_user_luid = t.user_luid
    first_token = t.token
    
    t2 = TableauRestApiConnection(server='http://127.0.0.1', username='a.nother', password='passittotheleft', site_content_url='some_site_other_site')
    t2.signin()
    t2.swap_token(site_luid=first_site_luid, user_luid=first_user_luid, token=first_token)
    
Now any actions you take with t2 will be the same session to the Tableau Server as those taken with t1. This can also be very useful for multi-threaded programming, where you may want to clone instances of an object so that they don't interact with one another while doing things on different threads, but still use the same REST API session.

### 1.2 Basics and Querying
NOTE: If you get a "RecursionError: maximum recursion depth exceeded" Exception, it generally means that you are trying a method that does not exist. The most common reason for this with the TableauServerRest objects is when porting code over from TableauRestApiConnection objects. All methods except for lookups live under a sub-object (t.users.method()) in TableauServerRest, so if you put a method directly on the main object, it will try and search for it but error out. This issue can work the other way, if you try and use a sub-object under TableauRestApiConnection when it doesn't exist.

#### 1.2.0 ElementTree.Element XML Responses
As mentioned in the 0 section, tableau_tools returns ElementTree.Element responses for any XML request. To know exactly what is available in any given object coming back from the Tableau REST API, you'll need to either consult the REST API Reference or convert to it something you can write to the console or a text file.

    t = TableauServerRest("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    t.signin()
    groups = t.groups.query_groups()
    print(ET.tostring(groups))
    
If it is a collection of elements, you can iterate through using the standard Python for.. in loop. If you know there is an attribute you want (per the Reference or the printed response), you can access it via the `.get(attribute_name)` method of the Element object:

    for group in groups:
        print(ET.tostring(group))
        group_luid = group.get('id')
        group_name = group.get('name')


Some XML responses are more complex, with element nodes within other elements. An example of this would be workbooks, which have a project tag inside the workbook tag. The Pythonic way is to iterate through the workbook object to get to the sub-objects. You must watch out, though, because the Tableau REST API uses XML Namespaces, so you can't simply match the tag names directly using the `==` operator. Instead, you are better off using `.find()` string method to find a match with the tag name you are looking for:

    wbs = t.workbooks.query_workbooks()
    for wb in wbs:
        print(ET.tostring(wb))
        for elem in wb:
            # Only want the project element inside
            if elem.tag.find('project') != -1:
                proj_luid = elem.get('id')
                
##### 1.2.0.1 Underlying Base Methods
This section is only useful or necessary if you are investingating an issue with the library, or need to do something from the reference guide that is not implemented yet with its own dedicated methods. 

The rest_api_base class implements a number of methods which are used by the more specific methods. These actualy perform the basic REST API actions, along with Tableau Server specific checks and algorithms. 

For example:

    query_resource(url_ending: str, server_level:bool = False, 
    filters: Optional[List[UrlFilter]] = None, 
    sorts: Optional[List[Sort]] = None, 
    additional_url_ending: Optional[str] = None, 
    fields: Optional[List[str]] = None) -> ET.Element

Is the underlying abstracted method that almost all "query_{element}s" methods actually use. It sends an HTTP GET request, and then returns back the response. If a new endpoint has been added to Tableau Server, before tableau_tools is updated, you could directly use query_resource:

    xml = t.query_resource(url_ending="newthings")
    # or
    xml = t.query_resource(url_ending="newserverlevelthings", server_level=True)
    
The query_resource method automatically builds the first part of the URL based on the currently signed in site / user / etc., so you only have to pass in the part of the URL after the site luid. The "server_level" flag tells the URL builder to omit the site luid, for those endpoints that are server-wide. And you can see the other parameters that are available for constructing more complex queries.

Similarly, there is a `query_resource_json()` for receiving back a JSON response to the same type of requests.

There are other `query_` methods available in RestApiBase, but they typically implement lookup or search mechanisms that are tailored to the particulars of the various objects on Tableau Server, so you should only be looking at them for bug-fixes or if you were trying to expand the library. 

The basic HTTP actions to the REST API are all implemented via methods that begin with `send_`:

    send_post_request(url: str) -> ET.Element
    send_add_request(url: str, request: ET.Element) -> ET.Element
    send_update_request(url: str, request: ET.Element) -> ET.Element
    send_delete_request(url: str) -> int
    send_publish_request(url: str, xml_request: Optional[ET.Element], content: bytes, boundary_string: str) -> ET.Element
    send_append_request(url: str, content: bytes, boundary_string: str) -> ET.Element
    send_binary_get_request(url: str) -> bytes

If there was a particular new endpoint that you needed to POST an XML element to to add to, you could accomplish using `send_add_request` (send_post_request exists for POSTs with no request content)

    tsr = ET.Element('tsRequest')
    e = ET.Element('newContentType')
    e.set('name', 'My.Name')
    tsr.append(e)
    new_luid = t.send_add_request(url='newcontenttypes', request=tsr)

Internally if you look at most add methods, this is fundamentally how they are all constructed. 

For historical reasons, the actual HTTP connections are handled in a separate class and object entirely, either RestXmlRequest or RestJsonRequest, rather than performed directly within RestApiBase. Those objects both use the standad `requests` library to handle their HTTP actions. Unless you are updating the library to handle a vastly new situation with regards to request or response types with the REST API, it is unlikely you'll ever need to look into the details of those objects or attempt to access them directly. 

#### 1.2.1 LUIDs - Locally Unique IDentifiers
The Tableau REST API represents each object on the server (project, workbook, user, group, etc.) with a Locally Unique IDentifier (LUID). Every command other than the sign-in to a particular site (which uses the `site_content_url`) requires a LUID. LUIDs are returned when you create an object on the server, or they can be retrieved by the Query methods and then searched to find the matching LUID. In the XML or JSON, they are labeled with the `id` value, but tableau_tools specifically refers to them as LUID throughout, because there are other Tableau IDs in the Tableau Server repoistory.

tableau_tools handles translations between real world names and LUIDs automatically for the vast majority of methods. Any argument names that can accept both LUIDs and names are named along the pattern : "..._name_or_luid".
 
 There are few cases where only the LUID can be accepted. In this case, the parameter will show just "_luid". 


#### 1.2.2 Plural querying methods and converting to { name : luid} Dict
The simplest method for getting information from the REST API are the "plural" querying methods

`TableauRestApiConnection.query_groups()`

`TableauRestApiConnection.query_users(username_or_luid)`

`TableauRestApiConnection.query_workbooks()`

`TableauRestApiConnection.query_projects()`

`TableauRestApiConnection.query_datasources()`

`TableauRestApiConnection.query_workbook_views()`

 `TableauServerRest.datasources.query_datasources()`

These will all return an ElementTree object representing the results from the REST API call. This can be useful if you need all of the information returned, but most of your calls to these methods will be to get a dictionary of names : luids you can use for lookup. There is a simple static method for this conversion

`TableauRestApiConnection.convert_xml_list_to_name_id_dict(xml_obj)`

`TableauServerRest.convert_xml_list_to_name_id_dict(xml_obj)`

Ex.

    default = TableauRestApiConnection34("http://127.0.0.1", "admin", "adminsp@ssw0rd")
    default.signin()
    groups = default.query_groups()
    groups_dict = default.convert_xml_list_to_name_id_dict(groups)
    
    for group_name in groups_dict:
        print "Group name {} is LUID {}".format(group_name, groups_dict[group_name])

There are also equivalent JSON querying methods for the plural methods, which return a Python str object, using the Tableau REST API's native method for requesting JSON responses rather than XML. 

The JSON plural query methods allow you to specify the page of results using the page= optional parameter(starting at 1). If you do not specify a page, tableau_tools will automatically paginate through all results and combine them together. 

`TableauRestApiConnection.query_groups_json(page_number=None)`

`TableauRestApiConnection.query_users_json(page_number=None)`

`TableauRestApiConnection.query_workbooks_json(username_or_luid, page_number=None)`

`TableauRestApiConnection.query_projects_json(page_number=None)`

`TableauRestApiConnection.query_datasources_json(page_number=None)`

`TableauRestApiConnection.query_workbook_views_json(page_number=None)`

`TableauServerRest.workbooks.query_workbook_views_json(page_number=None)`

##### 1.2.2.1 Filtering and Sorting 
`The classes implement filtering and sorting for the methods directly against the REST API where it is allowed. Singular lookup methods are programmed to take advantage of this automatically for improved performance, but the plural querying methods can use the filters to bring back specific sets.

http://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_concepts_filtering_and_sorting.htm%3FTocPath%3DConcepts%7C_____7

You should definitely check in the REST API reference as to which filters can be applied to which calls. Most of the function parameters should give you the expected filter (you can use a plural or singular version).

For example, `query_projects` will run by itself, but it also contains optional parameters for each of the filter types it can take.

`TableauRestApiConnection.query_projects(self, name_filter=None, owner_name_filter=None, updated_at_filter=None, created_at_filter=None,
                       owner_domain_filter=None, owner_email_filter=None, sorts=None)`

Filters can be passed via a `UrlFilter` class object. However, you do not need to generate them directly, but instead should use factory methods (all starting with "get_" to make sure you get them created with the right options. Both TableauServerRest and TableauRestApiConnection have a `.url_filters` property which gives you access to the correct UrlFilters object for that version of the API. This is the easiest access point and can be used like:

    t = TableauServerRest(server='', username='', password='')
    name_filter = t.url_filters.get_name_filter(name='some name to look for')

The following lists all of the available factory methods (although check with the IDE using autocomplete as there may be more): 

`UrlFilter.get_name_filter(name)`

`UrlFilter.get_site_role_filter(site_role)`

`UrlFilter.get_owner_name_filter(owner_name)`

`UrlFilter.get_created_at_filter(operator, created_at_time)`

`UrlFilter.get_updated_at_filter(operator, updated_at_time)`

`UrlFilter.get_last_login_filter(operator, last_login_time)`

`UrlFilter.get_tags_filter(tags)`

`UrlFilter.get_tag_filter(tag)`

`UrlFilter.get_datasource_type_filter(ds_type)`

`UrlFilter27.get_names_filter(names)`

`UrlFilter27.get_site_roles_filter(site_roles)`

`UrlFilter27.get_owner_names_filter(owner_names)`

`UrlFilter27.get_domain_names_filter(domain_names)`

`UrlFilter27.get_domain_nicknames_filter(domain_nicknames)`

`UrlFilter27.get_domain_name_filter(domain_name)`

`UrlFilter27.get_domain_nickname_filter(domain_nickname)`

`UrlFilter27.get_minimum_site_roles_filter(minimum_site_roles)`

`UrlFilter27.get_minimum_site_role_filter(minimum_site_role)`

`UrlFilter27.get_is_local_filter(is_local)`

`UrlFilter27.get_user_count_filter(operator, user_count)`

`UrlFilter27.get_owner_domains_filter(owner_domains)`

`UrlFilter27.get_owner_domain_filter(owner_domain)`

`UrlFilter27.get_owner_emails_filter(owner_emails)`

`UrlFilter27.get_owner_email_filter(owner_email)`

`UrlFilter27.get_hits_total_filter(operator, hits_total)`

Note that times must be specified with a full ISO 8601 format as shown below, however you can just pass a datetime.datetime object and the methods will convert automatically

Ex. 
    
    import datetime
    # t = TableauServerRest...
    bryant_filter = t.url_filters.get_owner_name_filter('Bryant')
    t_filter = t.url_filters.get_tags_filter(['sales', 'sandbox'])
    # If you manually want to set this time format:
    # ca_filter = t.url_filters.get_created_at_filter('gte', '2016-01-01T00:00:00:00Z')
    now = datetime.datetime.now()
    offset_time = datetime.timedelta(days=1)
    time_to_filter_by = now - offset_time
    ca_filter = t.url_filters.get_created_at_filter('gte', time_to_filter_by)
    t.workbooks.query_workbooks(owner_name_filter=bryant_filter, tags_filter=t_filter, created_at_filter=ca_filter)

There is also a Sort object, but it is best to use the static factory methods through the `.sorts` property of the main REST connection objects:

`Sort.Ascending(field: str) -> Sort`
`Sort.Descending(field:str) -> Sort`

Sorts can be passed as a list to those methods that can accept them like the following:
    
    # t = TableauServerRest( ...
    s = t.sorts.Ascending('name')
    t.query_workbooks(owner_name_filter=bryant_filter, tags_filter=t_filter, sorts=[s,])

##### 1.2.2.2 Fields 
Some REST API endpoints allow you to specify fields, which all for bringing back additional fields not in the original specifications for certain calls, or limit down what is retrieved so that there is not so much additional to process through.

Fields are only available on certain calls, detailed here:

https://onlinehelp.tableau.com/current/api/rest_api/en-us/help.htm#REST/rest_api_concepts_fields.htm%3FTocPath%3DConcepts%7C_____8

Where a field reduction can improve efficiency, it is implemented without any need to call it explicitly.

For the calls where there is MORE information available now with fields, they all have been converted to automatically call the `"_all_"` method of the fields, to bring back everything. If you instead want to send a particular set of fields, you can include them as a list of unicode values. Just make sure to the look at the reference guide for what to send.

For example, the definition of `query_users()` looks like:

`TableauRestApiConnection.query_users(all_fields=True, last_login_filter=None, site_role_filter=None, sorts=None, fields=None)`

You can use like this to specify specific fields only to come back:

    t.query_users(fields=['name', 'id', 'lastLogin')

(This is a lot more useful on something like `query_workbooks` which has additional info about the owner and the project which are not included in the defaults).

#### 1.2.3 LUID Lookup Methods
There are numerous methods for finding an LUID based on the name of a piece of content. Because LUID lookup for all types is useful to almost any commands, these methods live in the main class, even in `TableauServerRest`. An example is:

    TableauRestApiConnection.query_group_luid(name)
    # or 
    TableauServerRest.query_group_luid(name)

These methods are very useful when you need a LUID to generate another action.  You shouldn't need these methods very frequently, as the majority of methods will do the lookup automatically if a name is passed in. 

However, if you do have a LUID from a call or a create method, it will be faster to pass in the LUIDs, particularly for large lists.

#### 1.2.4 Singular querying methods
There are methods for getting the XML just for a single object, but they actually require calling to the plural methods internally in many cases where there is no singular method actually implemented in Tableau Server. 

Most methods follow this pattern:

`TableauRestApiConnection.query_project(name_or_luid)`

`TableauRestApiConnection.query_user(username_or_luid)`

`TableauRestApiConnection.query_datasource(ds_name_or_luid, proj_name_or_luid=None)`

`TableauRestApiConnection.query_workbook(wb_name_or_luid, p_name_or_luid=None, username_or_luid=None)`

Yo'll notice that `query_workbook` and `query_datasource` include parameters for the project (and the username for workbooks). This is because workbook and datasource names are only unique within a Project of a Site, not within a Site. If you search without the project specified, the method will return a workbook if only one is found, but if multiple are found, it will throw a `MultipleMatchesFoundException` .

Unlike almost every other singular method, `query_project` returns a `Project` object, which is necessary when setting Permissions, rather than an `ET.Element` . This does take some amount of time, because all of the underlying permissions and default permissions on the project are requested when creating the Project object

`TableauRestApiConnection.query_project(project_name_or_luid) : returns Project`

If you just need to look at the values from the XML, can request the XML element using:

`TableauRestApiConnection.query_project_xml_object(project_name_or_luid(`

or if you already have a Project object, you can access it this way:

`Project.get_xml_obj()`

#### 1.2.5 Querying Permissions
Because Permissions actually exist and attach to Published Content on the Tableau Server, all Permissions are handled through one of the derived `PublishedContent` classes (Project, Workbook, or Datasource). There are no direct methods to access them, because the `PublishedContent` methods include the most efficient algorithms for updating Permissions with the least amount of effort. See Section 1.4 for all the details on Permissions.

#### 1.2.6 "Download" and "Save" methods
Published content (workbooks and datasources) and thumbnails can all be queried, but they come down in formats that need to be saved in most cases. For this reason, their methods are named as following:

`TableauRestApiConnection.save_workbook_preview_image(wb_name_or_luid, filename)`

`TableauRestApiConnection.save_workbook_view_preview_image_by_luid(wb_name_or_luid, view_name_or_luid, filename)`

These live in the sub-objects for their content types in `TableauServerRest`:

`TableauServerRest.workbooks.save_workbook_preview_image(wb_name_or_luid, filename)`


**Do not include file extension. Without filename, only returns the response**
`TableauRestApiConnection.download_datasource(ds_name_or_luid, filename_no_extension, proj_name_or_luid=None)`

`TableauRestApiConnection.download_workbook(wb_name_or_luid, filename_no_extension, proj_name_or_luid=None)`


### 1.3 Administrative Actions (adding, removing, and syncing)

#### 1.3.1 Adding Users
There are two separate actions in the Tableau REST API to add a new user. First, the user is created, and then additional details are set using an update command. `tableau_rest_api` implements these two together as: 

`TableauRestApiConnection.add_user(username, fullname, site_role='Unlicensed', password=None, email=None, update_if_exists=False)`

If you just want to do the basic add, without the update, then do:

`TableauRestApiConnection.add_user_by_username(username, site_role='Unlicensed', update_if_exists=False)`

The update_if_exists flag allows for the role to be changed even if the user already exists when set to True.

#### 1.3.2 Create Methods for other content types
The other methods for adding content start with `"create_"`. Each of these will return the LUID of the newly created content

`TableauRestApiConnection.create_project(project_name, project_desc=None, locked_permissions=False)`

`TableauRestApiConnection.create_site(new_site_name, new_content_url, admin_mode=None, user_quota=None, storage_quota=None, disable_subscriptions=None)`

`TableauRestApiConnection.create_group(self, group_name)`

`TableauRestApiConnection.create_group_from_ad_group(self, ad_group_name, ad_domain_name, default_site_role='Unlicensed', sync_as_background=True)`

Ex.

    new_luid = t.create_group("Awesome People")
    # or if using TableauServerRest
    new_luid = t.groups.create_group('Awesome People')

##### 1.3.2.1 direct_xml_request arguments on ADD / CREATE methods for duplicating site information     
Some, if not all, of the create / add methods implement an optional `direct_xml_request` parameter, which allows you to submit your own ET.Element, starting with the tsRequest tag. When there is any value sent as an argument for this parameter, that method will ignore any values from the other arguments and just submit the Element you send in.

The original purpose of this parameter is for allowing easy duplication of content from one site/server to another in conjunction with 

`build_request_from_response(request: ET.Element)`

This method automatically converts any single Tableau REST API XML tsResponse object into a tsRequest -- in particular, it removes any IDs, so that the tsRequest can be submitted to create a new element with the new settings.

    t = TableauServerRest("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="site1")
    t.signin()
    
    t2 = TableauServerRest("http://127.0.0.1", "admin", "adminsp@ssw0rd", site_content_url="duplicate_site1")
    t2.signin()
    
    o_groups = t.groups.query_groups()
    for group in groups:
        new_group_request = t.build_request_from_response(group)
        new_group_luid = t2.groups.create_group(direct_xml_request=new_group_request)

#### 1.3.3 Adding users to a Group
Once users have been created, they can be added into a group via the following method, which can take either a single string or a list/tuple set. Anywhere you see the `"or_luid_s"` pattern in a parameter, it means you can pass a  string or a list of strings to make the action happen to all of those in the list. 

`TableauRestApiConnection.add_users_to_group(username_or_luid_s, group_name_or_luid)`

Ex.

    usernames_to_add = ["user1@example.com", "user2@example.com", "user3@example.com"]
    users_luids = []
    for username in usernames_to_add:
        new_luid = t.add_user_by_username(username, site_role="Interactor")
        users_luids.append(new_luid)
    
    new_group_luid = t.create_group("Awesome People")
    t.add_users_to_group_by_luid(users_luids, new_group_luid)

#### 1.3.4 Update Methods
If you want to make a change to an existing piece of content on the server, there are methods that start with `"update_"`. Many of these use optional keyword arguments, so that you only need to specify what yo'd like to change.

Here's an example for updating a datasource:
`TableauRestApiConnection.update_datasource(name_or_luid, new_datasource_name=None, new_project_luid=None,
                          new_owner_luid=None, proj_name_or_luid=False)`

Note that if you want to change the actual content of a workbook or datasource, that requires a `Publish` action with Overwrite set to True                          
                          
#### 1.3.5 Deleting / Removing Content
Methods with `"remove_"` are used for user membership, where the user still exists on the server at the end.

`TableauRestApiConnection.remove_users_from_site(username_or_luid_s)`

`TableauServerRest.groups.remove_users_from_site(username_or_luid_s))`

`TableauRestApiConnection.remove_users_from_group(username_or_luid_s, group_name_or_luid)`

`TableauServerRest.groups.remove_users_from_group(username_or_luid_s, group_name_or_luid)`

Methods that start with `"delete_"` truly delete the content 

`TableauRestApiConnection.delete_workbooks(wb_name_or_luid_s)`

`TableauRestApiConnection.delete_projects(project_name_or_luid_s)`
etc.

#### 1.3.6 Deleting a Site
The method for deleting a site requires that you first be signed into that site

`TableauRestApiConnection.delete_current_site()`

If you are testing a script that creates a new site, you might use the following pattern to delete the existing version before rebuilding it:

    d = TableauRestApiConnection(server, username, password, site_content_url='default')
    d.signin()
    d.enable_logging(logger)
    
    new_site_content_url = "my_site_name"
    try:
        print("Attempting to create site {}".format(new_site_content_url))
        d.create_site(new_site_content_url, new_site_content_url)
    except AlreadyExistsException:
        print("Site replica already exists, deleting bad replica")
        t = TableauRestApiConnection(server, username, password, site_content_url=new_site_content_url)
        t.enable_logging(logger)
        t.signin()
        t.delete_current_site()

    d.signin()
    d.create_site(new_site_content_url, new_site_content_url)

    print("Logging into {} site".format(new_site_content_url))
    t = TableauRestApiConnection(server, username, password, site_content_url=new_site_content_url)
    t.signin()
    t.enable_logging(logger)

#### 1.3.7 Schedules (Extract and Subscriptions)
You can add or delete schedules for extracts and subscriptions. While there is a generic TableauRestApiConnection.create_schedule() method , the unique aspects of each type schedule make it better to use the helper factory methods that specifically create the type of schedule you want:

`TableauRestApiConnection.create_daily_extract_schedule(name, start_time, priority=1, parallel_or_serial='Parallel')`

`TableauRestApiConnection.create_daily_subscription_schedule(name, start_time, priority=1, parallel_or_serial='Parallel')`
    
`TableauRestApiConnection.create_weekly_extract_schedule(name, weekday_s, start_time, priority=1, parallel_or_serial='Parallel')`

`TableauRestApiConnection.create_weekly_subscription_schedule(name, weekday_s, start_time, priority=1, parallel_or_serial='Parallel')`
  
`TableauRestApiConnection.create_monthly_extract_schedule(name, day_of_month, start_time, priority=1, parallel_or_serial='Parallel')`

`TableauRestApiConnection.create_monthly_subscription_schedule(name, day_of_month, start_time, priority=1, parallel_or_serial='Parallel')`

`TableauRestApiConnection.create_hourly_extract_schedule(name, interval_hours_or_minutes, interval, start_time, end_time, priority=1, parallel_or_serial='Parallel')`

`TableauRestApiConnection.create_hourly_subscription_schedule(name, interval_hours_or_minutes, interval, start_time, end_time, priority=1, parallel_or_serial='Parallel')`

All of these methods live in the `TableauServerRest.subscriptions` sub-object when using `TableauServerRest`

The format for start_time and end_time is `'HH:MM:SS'` like '13:15:30'. Interval can actually take a list, because Weekly schedules can run on multiple days. Priority is an integer between 1 and 100

You can delete an existing schedule with:

`TableauRestApiConnection.delete_schedule(schedule_name_or_luid)`

You can update an existing schedule with:

`TableauRestApiConnection.update_schedule(schedule_name_or_luid, new_name=None, frequency=None, parallel_or_serial=None, priority=None, start_time=None, end_time=None, interval_value_s=None, interval_hours_minutes=None)`

One use case for updating schedules is to enable or disable the schedule. There are two methods for doing just this action:

`TableauRestApiConnection.disable_schedule(schedule_name_or_luid)`

`TableauRestApiConnection.enable_schedule(schedule_name_or_luid)`

If you want to create a new schedule and then disable it, combine the two commands:

    sched_luid = t.create_daily_extract_schedule('Afternoon Delight', start_time='13:00:00')
    t.disable_schedule(sched_luid)


Ex. 

    try:
        t.log('Creating a daily extract schedule')
        t.create_daily_extract_schedule('Afternoon Delight', start_time='13:00:00')

        t.log('Creating a monthly subscription schedule')
        new_monthly_luid = t.create_monthly_subscription_schedule('First of the Month', '1',
                                                                       start_time='03:00:00', parallel_or_serial='Serial')
        t.log('Creating a monthly extract schedule')
        t.create_monthly_extract_schedule('Last Day of Month', 'LastDay', start_time='03:00:00', priority=25)
        t.log('Creating a monthly extract schedule')
        weekly_luid = t.create_weekly_subscription_schedule('Mon Wed Fri', ['Monday', 'Wednesday', 'Friday'],
                                                   start_time='05:00:00')
        time.sleep(4)
        t.log('Deleting monthly subscription schedule LUID {}'.format(new_monthly_luid))
        t.delete_schedule(new_monthly_luid)

        t.log('Updating schedule with LUID {}'.format(weekly_luid))
        t.update_schedule(weekly_luid, new_name='Wed Fri', interval_value_s=['Wednesday', 'Friday'])

    except AlreadyExistsException as e:
        t.log('Skipping the add since it already exists')

When looking for Schedules to use for Subscriptions and Extracts, there are the following querying methods

    TableauRestApiConnection.query_extract_schedules()
    TableauRestApiConnection.query_subscription_schedules()
    TableauRestApiConnection.query_schedules()
    TableauRestApiConnection.query_schedule_luid(schedule_name)
    TableauRestApiConnection.query_schedule(schedule_name_or_luid)

These methods all live in the 
`TableauServerRest.schedules`

sub-object of `TableauServerRest`

Not much reason to ever use the plain `query_schedules()` and have them mixed together. Schedules have unique names so there is no need to specify extract or subscription when asking individually.

#### 1.3.8 Subscriptions
You can subscribe a user to a view or a workbook on a given subscription schedule. This allows for mass actions such as subscribing everyone in a group to a given view or workbook, or removing subscriptions to old content and shifting them to new content.

All of the following methods exist under
`TableauServerRest.subscriptions` as well:

`TableauRestApiConnection.create_subscription_to_workbook(subscription_subject, wb_name_or_luid, schedule_name_or_luid, username_or_luid, project_name_or_luid=None)`

`TableauRestApiConnection.create_subscription_to_view(subscription_subject, view_name_or_luid, schedule_name_or_luid, username_or_luid, wb_name_or_luid=None, project_name_or_luid=None)`

There is a generic
`TableauRestApiConnection.create_subscription()`
but there the helper functions handle anything it can.

You can update a subscription with 

`TableauRestApiConnection.update_subscription(subscription_luid, subject=None, schedule_luid=None)`

`TableauRestApiConnection.delete_subscriptions(subscription_luid_s)`

You'll note that the update and delete subscriptions methods only take LUIDs, unlike most other methods in tableau_tools. This is because Subscriptions do not have a reasonably unique identifier -- to find the LUID, you would use a combination of things to filter on.

This brings us to how to find subscriptions to do things to via `query_subscriptions`

`TableauRestApiConnection.query_subscriptions(username_or_luid=None, schedule_name_or_luid=None, subscription_subject=None,view_or_workbook=None, content_name_or_luid=None, project_name_or_luid=None, wb_name_or_luid=None)`

You don't have to pass anything to `query_subscriptions()`, and you'll get all of them in the system. However, if you want to filter down to a subset, you can pass any of the parameters, and the filters will be applied successively.


### 1.4 Permissions
The `tableau_rest_api` library handles permissions via the Permissions and `PublishedContent` (`Project`, `Workbook`, `Datasource`) classes, encapsulating all of the necessary logic to make changes to permissions both easy and efficient.

Permissions are by far the most complex issue in the Tableau REST API. Every content object (Project, Workbook or Datasource) can have permissions (known as "capabilities" in the REST API) set for each member object (Group or User). This is represented in the REST API by `granteeCapabilities` XML, which is a relatively complex XML object. Capabilities can also be `"unspecified"`, and if this is the case, they simply are missing from the `granteeCapabilities` XML.

Additionally, there is no "update" functionality for permissions capabilities -- if you want to submit changes, you must first delete out those permissions. Thus any "update" must involve determining the current state of the permissions on an object and removing those permissions before assigning the new permissions. 

The most efficient algorithm for sending an update is thus:

    a. For the given user or group to be updated, see if there are any existing permissions for that user or group
    b. If the existing permissions match exactly, do not make any changes (Otherwise, yo'd have to delete out every permission only to reset it exactly as it was before)
    c. If the permissions do not match exactly, delete all of the existing permissions for that user or group (and only those that are set, therefore saving wasted deletion calls)
    d. Set the new permissions for that user or group

`tableau_rest_api` handles this through two concepts -- the `Permissions` object that represents the permissions / capabilities, and the `PublishedContent` classes, which represent the objects on the server that have permissions.

#### 1.4.1 PublishedContent Classes (Project, Workbook, Datasource, View, Database, Table)
Because of the complexity of Permissions, there are classes that represent the state of published content to a server; they all descend from the PublishedContent class, but there is no reason to ever access `PublishedContent` directly. Each of these require passing in an active and signed-in `TableauRestApiConnection` or `TableauServerRest` object so that they can perform actions against the Tableau Server.

Project obviously represents a project on the server. But a `Project` also contains a child `Workbook` and `Datasource` object that represent the Default Permissions that can be set for that project, and starting with `Project33` contains a Flow object as well to represent its defaults. 

Starting in API 3.2 (2018.3+), there is a View object which represents published Views that were not published as Tabs. Since views have the same permissions as workbooks, use the WorkbookPermissions objects just like with the Workbook object.

The `TableauRestApiConnection` and `TableauServerRest` classes have factory methods to get you the right object type for the right version of the API. Use these instead of constructing the objects directly.

`TableauRestApiConnection.get_published_project_object(project_name_or_luid)`

`TableauRestApiConnection.get_published_datasource_object(datasource_name_or_luid, project_name_or_luid)`

`TableauRestApiConnection.get_published_workbook_object(workbook_name_or_luid, project_name_or_luid)`


For Projects, since the standard `query_project()` method returns the Project object, so you can just use that method rather than the longer factory method above.

Projects have additional commands that the other classes do not:

`Project.lock_permissions() -> Project`

`Project.unlock_permission() -> Project`

`Project.are_permissions_locked()`

If you are locking or unlocking permissions, you should replace the project object you used with the response that comes back:

    proj = t.projects.query_project('My Project')
    proj = proj.lock_permissions()  # You want to updated object returned here to use from here on out
    ... 

You access the default permissions objects with the following, which reference the objects of the correct type that have already been built within the Project object:

`Project.workbook_defaults`

`Project.datasource_defaults`

Project28 expands the project definition to include a Parent Project LUID

Project33 expands the project definition to include a `.flow_defaults` sub-object for setting Default Permissions for Flows.

`Project33.flow_defaults`

#### 1.4.2 Permissions Classes
Any time you want to set or change permissions, you must use one of the `Permissions` classes to represent that set of permissions/capabilities available. You do not need to construct them directly, as below. Instead please use the factory method mentioned directly after:


`WorkbookPermissions(group_or_user, group_or_user_luid)`

`DatasourcePermissions(group_or_user, group_or_user_luid)`

`ProjectPermissions(group_or_user, group_or_user_luid)`

`WorkbookPermissions28(group_or_user, group_or_user_luid)`

`DatasourcePermissions28(group_or_user, group_or_user_luid)`

`ProjectPermissions28(group_or_user, group_or_user_luid)`

`FlowPermission33(group_or_user, group_or_user_luid)`

`DatabasePermissions35(group_or_user, group_or_user_luid)`

`TablePermission35(group_or_user, group_or_user_luid)`

Any PublishedContent object (Project, Workbook, etc.) will return the correct Permissions object type for itself with the following method. The optional `role` parameter sets the permissions to match one of the named roles in Tableau Server. It is a shortcut to the `set_capabilities_to_match_role` method. 

    get_permissions_obj(group_name_or_luid: Optional[str] = None, username_or_luid: Optional[str] = None, role: Optional[str] = None)

So if you want to set the default permissions for workbooks in a project, you can do

    proj_obj = t.query_project('My Project')
    wb_perms_1 = proj_obj.workbook_defaults.get_permissions_obj(group_name_or_luid='My Favorite Group')
    wb_perms_1.set_all_to_allow()
    wb_perms_1.set_capability_to_deny('Download Full Data')
    proj_obj.workbook_defaults.set_permissions(permissions=[wb_perms, ])

For compatibility with older releases, the `Project` classes still contain these much longer legacy methods that bring back permissions objects of the different class types. Please use the `get_permissions_obj()` method in newer code. :

`Project.create_datasource_permissions_object_for_group(group_name_or_luid, role=None)`

`Project.create_workbook_permissions_object_for_group(group_name_or_luid, role=None)`

`Project.create_project_permissions_object_for_group(group_name_or_luid, role=None)`

`Project.create_datasource_permissions_object_for_user(username_or_luid, role=None)`

`Project.create_workbook_permissions_object_for_user(username_or_luid, role=None)`

`Project.create_project_permissions_object_for_user(username_or_luid, role=None)`

`Project33.create_flow_permissions_object_for_group(group_name_or_luid, role=None)`

`Project33.create_flow_permissions_object_for_user(username_or_luid, role=None)`


#### 1.4.2 Setting Capabilities
The Permissions classes have methods for setting capabilities individually, or matching the selectable "roles" in the Tableau Server UI. 

There are actually three states: "Allow", "Deny" and "Unspecified". While there is an underlying method for setting them:
`Permissions.set_capability(capability_name, mode)`

You are better off using the specific methods: 

`Permissions.set_capability_to_allow(capability_name)`
`Permissions.set_capability_to_deny(capability_name)`
`Permissions.set_capability_to_unspecified(capability_name)`

There are two quick methods for all to allow or all to deny:

`Permissions.set_all_to_deny()`
`Permissions.set_all_to_allow()`

There is also a method to match the roles from the Tableau Server UI. It is aware of both the api version and the content_type, and will give you an error if you choose a role that is not available for that content type ("Project Leader" on a Workbook, for example)

`Permissions.set_capabilities_to_match_role(role)`

Ex. 
    
    proj = t.query_project('My Project')
    best_group_perms_obj = proj.get_permissions_obj(group_name_or_luid='Best Group')
    best_group_perms_obj.set_capabilities_to_match_role("Publisher")
    # alternatively, you can set this in the factory method
    # best_group_perms_obj = proj.get_permissions_obj(group_name_or_luid='Best Group', role='Publisher')

#### 1.4.2 Permissions Setting
All of the PublishedContent classes (Workbook, ProjectXX and Datasource) inherit the following method for setting permissions:

`PublishedContent.set_permissions_by_permissions_obj_list(new_permissions_obj_list)`

There is also a method to clear all permissions for a given object:

`PublishedContent.clear_all_permissions()`

Project(28, 33) has an additional optional parameter to control if the defaults should be cleared as well:

`Project.clear_all_permissions(clear_defaults=True)`

This method does all of the necessary checks to send the simplest set of calls to update the content object. It takes a list of Permissions objects and compares against any existing permissions to add or update as necessary.

Ex.

        proj = t.query_project('My Project')
        best_group_perms_obj = proj.get_permissions_obj(group_name_or_luid='Best Group')
        best_group_perms_obj.set_capabilities_to_match_role("Publisher")
        proj.set_permissions(permissions=[best_group_perms_obj, ]) # Note creating a list for singular item
    
    # Setting default permissions for workbook
    best_group_perms_obj = proj.workbook_defaults.get_permissions_obj(group_name_or_luid='Best Group')
    best_group_perms_obj.set_capabilities_to_match_role("Interactor")
    proj.workbook_defaults.set_permissions(permissions=[best_group_perms_obj, ])
    
    # Setting default permissions for data source
    best_group_perms_obj = proj.datasource_defaults.get_permissions_obj(group_name_or_luid='Best Group', role='Editor')
    proj.datasource_defaults.set_permissions(permissions=[best_group_perms_obj, ])

#### 1.4.3 Reusing Permissions Objects
If you have a Permissions object that represents a set of permissions you want to reuse, you should use the two copy methods here, which create actual new Permissions objects with the appropriate changes:

`PublishedContent.copy_permissions_obj_for_group(perms_obj, group_name_or_luid)`

`PublishedContent.copy_permissions_obj_for_user(perms_obj, username_or_luid)`

Ex.

    best_group_perms_obj = proj.create_datasource_permissions_object_for_group('Best Group', role='Editor')
    second_best_group_perms_obj = proj.copy_permissions_obj_for_group(best_group_perms_obj, 'Second Best Group')
    
    # Transform to user from group
    my_user_perms_obj = proj.copy_permissions_obj_for_user(second_best_group_perms_obj, 'My User Name')
    
    # Set on proj
    proj.clear_all_permissions()
    proj.set_permissiosn_by_permissions_obj_list([best_group_perms_obj, second_best_group_perms_obj, my_user_perms_obj])

#### 1.4.4 Replicating Permissions from One Site to Another
 -- There is an included example script "replicate_site_structure_sample.py" which shows this in action
The PublishedContent class has a method called 
PublishedContent.convert_permissions_obj_list_from_orig_site_to_current_site(permissions_obj_list, orig_site)

orig_site is a TableauRestApiConnection class object that is a signed-in connection to the original site. This allows the method to translate the names of Groups and Users from the Originating Site to the site where the PublishedContent lives. In most cases, yo'll do this on a Project object. The method returns a list of Permissions objects, which can be put directly into set_permissions_by_permissions_obj_list

Ex.

    orig_proj = o.query_project(proj_name)
    new_proj = n.query_project(proj_name)
    
    # Clear everything on the new one
    new_proj.clear_all_permissions()
    
    # Project Permissions
    o_perms_obj_list = orig_proj.current_perms_obj_list
    n_perms_obj_list = new_proj.convert_permissions_obj_list_from_orig_site_to_current_site(o_perms_obj_list, o)
    new_proj.set_permissions_by_permissions_obj_list(n_perms_obj_list)
    
    # Workbook Defaults
    o_perms_obj_list = orig_proj.workbook_defaults.current_perms_obj_list
    n_perms_obj_list = new_proj.workbook_defaults.convert_permissions_obj_list_from_orig_site_to_current_site(o_perms_obj_list, o)
    new_proj.workbook_defaults.set_permissions_by_permissions_obj_list(n_perms_obj_list)
    
    # Project Defaults
    o_perms_obj_list = orig_proj.datasource_defaults.current_perms_obj_list
    n_perms_obj_list = new_proj.datasource_defaults.convert_permissions_obj_list_from_orig_site_to_current_site(o_perms_obj_list, o)
    new_proj.datasource_defaults.set_permissions_by_permissions_obj_list(n_perms_obj_list)


### 1.5 Publishing Content
The Tableau REST API can publish both data sources and workbooks, either as TWB / TDS files or TWBX or TDSX files. It actually has two different methods of publishing; one as a single upload, and the other which chunks the upload. tableau_rest_api encapsulates all this into two methods that detect the right calls to make. The default threshold is 20 MB for a file before it switches to chunking. This is set by the "single_upload_limit" variable. 

If a workbook references a published data source, that data source must be published first. Additionally, unlike Tableau Desktop, the REST API will not find linked files and upload them. A workbook with a "live connection" to an Excel file, for example, must be saved as a TWBX rather than a TWB for an upload to work correctly. The error messages if you do not follow this order are not very clear. 

#### 1.5.1 Publishing a Workbook or Datasource
The publish methods must upload directly from disk. If you are manipulating a workbook or datasource using the TableauFile / TableauDocument classes, please save the file prior to publishing. Also note that you specify a Project object rather than the LUID. Publishing methods live under their respective sub-objects in TableauServeRest: .workbooks, .datasources or .flows


`TableauRestApiConnection.publish_workbook(workbook_filename, workbook_name, project_obj, overwrite=False, connection_username=None, connection_password=None, save_credentials=True, show_tabs=True, check_published_ds=True)`

`TableauRestApiConnection.publish_datasource(ds_filename, ds_name, project_obj, overwrite=False, connection_username=None, connection_password=None, save_credentials=True)`

The `check_published_ds` argument for publish_workbook causes the tableau_document sub-module to be used to open up and look to see if there are any published data sources in the workbook. If there are, it changes the Site Content URL property to match the site that the workbook is being published to. The only reason to choose False for the argument is if you know you are not using any Published Data Sources or you using the more thorough process described below (where those changes would already be made)

##### 1.5.1.1 Workbooks Connected to Published Data Sources 
Tableau Server only requires unique names for Workbooks and Data Sources within a Project, rather than within the Site. This means you can have multiple workbooks or data sources with the same “visible name”. Internally, Tableau Server generates a unique “contentUrl” property using a pattern which removes spaces and other characters, and appends numbers if the pattern would result in overwriting any existing contentUrl. However, there is no guarantee that you will get the same contentUrl from Site to Site, since the publish order could result in the numbering being different.

To publish a workbook connected to Published Data Sources, you need to be aware of what the Destination contentUrl property will be of any Data Source, and then substitute that value into the definition of the Published Data Source in the workbook file prior to publish (in addition to the Site Content Url, which otherwise would be handled automatically). This is done using tableau_documents, which is covered in section 2 of this README.

There's a very thorough explanation of this availabe at 
<https://tableauandbehold.com/2018/05/17/replicating-workbooks-with-published-data-sources/>. 

The example code to do this process correctly is included in the template_publish_sample.py using the function `replicate_workbooks_with_published_dses`

Here is an example of using that function

    orig_server = 'http://'
    orig_username = ''
    orig_password = ''
    orig_site = 'default'
    
    dest_server = ''
    dest_username = ''
    dest_password = ''
    dest_site = 'publish_test'
    
    o = TableauRestApiConnection28(server=orig_server, username=orig_username, password=orig_password,
                                   site_content_url=orig_site)
    o.signin()
    o.enable_logging(logger)
    
    d = TableauRestApiConnection28(server=dest_server, username=dest_username, password=dest_password,
                                   site_content_url=dest_site)
    d.signin()
    d.enable_logging(logger)
    
    wbs_to_replicate = ['Workbook Connected to Published DS', 'Connected to Second DS']
    o_wb_project = 'Default'
    d_wb_project = 'Default'
    
    replicate_workbooks_with_published_dses(o, d, wbs_to_replicate, o_wb_project, d_wb_project)

#### 1.5.2 Workbook and Datasource Revisions
If revision history is turned on for a site, allowing you to see the changes that are made to workbooks over time. Workbook and datasource revisions are identified by a number that counts up starting from 1. So if there has only ever been one publish action, there is only revision 1.

The REST API does not have a method for "promote to current". This means to restore to a particular revision you have two options:
    1) Delete all revisions that come after the one you want to be the current published workbook or datasource
    2) Download the revision you want to be current, and then republish it
    
To see the existing revisions, use

`TableauRestApiConnection.get_workbook_revisions(workbook_name_or_luid, username_or_luid=None, project_name_or_luid=None)`

`TableauRestApiConnection.get_datasource_revisions(datasource_name_or_luid, project_name_or_luid=None)`

You can remove revisions via 

`TableauRestApiConnection.remove_workbook_revision(wb_name_or_luid, revision_number, project_name_or_luid=None, username_or_luid=None)`

`TableauRestApiConnection.remove_datasource_revision(datasource_name_or_luid, revision_number, project_name_or_luid=None)`

You can download any revision as a file using methods that mirror the standard download workbook and datasource methods.

`TableauRestApiConnection.download_datasource_revision(ds_name_or_luid, revision_number, filename_no_extension, proj_name_or_luid=None)`

`TableauRestApiConnection.download_workbook_revision(wb_name_or_luid, revision_number, filename_no_extension, proj_name_or_luid=None)`

All of the revision methods live under
`TableauServerRest.revisions`

#### 1.5.3 Asynchronous Publishing (API 3.0+)
In API 3.0+ (Tableau 2018.1 and above), you can publish workbooks asychronously, so you can move on to other actions after you have pushed up the bits to the server.

The publish_workbook method has a new "async_publish" argument -- simply set it to True to publish async.

`TableauRestApiConnection30.publish_workbook(workbook_filename, workbook_name, project_obj, overwrite=False, async_publish=False, connection_username=None, connection_password=None, save_credentials=True, show_tabs=True, check_published_ds=False)`

When you choose async_publish, rather than returning a workbook tag in the response, you get a job tag. From that job tag, you can pull the `'id'` attribute and then use the `query_job()` method to find out when it has finished.

It appears when the publish completes, the progress attribute goes to 100, and the finishCode attribute turns from 1 to 0. At the time of writing (2018-04-24), there does not appear to be any indication of the LUID of the newly created workbook in the response. Always check the official documentation to determine if this is still the case.

Here's an example of an async publish, then polling every second to see if it has finished:

    proj_obj = t.query_project('Default')
    job_id = t.publish_workbook('A Big Workbook.twbx', 'Big Published Workbook & Stuff', proj_obj, overwrite=True, async_publish=True)
    print('Published async using job {}'.format(job_id))
    
    progress = 0
    while progress < 100:
        job_obj = t.query_job(job_id)
        job = job_obj.findall('.//t:job', t.ns_map)
        # When updating our while loop variable, need to cast progress attribute to int
        progress = int(job[0].get('progress'))
        print('Progress is {}'.format(progress))
        time.sleep(1)
    print('Finished publishing')

### 1.6 Refreshing Extracts

#### 1.6.1 Running an Extract Refresh Schedule 
The TableauRestApiConnection class, representing the API for Tableau 10.3 and above, includes methods for triggering extract refreshes via the REST API.

`TableauRestApiConnection.run_all_extract_refreshes_for_schedule(schedule_name_or_luid) `

runs through all extract tasks related to a given schedule and sets them to run.

If you want to run one task individually, use

`TableauRestApiConnection.run_extract_refresh_for_workbook(wb_name_or_luid, proj_name_or_luid=None, username_or_luid=None)`

`TableauRestApiConnection.run_extract_refresh_for_datasource(ds_name_or_luid, proj_name_or_luid=None, username_or_luid=None)`

You can get all extract refresh tasks on the server using

`TableauRestApiConnection.get_extract_refresh_tasks()`

although if you simply want to set all of the extract schedules to run, use

`TableauRestApiConnection.query_extract_schedules()`

There is equivalent for for subscription schedules:
`TableauRestApiConnection.query_subscription_schedules()`

Ex.

    extract_schedules = t.query_extract_schedules()
    sched_dict = t.convert_xml_list_to_name_id_dict(extract_schedules)
    for sched in sched_dict:
        t.run_all_extract_refreshes_for_schedule(sched_dict[sched])  # This passes the LUID
        # t.run_all_extract_refreshes_for_schedule(sched_dict) # You can pass the name also, it just causes extra lookups

#### 1.6.2 Running an Extract Refresh (no schedule) (10.5/ API 2.8)
In 10.5+, there is a method to update the extract in a published data source without specifying the Schedule Task. The `run_extract_refresh_for_datasource()` method in `TableauRestApiConnection28` automatically takes advantage of this, but it is implemented internaly by calling the new method:

`TableauRestApiConnection28.update_datasource_now(ds_name_or_luid, project_name_or_luid=False)`


#### 1.6.3 Putting Published Content on an Extract Schedule (10.5+)
Starting in Tableau 10.5 (API 2.8), you can put a workbook or datasource on an Extract Refresh Schedule using the REST API.

`TableauRestApiConnection28.add_workbook_to_schedule(wb_name_or_luid, schedule_name_or_luid, proj_name_or_luid)`
`TableauRestApiConnection28.add_datasource_to_schedule(ds_name_or_luid, schedule_name_or_luid, proj_name_or_luid)`


#### 1.6.4 Putting published content on an Extract Schedule Prior to 10.5 (high risk)
Just upgrade your server at this point, that is a long long time to go without an upgrade


### 1.7 Data Driven Alerts (2018.3+)
Starting in API 3.2 (2018.3+), you can manage Data Driven Alerts via the APIs. The methods for this functionality follows the exact naming pattern of the REST API Reference.

These methods live under `TableauServerRest.alerts` when using `TableauServerRest`

### 1.8 Tableau Prep Flows (2019.1+)
Starting in API 3.3 (2019.1+), Tableau Prep flows can be published and managed through the REST API. For Permissions on Published Flows, see Section 1.4 which describes all of the PublishedContent methods. 

The methods for flows live under `TableauServerRest.flows` in `TableauServerRest`
    
### 1.9 Favorites
Favorites live as their own items in Tableau Server. You can access the methods to set Favorites under `TableauServerREst.favorites`. Most of the methods to delete Favorites are plural and thus can take a list

Ex.

    add_workbook_to_user_favorites(favorite_name: str, wb_name_or_luid: str, username_or_luid: str, 
                                    proj_name_or_luid: Optional[str] = None) -> etree.Element
    delete_views_from_user_favorites(view_name_or_luid_s: Union[List[str], str],
                                     username_or_luid: str, wb_name_or_luid: Optional[str] = None)

### 1.10 Metadata (2019.3+)
The Metadata methods are implemented under `TableauServerRest.metata` in `TableauServerRest`. They have not been fully tested in 5.0.0 release. 

Of note, there is a method for accessing the GraphQL API. Note that you send a string (please look at the Tableau GraphQL API documentation for examples of correct queries) but it will return back on object, representing the result of Python running `json.dumps()` on the resulting JSON string returned by GraphQL.

    graphql(graphql_query: str) -> Dict

### 1.11 Webhooks (2019.4+)
The Webhooks methods are implemented under `TableauServerRest.webhooks` in `TableauServerRest`. They have not been fully tested in 5.0.0 release. 

## 2 tableau_documents: Modifying Tableau Documents (for Template Publishing)
tableau_documents implements some features that go beyond the Tableau REST API, but are extremely useful when dealing with a large number of workbooks or datasources, particularly for multi-tenented Sites. It also provides a mechanism for utilizing newly updated Hyper files generated by Extract API or Hyper API to update existing TWBX and TDSX files. These methods actually allow unsupported changes to the Tableau workbook or datasource XML. If something breaks with them, blame the author of the library and not Tableau Support, who won't help you with them.

### 2.0 Getting Started with tableau_documents: TableauFileOpener class
tableau_documents is a sub-package of the main tableau_tools library. It is only imported if you need it, using:

    from tableau_tools.tableau_documents import *
    
The class you will use to handle existing classes is TableauFileOpener. This is a class full of static methods for detecting if something is a Tableau file and opening it as the correct object type, among other features. Use the `.open(filename)` method to get back one of the objects that represents a Tableau file. If you know the file type, you can annotate the variable for code completion. If not, don't worry, there ways to handle all files without knowing ahead of time:

    from tableau_tools.tableau_documents import *
    t_file: TDS = TableauFileManager.open(filename='Live PostgreSQL Connection.tds')
    if isinstance(t_file, DatasourceFileInterface):
       

### 2.1 tableau_documents basic model
In 5.0+, tableau_documents has been updated considerably with a more consistent model than in the past. There is a hierarchy of the objects, which reflects a model of "Tableau XML File on Disk" -> "Object that manipulates the XML, built from File". For Tableau's packaged file types (the ones that end in X), there is an additional layer, which is the ZIP file that contains the XML File. The TableauWorkbook and TableauDatasource objects both inherit from TableauDocument, which just defines certain methods they both share. 

Datasource:

* TDS (TableauXmlFile, DatasourceFileInterface)
    * TableauDatasource (TableauDocument)
        * [TableauConnection]
        * TableauColumns

* TDSX (TableauPackagedFile, DatasourceFileInterface)
    * TDS (TableauXmlFile, DatasourceFileInterface)
        * TableauDatasource (TableauDocument)
            * [TableauConnection]
            * TableauColumns
    
Workbooks:

* TWB (TableauXmlFile, DatasourceFileInterface)
    * TableauWorkbook (TableauDocument)
        * [TableauDatasource]
            * [TableauConnection]
            * TableauColumns

* TWBX (TableauPackagedFile, DatasourceFileInterface)
    * TWB (TableauXmlFile, DatasourceFileInterface)
        * TableauWorkbook (TableauDocument)
            * [TableauDatasource]
                * [TableauConnection]
                * TableauColumns

Flows:
 (not available and tested yet)    

You'll note in parentheses that some of these classes do descend from the same abastract parent classes. This means they have the same methods available, despite being different classes. In particular, DatasourceFileInterface allows you to access the datasource objects stored within any of the object types without worrying about the hierarchy of the individual object. We'll explore the effects of this in one of the next sections.

### 2.2 TableauXmlFile Classes (TDS, TWB, TFL)
The TableauXmlFile class represents one single existing Tableau file on disk (.tds, .twb, .tfl ). Use `TableauFileOpener.open()` to return the correct object for the type of file you are opening. The main function of the TableauXmlFile objects is to handle reading from and writing to disk correctly.

There is a `tableau_document` property to any TableauXmlFile object which gives access to the TableauWorkbook, TableauDatasource, or TableauFlow object inside. Other than the `TableauParameters` object of a TableauWorkbook (allowing you to set or changed Parameter definitions), you will most likely use the `.datasources` property rather than traversing the full object tree (since this has different depth depending on the object type).
 

You can save a new file (including any changes you would have made to the underlying tableau_document objects) using 

    TableauXmlFile.save_new_file(filename_no_extension: str, save_to_directory: Optional[str] = None)


It automatically appends the correct extension, so you don't need to include when giving the save name.

If a file is found on disk with the same name, a number will be appended to the end.

ex.

    tf = TableauFileOpener('A Workbook.twb')
    file_1 = tf.save_new_file('A Workbook')
    file_2 = tf.save_new_file('A Workbook')
    print(file_1)
    # 'A Workbook (1).twb'
    print(file_2)
    # 'A Workbook (2).twb'

### 2.3 TableauPackagedFile Classes (TDSX, TWBX, TFLX)
When publishing to the REST API, the most common file type is actually a Tableau Packaged file, which is really just a ZIP file with a particular structure and a different file ending. 

Within these ZIP files exists a Tableau XML file (.tds, .twb, .tfl) and any associated assets that are needed for publishing, including Extract files and static file sources like Excel or CSV files. 

The TableauPackagedFile classes give you access to the TableauXmlFile object through the property:

    TableauPackagedFile.tableau_xml_file
    
As well as have functionality for working with the static files (see next session)

There is a save_new_file method:

    TableauPackagedFile.save_new_file(new_filename_no_extension: str)
    
which works similiarly to the save_new_file function of TableauXmlFile, in that it will append a number rather than overwrite an existing file. It will actually call the underlying TableauXmlFile object, which means any changes you have made will be saved into the file placed into the final packaged file. 

#### 2.3.1 Replacing Static Data Files
`TableauPackagedFile` maintains an internal dictionary of files to replace during the `save_new_file()` process. This is useful for swapping in Hyper files or different CSV or Excel files (and potentially anything else stored in an packaged workbook).

    TableauPackagedFile.get_filenames_in_package() -> List[str]
    
will tell you the names of any file that lives within the ZIP directory structure. Given that name, you can set it for replacement with another file from disk using

    TableauPackagedFile.set_file_for_replacement(self, filename_in_package: str, replacement_filname_on_disk: str)

When you call `save_new_file()`, the replacement file from disk will be written into the new packaged file on disk with the original name as it was in the packaged. If there was no original file by that name, it will be placed into the packaged file (not sure the use for this, but it is possible) 


### 2.4 TableauDatasource Class and the DatasourceFileInterface
The TableauDatasource class is represents the XML contained within a TDS or an embedded datasource within a TWB file. 

Any class which implements the DatasourceFileInterface class (TWB, TWBX, TDS, TDSX) make a list of all included TableauDatasource objects available via the `datasources` property. 

You can also transverse the various object types to get to the inner TableauDatasources, but this is usually unnecessary. 

You would only initialize a `TableauDatasource` object directly when creating a datasource from scratch in which case you initialize it like:

`TableauDatasource(datasource_xml=None, logger_obj=None, ds_version=None)`

ex. 

    logger = Logger('ds_log.txt')
    new_ds = TableauDatasource(logger_obj=logger)
    
#### 2.4.1 Iterating through .datasources
The main pattern for accessing datasources from any of the objects is

    DatasourceFileInterface.datasources

This abstraction allows you to iterate through all different file types without worrying about what they are and use the same code to make changes to the datasource properties. 

Ex.

    a_logger = Logger('my_log.log')
    list_o_files = ['A Twb.twb', 'A TDSX.tdsx', 'An TWBX.twbx', 'This here TDS.tds']
    for file in list_o_files:
        t_file = TableauFileManager.open(filename=file, logger_obj=a_logger)
        # This just makes sure you can do these actions. You could also catch and ignore exceptions I guess
        if isinstance(t_file, DatasourceFileInterface):
            datasources = t_file.datasources
            for ds in datasources:
                # do some stuff to the data source
                for conn in ds.connections:
                    # Do some stuff to the connection

#### 2.4.2 Published Datasources in a Workbook
Datasources in a workbook come in two types: Embedded and Published. An embedded datasource looks just like a standard TDS file, except that there can be multiple in a workbook. Published Datasources have an additional tag called `<repository-location>` which tells the information about the Site and the published Datasource name

To see if a datasource is published, use the following property to check:
`TableauDatasource.is_published -> bool` 

If is_published is True, you can get or set the Site of the published DS. This was necessary in Tableau 9.2 and 9.3 to publish to different sites, and it still might be best practice, so that there is no information about other sites passed in (see notes)

ex.

    twb = TableauFileManager('My TWB.twb')
    dses = twb.datasources
    for ds in dses:
        if ds.is_published is True:
            print(ds.published_ds_site)
            # Change the ds_site
            ds.published_ds_site = 'new_site'  # Remember to use content_url rather than the pretty site name

#### 2.4.3 TableauConnection Class: Most of the stuff you want to change
Since Tableau version 10, a single datasource can have any number of federated connections. 

The TableauConnection class represents the connection to the datasource, whether it is a database, a text file. It should be created automatically for you through the `TableauDatasource` object. 

You can access and set all of the relevant properties for a connection, using the following properties

`TableauConnection.server`

`TableauConnection.dbname`

`TableauConnection.schema  # equivalent to dbname. Actual XML does vary -- Oracle has schema attribute while others have dbname. Either method will do the right thing`

`TableauConnection.port`

`TableauConnection.connection_type`

`TableauConnection.sslmode`

`TableauConnection.authentication`

If you are changing the dbname/schema on certain datasource types (Oracle and Teradata for sure, but possibly others), Tableau saves a reference to the database/schema name in the table name identifier as well. This attribute is actually stored in the relations tags in the datasource object directly (above the level of the connection), so yo'll want to also call the following method:

`TableauDatasource.update_tables_with_new_database_or_schema(original_db_or_schema, new_db_or_schema)`

When you set using these properties, the connection XML will be changed when the save method is called on the TableauDatasource object.

ex.

    twb = TableauFile('My TWB.twb')
    dses = twb.tableau_document.datasources
    for ds in dses:
        if ds.published is not True:  # See next section on why you should check for published datasources
            ds.update_tables_with_new_database_or_schema('test_db', 'production_db')  # For systems where db/schema is referenced in the table identifier
            for conn in ds.connections:
                if conn.dbname == 'test_db':
                    conn.dbname = 'production_db'
                    conn.port = '5128'
    
                    
    twb.save_new_file('Modified Workbook')

#### 2.4.4 TableauColumns Class
A TableauDatasource will have a set of column tags, which define the visible aliases that the end user sees and how those map to the actual columns in the overall datasource. Calculations are also defined as a column, with an additional calculation tag within. These tags to do not have any sort of columns tag that contains them; they are simply appended near the end of the datasources node, after all the connections node section.

The TableauColumns class encapsulates the column tags as if they were contained in a collection. The TableauDatasource object automatically creates a TableauColumns object at instantiation, which can be accessed through the `TableauDatasource.columns` property. 

Columns can have an alias, which is conveniently represented by the `caption` attribute. They also have a `name` attribute which maps to the actual name of the column in the datasource itself. For this reason, the methods that deal with "Column Names" tend to search through both the name and the caption attributes.

The primary use case for adjusting column tags is to change the aliases for translation. To achieve this, there is a method designed to take in a dictionary of names to search and replace: 

`TableauColumns.translate_captions(translation_dict)`

The dictionary should be a simple mapping of the caption from the template to the caption you want in the final translated version. Some customers have tokenized the captions to make it obvious which is the template:

    english_dict = { '{token1}': 'category', '{token2}': 'sub-category'}
    german_dict = { '{token1}': 'Kategorie', '{token2}': 'Unterkategorie'}
    tab_file = TableauFileManager('template_file.tds')
    dses = tab_file.datasources 
    for ds in dses:
        ds.columns.translate_captions(english_dict)
    new_eng_filename = tab_file.save_new_file('English Version')
    # Reload template again
    tab_file = TableauFileManager('template_file.tds')
    dses = tab_file.datasources 
    for ds in dses:
        ds.columns.translate_captions(german_dict)
    new_ger_filename = tab_file.save_new_file('German Version')

#### 2.4.5 Modifying Table JOIN Structure in a Datasource
The way that Tableau stores the relationships between the various tables, stored procedures, and custom SQL definitions is fairly convoluted. It is explained in some detail here https://tableauandbehold.com/2016/06/29/defining-a-tableau-data-source-programmatically/ .

The long and short of it is that the first / left-most table you see in the Data Connections pane in Tableau Desktop is the "main table"  which other relations connect to. At the current time , tableau_tools can consistently identify and modify this "main table", which suffices for the vast majority of data source swapping use cases.

There is a TableRelations (note not TableauRelations) object within each TableauDatasource object, representing the table names and their relationships, accesible through the `.tables` property. Any methods for modifying will go through `TableauDatasource.tables`

You can access the main table relationship using

`TableauDatasource.tables.main_table`

To see if the datasource is connected to a Stored Procedure, check

`TableauDatasource.is_stored_procedure: bool`

like:

    tab_file = TableauFileManager('template_file.tds')
        dses = tab_file.datasources 
        for ds in dses:
            if ds.is_stored_procedure:
                # do stored proc things

You can also determine the type of the main table, using:

`TableauDatasource.main_table_type`

which should return either `'table', 'stored-proc' or 'custom-sql'`

##### 2.5.5.1 Database Table Relations
If the type is 'table', then unsurprisingly this relation represents a real table or view in the database. The actual table name in the database is stored in a 'table' attribute, with brackets around the name (regardless of the database type).

Access and set via property:
`TableauDatasource.tables.main_table_name`

Ex.

    for ds in dses:
        if ds.main_table_type == 'table':
            if ds.tables.main_table_name == '[Test Table]':
                ds.tables.main_table_name = '[Real Data]'

##### 2.4.5.2 Custom SQL Relations
Custom SQL relations are stored with a type of 'text' (don't ask me, I didn't come up with it). The text of the query is stored as the actual text value of the relation tag in the XML, which is also unusual for the Tableau XML files.

To retrieve the Custom SQL itself, use the property to get or set:

`TableauDatasource.tables.main_custom_sql`

Ex.

    for ds in dses:
        if ds.is_custom_sql:
            ds.tables.main_custom_sql = 'SELECT * FROM my_cool_table'

##### 2.4.5.3 Stored Procedure Relations
Stored Procedures are thankfully referred to as 'stored-proc' types in the XML, so they are easy to find. Stored Procedures differ from the other relation types by having parameters for the input values of the Stored Procedure. They also can only connect to that one Stored Procedure (no JOINing of other tables or Custom SQL). This means that a Stored Procedure Data Source only has one relation, the `main_table_relation`.

There are actually two ways to set Stored Procedure Parameters in Tableau -- either with Direct Value or linking them to a Tableau Parameter. Currently, tableau_tools allows you to set Direct Values only.

To see the current value of a Stored Procedure Parameter, use (remember to search for the exact parameter name. If SQL Server or Sybase, include the @ at the beginning):
`TableauDatasource.tables.get_stored_proc_parameter_value_by_name(parameter_name)`

To set the value:
`TableauDatasource.tables.set_stored_proc_parameter_value_by_name(parameter_name, parameter_value)`

For time or datetime values, it is best to pass in a `datetime.date` or `datetime.datetime` variable, but you can also pass in unicode text in the exact format that Tableau's XML uses:

datetime: '#YYYY-MM-DD HH:MM:SS#'
date: '#YYYY-MM-DD#'

Ex.

    for ds in dses:
        if ds.is_stored_proc:
            ds.tables.set_stored_proc_parameter_value_by_name('@StartDate', datetime.date(2018, 1, 1))
            ds.tables.set_stored_proc_parameter_value_by_name('@EndDate', "#2019-01-01#")


\*\*\*NOTE: From this point on, things become increasingly experimental and less supported. However, I can assure you that many Tableau customers do these very things, and we are constantly working to improve the functionality for making datasources dynamically.


#### 2.4.6 Adding Data Source Filters to an Existing Data Source
There are many situations where programmatically setting the values in a Data Source filter can be useful -- particularly if you are publishing data sources to different sites which are filtered per customer, but actually all connect to a common data warehouse table. Even with Row Level Security in place, it's a nice extra security layer to have a Data Source filter that insures the customer will only ever see their data, no matter what.

The `TableauDatasource` class has methods for adding the three different types of data sources.

`TableauDatasource.add_dimension_datasource_filter(column_name, values, include_or_exclude='include', custom_value_list=False)`

`TableauDatasource.add_continuous_datasource_filter(column_name, min_value=None, max_value=None, date=False)`

`TableauDatasource.add_relative_date_datasource_filter(column_name, period_type, number_of_periods=None, previous_next_current='previous', to_date=False)`

One thing to consider is that column_name needs to be the True Database Column name, not the fancy "alias" that is visible in Tableau Desktop. You can see what this field name is in Desktop by right clicking on a field and choosing "Describe" - the "Remote Column Name" will tell you the actual name. You do not need to pass in the square brackets [] around the column_name, this will be done automatically for you.

Values takes a Python list of values, so to send a single value us the ['value', ] syntax

Here is an examples of setting many dimension filters:

    existing_tableau_file = TableauFile('Desktop DS.tds')
    doc = existing_tableau_file.tableau_document
    # This syntax gets you correct type hinting
    dses = doc.datasources  #type: list[TableauDatasource]
    ds = dses[0]
    ds.add_dimension_datasource_filter(column_name="call_category",
                                                      values=["Account Status", "Make Payment"])
    ds.add_dimension_datasource_filter(column_name="customer_name", values=["Customer A", ])
    ds.add_dimension_datasource_filter(column_name="state", values=["Hawaii", "Alaska"], include_or_exclude='exclude')
    mod_filename = existing_tableau_file.save_new_file('Modified from Desktop')

If you add filters to an extract, there functions that work similarly the Data Source Filter functions. These filters are obeyed when the Extract is generated (or refreshed), as opposed to afterwards, so they can help with limited down data sizes. Note: at the current time, multi-table extracts cannot have filters applied, so you must limit them down using Custom SQL or a database view.

`TableauDatasource.add_dimension_extract_filter(column_name, values, include_or_exclude='include', custom_value_list=False)`

`TableauDatasource.add_continuous_extract_filter(column_name, min_value=None, max_value=None, date=False)`

`TableauDatasource.add_relative_date_extract_filter(column_name, period_type, number_of_periods=None, previous_next_current='previous', to_date=False)`

#### 2.4.7 Defining Calculated Fields Programmatically
For certain filters, you made need to define a calculation in the data source itself, that the filter can reference. This is particularly useful for row level security type filters. You'll note that there are a lot of particulars to declare with a given calculation. If you are wondering what values you might need, it might be advised to create the calculation in Tableau Desktop, then save the TDS file and open it in a text editor to take a look.

`TableauDatasource.add_calculation(calculation, calculation_name, dimension_or_measure, discrete_or_continuous, datatype)`

The add_calculation method returns the internally defined name for the calculation, which is necessary if you want to define a Data Source filter against it. This is particularly useful for creating Row Level Security calculations programmatically.

The following is an example:

    # Add a calculation (this one does row level security
    calc_id = ds.add_calculation('IIF([salesperson_user_id]=USERNAME(),1,0) ', 'Row Level Security', 'dimension', 'discrete', 'integer')
    
    # Create a data source filter that references the calculation
    ds.add_dimension_datasource_filter(calc_id, [1, ], custom_value_list=True)


### 2.5 TableauWorkbook class

#### 2.5.1 Creating and Modifying Parameters
Parameters are actually stored in the TWB file as a special type of data source. They don't behave at all like other data sources, so they are modeled differently. If detected, the `TableauParameters` class will be created by the `TableauWorkbook` object during instantiation, and stored as the property:

`TableauWorkbook.parameters`

If a workbook does not have any parameters, you can add a `TableauParameters` object using

`TableauWorkbook.add_parameters_to_workbook()`

which returns the new `TableauParameters` object (equivalent to `TableauWorkbook.parameters`), or will simply return the existing `TableauParameters` object if it already existed.

The parameters themselves are represented via `TableauParameter` objects. Because they have a specific naming convention that depends on the overall `TableauParameters` object, if you want to create a new parameter, use the factory method:

`TableauParameters.create_parameter(name=None, datatype=None, current_value=None)  # Returns a TableauParameter object`

Yo'll need to explicitly add the newly create parameter object back using:

`TableauParameters.add_parameter(parameter)    # parameter is a TableauParameter object`

Parameters have an numbering scheme, which is why you should create and add them through the TableauParameters factory methods rather than directly

You can also delete an existing Parameter by its name/alias:

`TableauParameters.delete_parameter_by_name(parameter_name)`

If you want to modify an existing parameter, use the following method:

`TableauParameters.get_parameter_by_name(parameter_name)  # parameter_name is the name visible to the end user, from the caption attribute.`

If you want to change the name of a parameter, you should delete the existing parameter and then add a new one with the new name. The `TableauParameter` class tracks the parameters via their caption names, and while you can directly change the name of a parameter using the `TableauParameter` class, it will not overwrite the previous one with the different name.

Ex.

    t_file = TableauFile('Workbook with Parameters.twb', logger)
        if t_file.tableau_document.document_type == 'workbook':
            parameters = t_file.tableau_document.parameters  # type: TableauParameters
            p1 = parameters.get_parameter_by_name('Parameter 1')
            print(p1.current_value)
            p1.current_value = 'All'
            new_param = parameters.create_new_parameter('Choose a Number', 'integer', 1111)
            parameters.add_parameter(new_param)


##### 2.5.1.1 TableauParameter class
The actual values and settings of a Tableau Parameter are set using the `TableauParameter` class. When it is instantiated from an existing parameter in the XML of a TWB, all of the values are mapped to their properties, which are the only interface you should use to set or retrieve values.

When you create a `TableauParameter` from scratch, it comes pre-defined as an "all" type parameter, but with no datatype defined (unless you defined the datatype at creation time)

The properties you can set are:

`TableauParameter.name`

`TableauParameter.datatype  # 'string', 'integer', 'datetime', 'date', 'real', 'boolean'`

`TableauParameter.current_value  # Use the alias i.e. the value that is visible to the end user`

You can retrieve what type of `allowable_values` a parameter has using the property

`TableauParameter.allowable_values   # returns either "all", "range", or "list"`

However, the actual value of `allowable_values` is set automatically if you set a range or a list of values.

To set the allowable values:

`TableauParameter.set_allowable_values_to_all()`

`TableauParameter.set_allowable_values_to_range(minimum=None, maximum=None, step_size=None, period_type=None)`

`TableauParameter.set_allowable_values_to_list(list_value_display_as_pairs)`

When using `set_allowable_values_to_list()`, the data structure that is expected is a list of `{value : display_as}` dicts.

Ex.

    tab_params = wb.parameters
    param = tab_params.create_parameter('Semester', 'string')
    # Alternatively:
    # param = tab_params.create_parameter()
    # param.name = 'Semester'
    # param.datatype = 'string'
    allowable_values = [ { "Spring 2018" : "2018-02-01"} , { "Fall 2018" : "2018-09-01" } ]
    param.set_allowable_values_to_list(allowable_values)
    param.set_current_value('Spring 2018')

\*\*\*NOTE: There is functionality for making a datasource from scratch however IT IS NOT FULLY TESTED IN RELEASE 5.0.0. Don't expect it to work, but there is code in there for how it could work (and it has worked in the past). Our best recommendation for now is: 
(1) CREATE A TEMPLATE USING TABLEAU DESKTOP 
(2) MODIFY THAT TEMPLATE FILE BY MAKING CHANGES OR SWAPPING IN FILES 
(3) PUBLISH TO SERVER, THEN USE THE REST API TO TRIGGER AN EXTRACT REFRESH IMMEDIATELY, THEN PUT ON A SCHEDULE


## 3 tabcmd
The Tableau Server REST API can do most of the things that the tabcmd command line tool can, but if you are using older versions of Tableau Server, some of those features may not have been implemented yet. If you need a functionality from tabcmd, the `tabcmd.py` file in the main part of tableau_tools library wraps most of the commonly used functionality to allow for easier scripting of calls (rather than doing it directly on the command line or using batch files)


### 3.1 Tabcmd Class
The Tabcmd class is a wrapper around the actual tabcmd program. It knows how to construct the correct commands and then sends them via the command line automatically for you. The Tabcmd class takes the following for a constructor:

`Tabcmd(tabcmd_folder, tableau_server_url, username, password, site='default', repository_password=None, tabcmd_config_location=None)`

You have to provide the folder\directory where tabcmd lives on the local computer (just the folder, not the location of the file), along with the username and password of the user you want to run the tabcmd commands as. There are two optional parameters which allow for user impersonation when generating exports (not needed to trigger extract refreshes). These parameters are the `"repository_password"` for the `"readonly"` user in the Repository and the `"tabcmd_config_location"`. This is a file that is generated after the first run of tabcmd and lives in a location similar to what you see in the example below:

Ex.

    tabcmd_dir = "C:\\tabcmd\\Command Line Utility\\"
    tabcmd_config_location = 'C:\\Users\\{}\\AppData\\Local\\Tableau\\Tabcmd\\'
    
    server = 'http://127.0.0.1'
    site_content_url = 'default'
    username = '{}'
    password = '{}'
    
    tabcmd = Tabcmd(tabcmd_dir, server, username, password, site=site_content_url, tabcmd_config_location=tabcmd_config_location)

### 3.2 Triggering an Extract Refresh
Prior to 10.3, the only way to programmatically trigger an extract to be refreshed by a Tableau Server was via tabcmd. tabcmd doesn't use any of the REST API "plumbing", so there are no LUIDs or lookups to make it work, and you can pass in "real names. However, this also makes it a little less exact.

`Tabcmd.trigger_extract_refresh(project, workbook_or_datasource, content_pretty_name, incremental=False, workbook_url_name=None`

The `"workbook_or_datasource"` parameter should literally be either `"workbook"` or `"datasource"`. `"content_pretty_name"` means the name that you see listed in the UI, while the "workbook_url_name" is the name you see in the URL when you ivew the content. Project in this case is just the Project name, exactly as you see it in the UI.

The example script called `"extract_refresh_pre_10_3_sample.py"` shows how this can be used.

### 3.3 Creating an Export
There are a few types of file exports, particularly a PDF which includes all pages, including info in a scrollbar, that can only be accomplished via tabcmd. If you need to generate a `"fullpdf"` export, the `Tabcmd.create_export()` method is designed to help.

For views that do not have row-level security, you can run exports as the administrator and they will work just fine. If you do need the reporst to be for a certain user, and have entered in values for the optional parameters when you created the Tabcmd object (see 3.1), then you can use the "user_to_impersonate" parameter and tabcmd will reconfigure to run as that user.

`Tabcmd.create_export(export_type, view_location, view_filter_map=None, user_to_impersonate=None, filename='tableau_workbook’)`

`"export_type"` is either "png", "pdf", "fullpdf", or "csv". "fullpdf" gets all of the data from scrolling sheets to be presented. These are much larger files, but sometimes you need them!

If you need more options for exports and more control, the Behold! Emailer https://github.com/bryantbhowell/Behold--Emailer/ implements a GUI in Windows to configure and run exports on a schedule.


## 4. tableau_repository
The tableau_repository.py file in the main section of the tableau_tools library is a wrapper to the PostgreSQL repository in a Tableau Server. It uses the psycopg2 library, which you can install via pip if you haven't already. The library is not a requirement for all of tableau_tools because you might never need to use the TableauRepository class.

### 4.1 TableauRepository Class
You initiate a `TableauRepository` object using:

`TableauRepository(tableau_server_url, repository_password, repository_username='readonly')`

`"repository_username"` can also be "tablea" (although "readonly" has higher access) or `"tblwgadmin"` if you need to make updates or have access to hidden tables. It is highly suggested you only ever sign-in with tblwgadmin for the minimal amount of commands you need to send from that privledged user, then close that connection and reconnect as readonly.

### 4.2 query() Method
`TableauRepository.query(sql, sql_parameter_list=None)`

basically wraps the execute method of psycopg2. You can put in a standard query as a string, with any parameters you would like to substitute in represented by '%s'. The optional `sql_parameter_list` takes the values that you want to be subsituted into the query, in the order they are listed. The `query()` method returns an iterable psycopg2 cursor object.

Ex.

    datasource_query = """
    SELECT id
    FROM _datasources
    WHERE name = %s
    AND site_id = %s
    AND project_id = %s
    """
    cur = self.query(datasource_query, [datasource_name, site_id, project_id])
    for row in cur:
        # Print each column of each row
        for col in row:
            print(col)


Luckily you don't have to come up with all of your own queries, TableauRepository has quite a few built-in.

### 4.3 Querying (and killing) Sessions
`TableauRepository.query_sessions(username=None)`

runs the following:

    SELECT
    sessions.session_id,
    sessions.data,
    sessions.updated_at,
    sessions.user_id,
    sessions.shared_wg_write,
    sessions.shared_vizql_write,
    system_users.name AS user_name,
    users.system_user_id
    FROM sessions
    JOIN users ON sessions.user_id = users.id
    JOIN system_users ON users.system_user_id = system_users.id
    WHERE system_users.name = %s  -- Optional
    ORDER BY sessions.updated_at DESC

`session_id` can actually be substituted for the REST API token, allowing you to kill a session:

Ex.

    t = TableauRestApiConnection27(server, username, password, site_content_url)
    t_rep = TableauRepository(server, repository_password=rep_pw)
    sessions_for_username = t_rep.query_sessions(username='some_username')
    for row in sessions_for_username:
        t.signout(row[0])

Yes, there are all sorts of other IDs besides the LUID in the repository, but you need to have gone through the work to confirm you want to do this.

## 5 Internal Library Workings
This section contains explanations of how everything is put together for anyone looking to work on the library or investigating errors.

### 5.1 tableau_tools Library Structure
tableau_tools
* tableau_rest_api
    * methods
        * _lookups
        * alert
        * datasource
        * extract
        * favorites
        * flow
        * group
        * metadata
        * project
        * rest_api_base
        * revision
        * schedule
        * site
        * subscription
        * user
        * webhooks
        * workbook
    * permissions
    * published_content (Project, Workbook, Datasource)
    * rest_xml_request
    * rest_json_request
    * sort
    * tableau_rest_api_server_connection
    * url_filter
* tableau_documents
    * table_relations
    * tableau_columns
    * tableau_connection
    * tableau_datasource 
    * tableau_document
    * tableau_file
    * tableau_parameters
    * tableau_workbook
    * hyper_file_generator   (legacy)
* tableau_rest_api_connection
* tableau_server_rest
* logger
* logging_methods
* tableau_exceptions
* tableau_repository
* tabcmd
* tableau_http
* tableau_emailer (legacy, unsupported)