Standards & Guidelines For Defining Events
==========================================

### Inter-departmental communication via events


For asynchronous inter-departmental communication
[Avro](http://avro.apache.org/docs/current/) has been chosen as the de facto
standard for event definition and transport.


### Avro IDL

[Avro IDL](http://avro.apache.org/docs/current/idl.html) is the standard tool
we choose for defining Avro schemas at Gilt.

The primary benefit of IDL is how the focus of the documents is the schema
itself which we think will lead to higher quality reviews over time.


### Naming

#### Repository

Due to the importance of getting the inter-departmental communication API right
we encourage creating a separate repository with high visibility and tight
governance for defining your Avro schemas.

By convention the repository should be named: `events-<protocol>` where
`<protocol>` should be repaced by the name of the Avro protocol you are
defining.

View all existing events repositories at
[GitHub](https://github.com/gilt?query=events)


#### Avro

We include major version numbers in the namespace. Note that all changes to
events sharing a major version number must by definition in Avro be backwards
compatible.

Examples:

If you create a repo named 'events-orders' we would expect to see an Avro IDL
file named ```orders.avdl``` in it's root directory containing:

    @namespace("com.gilt.orders.v1")
    protocol Orders {
      ...
    }

If you create a repo named 'events-mobile-tapstream' we would expect to see an
Avro IDL file named ```mobile-tapstream.avdl``` in it's root directory
containing:

    @namespace("com.gilt.mobile.tapstream.v1")
    protocol MobileTapstream {
      ...
    }


### Capitalization Rules

* Type names (i.e. records, enums, etc) should be written in [upper camel
  case](http://c2.com/cgi/wiki?UpperCamelCase)

* Field names should be written in [lower camel
  case](http://c2.com/cgi/wiki?LowerCamelCase)

Some clarifications:

* Acronyms should also be camel cased. E.g. when ```URL``` appears in a type or
  field name it should be be written as ```Url``` except if it appears at the
  beginning of a field name in which case it should be written as ```url```.

* Oddly capitalized words such as ```iOS``` should be written as ```Ios```
  except if it appears at the begining of a field name in which case it should
  be written as ```ios```.


### Types

In general, for non primitive types, we are following the guidelines of the
types documented at [apidoc](http://www.apidoc.me/doc/types).

Specific schemas for common types will be listed here as recommendations for
teams to adopt.


### GFC Avro

GFC (or Gilt Foundation Classes) is a standard namespace for key libraries that
we open source at Gilt. We have shared avro schemas in the [events-gfc-avro
repository](https://github.com/gilt/events-gfc-avro). You can pull in the
latest version of these schemas with the following command:

    curl -s -o gfc.avdl \
        "https://raw.githubusercontent.com/gilt/events-gfc-avro/master/events.avdl"


### Dates and Timestamps

The preferred format for dates and timestamps is ISO-8601 in string type. The
name should be suffixed with Iso8601 for clarity. E.g.:

```
string createdAtIso8601;
```

For a more optimized event model [epoch
time](http://en.wikipedia.org/wiki/Unix_time) in milliseconds is preferred. Use
a type long. E.g.:

```
long createdAt;
```

