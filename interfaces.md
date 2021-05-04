# Data Science - Best Practices &middot; [![License](https://img.shields.io/badge/license-CC%20BY%204.0-blue)](./LICENSE.txt)

## Table of Content

- [Chapter 1 - Introduction](./readme.md#chapter-1---introduction)
- [Chapter 2 - Project Team (Design)](./project_team.md#chapter-2---project-team)
- [Chapter 3 - Architecture (Deploy)](./architecture.md#chapter-3---architecture)
- [Chapter 4 - Source Code (Engineer)](./source_code.md#chapter-4---source-code)
- [Chapter 5 - Documentation (Engineer)](./documentation.md#chapter-5---documentation)
- [Chapter 6 - Versioning (Engineer)](./versioning.md#chapter-6---versioning)
- [Chapter 7 - Data Management (Engineer)](./data_management.md#chapter-7---data-management)
- [Chapter 8 - Dependency Management (Engineer)](./dependency_management.md#chapter-8---dependency-management)
- [Chapter 9 - Configuration Management (Engineer)](./configuration_management.md#chapter-9---configuration-management)
- [Chapter 10 - Testing (Engineer)](./testing.md#chapter-10---testing)
- [Chapter 11 - Quality Measurements (Monitor)](./quality_measurements.md#chapter-11---quality-measurements)
- [Chapter 12 - Model Training (Engineer)](./model_training.md#chapter-12---model-training)
- [Chapter 13 - Distribution (Deploy)](./distribution.md#chapter-13---distribution)
- [Chapter 14 - Cloud-Deployment (Deploy)](./cloud_deployment.md#chapter-14---cloud-deployment)
- [Chapter 15 - Edge Deployment (Deploy)](./edge_deployment.md#chapter-15---edge-deployment)
- [Chapter 16 - Monitoring (Monitor)](./monitoring.md#chapter-16---monitoring)
- [Chapter 17 - Automation (Scalability)](./automation.md#chapter-17---automation)
- [Chapter 18 - Scaling (Scalability)](./scaling.md#chapter-18---scaling)
- [Chapter 19 - Sizing (Scalability)](./sizing.md#chapter-19---sizing)
- [Chapter 20 - Security (Engineer)](./security.md#chapter-20---security)
- [License & Contributing](./license.md)

## Interfaces

Web Services provide web-based interfaces that can be accessed using the HTTP protocol for accessing data or functions. Interfaces can be build with styles like REST.
Representational state transfer (REST) is a software architectural style that defines a set of constraints to be used for creating Web services. REST is not linked to any particular platform or technology.

### Restful API Design

#### Learn the basics of HTTP applied to REST

- HTTP methods/verbs: GET, POST, PUT, PATCH and DELETE
- REST is resource-oriented, resources are represented by an URI: /digitization/.
- An endpoint is the combination of a method and an URI, e.g. GET: /documents/.
- An endpoint can be interpreted as an action on a resource. For example, POST: /documents/ may mean "Create a new document".
- At a high-level, methods map to CRUD operations: 
  
|   Method      | Description   |
| ------------- | ------------- |
|   GET 	      |  Used to retrieve a representation of a resource.|
|   POST 	      |  Used to create new new resources and sub-resources.|
|   PUT 	      |  Used to update existing resources.|
|   PATCH 	    |  Used to update existing resources.|
|   DELETE 	    |  Used to delete existing resources.|

- A response's status is specified by its status code: 1xx for information, 2xx for success, 3xx for redirection, 4xx for client errors and 5xx for server errors.

#### Describe URIs by using nouns, not verbs

Resources are the fundamental elements of the web platform. Every resource has a unique identifier on the web platform, the universal resource identifier (URI).

HTTP methods should be sufficient to describe the action being performed on the resource.

When providing an endpoint to generate and retrieve a page image for a document, you might be tempted to create this endpoint:

    GET: /articles/generatePageImage/   

The GET method is semantically sufficient to say that we're retrieving ("GETting") a page. Just use:

    GET: /articles/page/

Similarly, for an endpoint that creates a new document:

*Don't Do*

    POST: /articles/createNewDocument/

*Do*

    POST: /documents/

#### Representation

It is about determining a way to showcase these resources to clients. REST supports the use of all formats without any restrictions.
Based on the client’s and server’s ability to work with the formats, you can go with JSON, XML, or any other format.

    For example, in RESTful Web Services , a document resource is represented using the following JSON format:
    { 
       "id":1, 
       "name":"amazon.pdf", 
       "doctype":"Invoice" 
    }

    The same resource can be represented in XML format as follows:
    <document> 
       <id>1</id> 
       <name>amazon.pdf</name>
       <doctype>Invoice</doctype> 
    </document> 


#### Use plural resource nouns

It may be hard to decide whether or not you should use the plural or singular form for resource nouns.

Should you use /document/:id/ (singular) or /documents/:id/ (plural)?

It is recommended to use the plural form because it fits all types of endpoints. When performing a GET on /document/ the answer could be a only a single one or all documents. 
To prevent this kind of ambiguity, the plural should be used everywhere.

    GET: /documents/:id/
    POST: /documents/

#### Return error details in the response body

Always return a representation, that describes the error in detail. This includes the response status code, response headers, and a body containing the description of the error. If you are logging errors somewhere, include an identifier of the same.

    HTTP 1.1 defines two classes of error codes:
    1. 4xx: Client Error
    4xx codes are used when there is an error / exception at the client’s side. 
    This happens because of requesting an unavailable resource or placing a bad request.
    2. 5xx: Server Error
    5xx codes are used when there is an error / exception at the server-side while interpreting the request

    HTTP/1.1 400 Bad Request
    Content-Type: application/json

    {
       "error": "Invalid payload.",
        "detail": {
          "user_id": "This field is required."
         }
    }

#### Pay attention to status codes

This one is super important. The worst thing your API could do is return an error response with a 200 OK status code. Instead, return a meaningful status code that correctly describes the type of error.

Wrong APIs could return false responses, that are misleading like this one:

    HTTP/1.1 200 OK
    Content-Type: application/json

    {
        "status": "failure",
        "data": {
            "error": "Expected at least two items in list."
        }
    }

This is extremely un-RESTful. Make use of the status code and only use the response body to provide error details.

    HTTP/1.1 400 Bad Request
    Content-Type: application/json

    {
        "error": "Expected at least two items in list."
    }

#### Use status codes consistently

Status codes should be used consistently. For example, if a POST endpoint returns a 201 Created somewhere, the same status code for every POST endpoint should be used. 

#### Don't nest resources

REST APIs deal with resources, and retrieving a list or a single instance of a resource is straight forward. 

But what happens when dealing with related resources? Let's view the retrieval of the list of documents for a particular user. There are basically two options.

The first option is to nest the documents resource under the users resource, e.g.:

    GET: /users/2/documents/

This solution is not clear in what kind of resource its requesting, users or documents? There is a better way to do it, that is flat and not nested.

Use the querystring to filter the documents resource directly:

    GET: /documents/?user_id=2

This clearly means: "get all documents for author #2"
