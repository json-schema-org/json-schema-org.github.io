---
layout: page
title: JSON Schema
permalink: /
---

**NEW DRAFT PUBLISHED!**
{: style="color:red; font-size: 200%; text-align: center;"}

The current version is [2019-09](specification.html)!
{: style="color:gray; font-size: 150%; text-align: center;"}

**JSON Schema** is a vocabulary that allows you to **annotate** and **validate** JSON documents.


## Advantages

<div class="block" style="float:left;width:50%;" markdown="1">

### JSON Schema

* Describes your existing data format(s).
* Provides clear human- and machine- readable documentation.
* Validates data which is useful for:
    * Automated testing.
    * Ensuring quality of client submitted data.

</div>

<div class="block" style="float:right;width:50%;" markdown="1">

### JSON Hyper-Schema

* Make any JSON format a hypermedia format with no constraints on document structure
* Allows use of [URI Templates](https://tools.ietf.org/html/rfc6570) with instance data
* Describe client data for use with links using JSON Schema.
* Recognizes collections and collection items.

</div>

## Project Status

16 September 2019: Draft 2019-09 (formerly known as draft-08) has been published!

The IETF document IDs are of the form `draft-handrews-*-02`.  We are now using dates
for meta-schemas, which are what implementations should use to determine behavior,
so we will usually refer to `2019-09` (without the word "draft") on this web site.

See the [Specification page](specification.html) for details about naming and numbering.

### The Path to Standardization

The JSON Schema project intends to shepherd all four draft series to RFC status.  Currently, we are continuing to improve our self-published Internet-Drafts. The next step will be to get the drafts adopted by an IETF Working Group.  We are actively investigating how to accomplish this.

If you have experience with such things and would like to help, please contact us!

In the meantime, publication of Internet-Draft documents can be tracked through the IETF:
* [JSON Schema (core)](https://datatracker.ietf.org/doc/draft-handrews-json-schema/)
* [JSON Schema Validation](https://datatracker.ietf.org/doc/draft-handrews-json-schema-validation/)
* [JSON Hyper-Schema](https://datatracker.ietf.org/doc/draft-handrews-json-schema-hyperschema/)
* [Relative JSON Pointers](https://datatracker.ietf.org/doc/draft-handrews-relative-json-pointer/)

Internet-Drafts expire after six months, so our goal is to publish often enough to always have a set of unexpired drafts available.  There may be brief gaps as we wrap up each draft and finalize the text.

The intention, particularly for vocabularies such as validation which have been widely implemented, is to remain as compatible as possible from draft to draft.  However, these are still drafts, and given a clear enough need validated with the user community, major changes can occur.

## Quickstart

The JSON document being validated or described we call the *instance*, and the document containing the description is called the *schema*.

The most basic schema is a blank JSON object, which constrains nothing, allows anything, and describes nothing:

```json
{}
```

You can apply constraints on an instance by adding validation keywords to the schema. For example, the "type" keyword can be used to restrict an instance to an object, array, string, number, boolean, or null:

```json
{ "type": "string" }
```

JSON Schema is hypermedia ready, and ideal for annotating your existing JSON-based HTTP API. JSON Schema documents are identified by URIs, which can be used in HTTP Link headers, and inside JSON Schema documents to allow recursive definitions.

## More

Interested? Check out:

* [Understanding JSON Schema](/understanding-json-schema/)
* The [specification](./specification.md)
* [Learning resources](./learn/index.md)
* the growing list of [JSON (Hyper-)Schema software](./implementations.md)

We encourage updating to the latest specification, as described by the draft-07 meta-schemas.

Questions? Feeling helpful? Get involved on:

* [GitHub](http://github.com/json-schema-org/json-schema-spec)
* [Google Groups](https://groups.google.com/forum/#!forum/json-schema)
* [Slack](https://join.slack.com/t/json-schema/shared_invite/enQtNjc5NTk0MzkzODg5LTVlZGIxNmVhMGY2MWFlYTdiNDQ5NWFiZGUwOThhNmYxZDE0YzA5YjRiOTA5MGY4ZTZlZGZhZDFmYTY4NWM2N2Y)
