---
layout: page
title: JSON Schema
permalink: /
---

**JSON Schema** is a vocabulary that allows you to **annotate** and **validate** JSON documents.

Advantages
----------

<div class="block" style="float:left;width:50%;" markdown="1">

### JSON Schema

-   describes your existing data format
-   clear, human- and machine-readable documentation
-   complete structural validation, useful for
    -   automated testing
    -   validating client-submitted data
</div>

<div class="block" style="float:right;width:50%;" markdown="1">

### JSON Hyper-Schema

-   describes your existing API - no new structures required
-   links (including [URI Templates](http://tools.ietf.org/html/rfc6570) for target URIs)
-   forms - specify a JSON Schema for the desired data

</div>

<div style="clear:both"></div>

Quickstart
----------

The JSON document being validated or described we call the *instance*, and the document containing the description is called the *schema*.

The most basic schema is a blank JSON object, which constrains nothing, allows anything, and describes nothing:

```json
{}
```

You can apply constraints on an instance by adding validation keywords to the schema. For instance, the "type" keyword can be used to restrict an instance to an object, array, string, number, boolean, or null:

```json
{"type": "string"}
```

JSON Schema is hypermedia ready, and ideal for annotating your existing JSON-based HTTP API. JSON Schema documents are identified by URIs, which can be used in HTTP Link headers, and inside JSON Schema documents to allow recursive definitions.

More
----

Interested? Check out:

-   the [specification](documentation.html)
-   some [examples](examples.html)
-   this [excellent guide](http://spacetelescope.github.io/understanding-json-schema/) for schema authors, from the [Space Telescope Science Institute](http://www.stsci.edu/)
-   the growing list of [JSON (Hyper-)Schema software](implementations.html)

Questions? Feeling helpful? Get involved on:

-   the [GitHub repo](http://github.com/json-schema-org/json-schema-spec)
-   the [Google Group](https://groups.google.com/forum/#!forum/json-schema)
-   the [IRC channel](irc://chat.freenode.net/json-schema) ([Freenode Webchat](https://webchat.freenode.net/?channels=json-schema))


