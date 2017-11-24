---
layout: page
title: JSON Schema
permalink: /
---

***Draft-07 has been [published](documentation.md)!***
{: style="color:gray; font-size: 150%; text-align: center;"}

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

-   make any JSON format a hypermedia format - no constraints on document structure
-   use [URI Templates](https://tools.ietf.org/html/rfc6570) with instance data
-   describe client data for use with links using JSON Schema
-   recognize collections and collection items

</div>

Project Status
--------------

The JSON Schema project intends to shepherd the Core, Validation, and Hyper-Schema specifications
to RFC status.  Currently, we are continuing to improve our self-published Internet-Drafts.
The next step will be to get the drafts adopted by an IETF Working Group.

In the meantime, publication of Internet-Draft documents can be tracked through the IETF:
- [JSON Schema (core)](https://datatracker.ietf.org/doc/draft-handrews-json-schema/)
- [JSON Schema Validation](https://datatracker.ietf.org/doc/draft-handrews-json-schema-validation/)
- [JSON Hyper-Schema](https://datatracker.ietf.org/doc/draft-handrews-json-schema-hyperschema/)
- [Relative JSON Pointers](https://datatracker.ietf.org/doc/draft-handrews-relative-json-pointer/)

Internet-Drafts expire after six months, so our goal is to publish often enough to always have
a set of unexpired drafts available.  There may be brief gaps as we wrap up each draft and finalize
the text.

The intention, particularly for vocabularies such as validation which have been widely
implemented, is to remain as compatible as possible from draft to draft.  However, these are still
drafts, and given a clear enough need validated with the user community, major changes can occur.

Progress on the next set of Internet-Drafts can be tracked on GitHub.
The [draft-08](https://github.com/json-schema-org/json-schema-spec/milestone/6) milestone 
will track the evolving scope of the draft

<div style="clear:both"></div>

Quickstart
----------

The JSON document being validated or described we call the *instance*, and the document containing the description is called the *schema*.

The most basic schema is a blank JSON object, which constrains nothing, allows anything, and describes nothing:

```json
{}
```

You can apply constraints on an instance by adding validation keywords to the schema. For example, the "type" keyword can be used to restrict an instance to an object, array, string, number, boolean, or null:

```json
{"type": "string"}
```

JSON Schema is hypermedia ready, and ideal for annotating your existing JSON-based HTTP API. JSON Schema documents are identified by URIs, which can be used in HTTP Link headers, and inside JSON Schema documents to allow recursive definitions.

More
----

Interested? Check out:

-   the [specification](documentation.md)
-   some [examples](examples.md)
-   the growing list of [JSON (Hyper-)Schema software](implementations.md)

We encourage updating to the latest specification, as described by the draft-07 meta-schemas.  However, if you are still using draft-04, you may be interested in:
-   this [excellent guide](http://spacetelescope.github.io/understanding-json-schema/) for schema authors, from the [Space Telescope Science Institute](http://www.stsci.edu/)

Questions? Feeling helpful? Get involved on:

-   the [GitHub repo](http://github.com/json-schema-org/json-schema-spec)
-   the [Google Group](https://groups.google.com/forum/#!forum/json-schema)
-   the [IRC channel](irc://chat.freenode.net/json-schema) ([Freenode Webchat](https://webchat.freenode.net/?channels=json-schema))
