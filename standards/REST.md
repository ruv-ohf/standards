# REST APIs at Gilt

## Introduction

REST is an architecture style for developing web services. It was
first described by
[Fielding](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)
in 2000. Since then it has become a standard approach to developing
web based APIs.

We have adopted REST over HTTP/JSON as the standard method for
communication between services; that is, all services at gilt should
expose a REST API.

REST itself is a large standard. This document highlights the specific
components of REST that are requirements for Gilt services, and leaves
as suggestions other common / best practices we encourage teams to
consider.

## Motivation

The two largest drivers of REST at Gilt include:

  1. With micro service architectures, it is common for individuals
     and teams to interact with dozens or even hundreds of discrete
     services on a monthly basis. By adopting REST as a standard, we
     can make it easier to interact with services produced by
     different teams; consistency is even more important as the number
     of services increases.

  2. As developers, we enjoy consuming external services - whether
     open source or commercial - when those services have incredibly
     high quality REST APIs. Thus we are choosing to built any
     internal services in exactly the spirit of open source.

## apidoc

Gilt sponsors the development of http://www.apidoc.me - an open
source, free SAAS solution to help developers develop REST
APIs. apidoc is NOT a standard at Gilt; rather just a tool that some
teams are finding helpful in building REST APIs.

We encourage developers to explore the APIs available in apidoc - the
apidoc schema is designed to make it natural for an API to make
decisions consistent with the REST standard.


## The Principles - Five steps to RESTful APIs

[Tilkov](http://www.infoq.com/articles/rest-introduction) describes
REST as a set of principles that define how Web standards, such as
HTTP and URIs, are supposed to be used. It promises, as Tilkov
describes, a means of exploiting the Web’s architecture. The main
principles are as follows:

  - A resource is an identifiable "thing". 
  - Use standard HTTP methods. 
  - Link things together. 
  - Resources have multiple representations.
  - Stateless. 

### A Resource is an Identifiable "Thing"

In REST a resource is an identifiable object within the system. It is
"a thing", "a noun", it is a key abstraction worth identifying that is
uniquely addressable. For example:

	http://example.com/products/123

provides an unique, addressable end point for a resource called
products with an identifier of 123. The following URI may look
different, in that it does not address a single instance of a
resource, but in fact it is the unique end point for a collection of
products.

	http://example.com/products

In designing a particular resource that has links to other resources,
we prefer to think in terms of "dots" to help us identify the
naming. As a simple example, if a user belongs to an organization, we
may explore two options for representing that user:


#### Option 1 (preferred):

    {
      "id": "72297659-a290-4f1d-9e4c-2fe77ece7f98",
      "organization": {
        "id": "b3b06142-dd49-46b3-b663-1b879a18b62a"
      }
    }

#### Option 2:

    {
      "id": "72297659-a290-4f1d-9e4c-2fe77ece7f98",
      "organization_id": "b3b06142-dd49-46b3-b663-1b879a18b62a"
    }



At Gilt, we prefer option 1 as it leads to a more natural style of
code and also enables simple future expansion of the organization's
API.


#### Gilt Standards

  1. A resource MUST represent an identifiable object within the
     system.

  2. All identifiable resources MUST have an unique, addressable
     identifier.

#### General Suggestions

  1. Use "user.organization.id" instead of "user.organization_id"

  1. Use nouns for resources.

  2. Resource names SHOULD be pluralised.

  3. Use simple nouns - e.g. products - vs. compount nouns -
     e.g. "external_products". If you are unable to name a simple noun
     for a critical resource, it is likely the design of the API is
     not quite right. Iterate on the design and usually a simple noun
     will emerge.

### Use standard HTTP methods

While resources are the nouns, [HTTP
methods](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9) -
GET, POST, DELETE, PATCH and PUT - are the verbs that allow resources
to be manipulated within a service. In this case getting, creating,
updating, and deleting (CRUD) of resources. The HTTP GET method gets a
single resource or collection of resources. A POST method is used to
create a resource where the ID of the resource is not known
beforehand. A PUT method can be used to create a resource where of the
ID of the resource is known. The PUT method can also be used to do a
full update of a resource by replacing the resource in its
entirety. PATCH is used to do partial updates of resources while the
DELETE method deletes an identifiable resource. The [HTTP
RFC](http://www.w3.org/Protocols/rfc2616/rfc2616.html) provides a
detailed explanation on correct usage of HTTP methods.

HTTP methods can be described as being safe and/or idempotent. A safe
method is a method that does not modify a resource no matter how many
times it is called. A GET method would be classified as a safe
method. An idempotent HTTP method can be called multiple times without
the outcome changing. A PUT or a DELETE would be an example of an
idempotent operation while a POST would not be as subsequent calls
would generated new resources. When designing RESTful APIs it is
desirable to use safe and idempotent HTTP methods. A more detailed
description can be found at [4].

Successful HTTP requests return a status response in the 2xx
range. For example a successful GET will return 200 Ok, while a POST
will return a 201 Created with the created resource in the
body. Unsuccessful requests are either in the 4xx or 5xx range of
codes. The 4xx typically indicates that there is something wrong with
the request that the server is unable to process. In contrast the 5xx
shows that an error has occured on the server side. See [full
definition of the status
codes](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10).

A general rule when it comes to developing RESTful APIs is to use
standard HTTP. No need to reinvent the wheel.

#### Gilt Standards

  1. A successful GET request MUST return 200 Ok status and a body
     representing the resource or collection of resources.

  2. Successful PUT, POST, PATCH requests MUST return a 200 OK or 201
     Created status.

  3. A successful DELETE request MUST return 204 No Content status
     with no body present.

#### General Suggestions

  1. GET Requests without a resource identifier should implement
     search and return a paginated list of resources. We suggest using
     limit and offset for pagination. 

       GET /products?limit=25&offset=0

       should return an array of products, each of which is a JSON
       object

  1. PUT / POST / PATCH should return a body representing the resource
     that was created or updated, including any changes made on that
     request.

  2. A PUT request that creates a resource should return 201 Created;
     if the resource already existed, a PUT request should return 200
     Ok.

  3. Delete requests should be made idempotent; that is the end state
     of the DELETE request is that the resource no longer exists. At
     Gilt, we do not distinguish between whether or not the resource
     existed; This simplification means we avoid race conditions
     (e.g. double clicking would result in 2 successful requests).

  4. [Idempotent or safe HTTP
     methods](http://restcookbook.com/HTTP%20Methods/idempotency/)
     are preferred.

  5. A successful GET of a resource should include [caching
     headers](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9).


### Resources have multiple representations

The heterogeneous nature of systems means clients often require
responses in different formats or representations. The classic example
is providing a customer’s contact details in either VCard or JSON
format. HTTP provides a mechanism called [content
negotiation](http://www.w3.org/Protocols/rfc2616/rfc2616-sec12.html#sec12)
which allows the handling of data to be separated from the operation
performed on it. This means various clients can operate on the same
resources using different representation of the resource.

#### Gilt Standards

  1. Request with an [Accept
     header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1)
     MUST return content on the type requested. If the request cannot
     be satisfied a HTTP status of 415 Unsupported Media Type MUST be
     returned.

  2. Requests with a body MUST include a [Content-Type
     header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.17)
     that reflects the type of data in the body.

  3. When the content type is JSON a resource MUST be represented as a
     JSON object.

  4. When the content type is JSON a collection of resources MUST be
     represented as a JSON array of JSON objects.


### Link things together

The next principle of REST is to [link
resources](https://i.imgur.com/hsRbKmO.jpg) together. This has the
advantage of allowing related resources to be discovered by
clients. Links are URIs that point directly to the resource. The
approach ensures that clients don’t need to fumble with generating
URIs or have the need for prior knowledge of how to generate the
URIs. This means links included within a resource don’t need to be
from the original service and importantly, the location the links
point to can be changed in later versions without affecting the
client.

#### Gilt Standards

We have no Gilt Standards in this area. Currently, common web
frameworks do not yet provide simple ways to inject appropriate
headers to incoming REST calls; we choose to avoid a gilt standard
until we can also demonstrate beautiful code implementing the
standard.


#### General Suggestions

  1. A resource can include a self link to access the resource in
     subsequent requests.

  2. A successful POST request can include a [Location
     header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.30)
     in the response pointing to the location of the created resource.

  3. A resource can include links to other relevant resources.

  4. A link can be represented as a [Link
     header](http://tools.ietf.org/html/rfc5988) or when the content
     type is JSON it MAY be included in a _links element as part of an
     array of links.

  5. If adding pagination headers, use the standard [Link
     headers](http://tools.ietf.org/html/rfc5988) for next/prev.

       Link: <https://api.example.com/products?offset=0>; rel="prev", <https://api.example.com/products?offset=25>; rel="next"

### Stateless

REST mandates that state should either be kept on the client or turned
into a resource. The result is that the server is
[stateless](http://en.wikipedia.org/wiki/Representational_state_transfer#Stateless)
in terms of communication with the client. In practice this means the
server never relies on information from a previous request to process
the current request. This implies that the client must send all
relevant information in the request for it to be processed by the
server. The server does not retain communication state with the
client. This has the benefit of allowing any server in a service
handle a client’s request without the need, for example, of
maintaining session state between servers. The service tends, as a
result, to scale and handle failure more efficiently.

#### Gilt Standards

  1. A request MUST contain all the necessary information to process
     the request.

  2. A server MUST not retain communication state beyond a single
     request [2].


### Summary 

In this document we have described the main principles behind REST and
have outlined a number of guidelines to help develop RESTful APIs. The
guidelines are intended to provide a framework in which to develop
RESTful APIs at Gilt. They are in no means exhaustive and undoubtedly
there will be exceptions and as a result the guidelines should be used
as such - guidelines.


## Appendix: Examples

Below are examples of REST api requests that follow the above guidelines. 

### GET Request - Single Resource 

    GET /products/123 HTTP/1.1
    Host: api.example.com
    Accept: application/json

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1234
    Link: <http://api.example.com/tags/123>; rel="tag"

    {
	"name": "A big bag",
	"_links": {
            "self": {"href": "http://api.example.com/products/123"} 
        }
    }

### GET Request - Single Resource With Cache-Control

    GET /products/123 HTTP/1.1
    Host: api.example.com
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1234
    Cache-Control: public,max-age=300,no-transform
    ETag: "bbeaadb7e1785119a7aaafdd504c5fff"
    Date: Mon, 29 Apr 2013 16:38:15 GMT
    Last-Modified: Sat, 27 Apr 2013 00:44:54 GMT
    Expires: Mon, 29 Apr 2013 21:44:55 GMT
    Vary: Accept,Accept-Encoding
    
    {
    	"name": "A big bag",
    	"_links": {
            "self": {"href": "http://api.example.com/products/123"} 
        }
    }

### GET Request - Collection

    GET /products?offset=20&limit=10 HTTP/1.1
    Host: api.example.com
    Accept: application/json
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1234
    Link: <http://api.example.com/products?offset=4&limit=10>; rel="prev"
    Link: <http://api.example.com/products?offset=6&limit=10>; rel="next"
    
    [{
    	"name": "Chocolate Biscuit",
        "description": "A big bag of biscuits"
    	"_links": {
            "self": {"href": "http://api.example.com/products/123"}, 
            "tag": {"href": "http://api.example.com/tag/chocolate"}, 
        }
     },
     {
    	"name": "Plain Biscuit",
        "description": "A small bag of biscuits"
    	"_links": {
            "self": {"href": "http://api.example.com/products/456"},
            "tag": {"href": "http://api.example.com/tag/plain"},  
        }
     }
    ]

### POST Request

    POST /products HTTP/1.1
    Host: api.example.com
    Content-Type: application/json
    Accept: application/json
    Content-Length: 1234
    
    {
    	"name": "Chocolate Biscuit",
        "description": "A big bag of biscuits"
    }
    
    HTTP/1.1 201 Created
    Content-Type: application/json
    Content-Length: 1234
    Location: http://api.example.com/products/123
    
    {
    	"name": "Chocolate Biscuit",
        "description": "A big bag of biscuits"
    	"_links": {
            "self": {"href": "http://api.example.com/products/123"} 
        }
    }


### PUT Request

    PUT /products/123 HTTP/1.1
    Host: api.example.com
    Content-Type: application/json
    Accept: application/json
    Content-Length: 1234
    
    {
    	"name": "Chocolate Biscuit",
       "description": "A big bag of biscuits"
    }
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1234
    
    {
    	"name": "Chocolate Biscuit",
       "description": "A big bag of biscuits"
    	"_links": {
            "self": {"href": "http://api.example.com/products/123"} 
        }
    }
    
### PATCH Request

    PATCH /products/123 HTTP/1.1
    Host: api.example.com
    Content-Type: application/json
    Accept: application/json
    Content-Length: 1234
    
    {
    	"name": "Plain Biscuit"
    }
    
    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 1234
    
    {
    	"name": "Plain Biscuit",
    }
    
### DELETE Request 

    DELETE /products/123 HTTP/1.1
    Host: api.example.com
    Accept: application/json
    
    HTTP/1.1 204 No Content
    GET Request - Single Resource - Not Found
    GET /products/123 HTTP/1.1
    Host: api.example.com
    Accept: application/json
    
    HTTP/1.1 404 Not Found

### POST Request - Bad Request. 

    POST /products HTTP/1.1
    Host: api.example.com
    Content-Type: application/json
    Accept: application/json
    Content-Length: 1234
    
    {
    	"name": "Plain Biscuit"
    }
    
    HTTP/1.1 400 Bad Request 
    Content-Type: application/json
    Content-Length: 1234
    
    [{
        "code": "1000",
        "message": "Missing field description"
    }]

    