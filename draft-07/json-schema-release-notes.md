---
title: JSON Schema Draft-07 Release Notes
layout: page
---

For the Core and Validation specifications, draft-07 is a relatively
minor update.  In terms of validation keywords and outcomes, it is fully
backwards-compatible with draft-06.

Some differences exist with keywords moved over from Hyper-Schema, and with
how instances and schemas are recommended to be linked together.  Finally,
the process of collecting annotation keyword values has been defined
more clearly than before.

* TOC
{:toc}

### Keywords

* No keywords changed behavior
* No keywords were removed
* Some keywords were moved from Hyper-Schema, and two of those were renamed

keyword | change | notes
---- | ---- | ----
[`"$comment"`](json-schema-core.html#rfc.section.9) | added to Core | Intended for notes to schema maintainers, as opposed to [`"description"`](json-schema-validation.html#rfc.section.10.1) which is suitable for display to end users
[`"if"`, `"then"`, `"else"`](json-schema-validation.html#rfc.section.6.6) | added to Validation | explicit conditional schema evaluation 
[`"readOnly"`](json-schema-validation.html#rfc.section.10.3) | moved from Hyper-Schema to Validation | not limited to hypermedia environments
[`"writeOnly"`](json-schema-validation.html#rfc.section.10.3) | added to Validation | general write-only fields, including but not limited to passwords
[`"contentMediaType"`](json-schema-validation.html#rfc.section.8) | moved from Hyper-Schema to Validation | formerly [`"media": {"type": "..."}`](../draft-06/json-schema-hypermedia.html#rfc.section.5.3)
[`"contentEncoding"`](json-schema-validation.html#rfc.section.8)  | moved from Hyper-Schema to Validation | formerly [`"media": {"binaryEncoding": "..."}`](../draft-06/json-schema-hypermedia.html#rfc.section.5.3)

Note that the `"content*"` keywords do not _require_ validation.

### Formats

Numerous formats were added, clarified, or restored from older drafts.

format | change | notes
---- | ---- | ----
[`"iri"`](json-schema-validation.html#rfc.section.7.3.5) | added | I18N equivalent of `"uri"`
[`"iri-reference"`](json-schema-validation.html#rfc.section.7.3.5) | added | I18N equivalent of `"uri-reference"`
[`"uri-template"`](json-schema-validation.html#rfc.section.7.3.6) | noted IRI support | There is no separate IRI Template standard
[`"idn-email"`](json-schema-validation.html#rfc.section.7.3.2) | added | I18N equivalent of `"email"`
[`"idn-hostname"`](json-schema-validation.html#rfc.section.7.3.3) | added | I18N equivalent of `"hostname"`
[`"json-pointer"`](json-schema-validation.html#rfc.section.7.3.7) | clarified | Use for string form only, not URI fragment
[`"relative-json-pointer"`](json-schema-validation.html#rfc.section.7.3.7) | added | Revived [Relative JSON Pointer](relative-json-pointer.html) draft
[`"regex"`](json-schema-validation.html#rfc.section.7.3.8) | restored from draft-03 | ECMA 262 regular expressions
[`"date"`](json-schema-validation.html#rfc.section.7.3.1) | restored from draft-03 | RFC 3339 "full-date"
[`"time"`](json-schema-validation.html#rfc.section.7.3.1) | restored from draft-03 | RFC 3339 "full-time"

All other RFC 3339 date, time, and duration names are reserved for future
consideration.  If added as extension formats, they SHOULD be implemented
in a way that is compatible with their use in the RFC to ensure future
compatibility.

### Classification of Keywords

While it does not have a direct impact on the validation process, this set
of drafts classifies keywords by their behavior.  The names of these
classifications are used throughout the documents, so they are useful
to know:

* [Applicability](json-schema-validation.html#rfc.section.3.1)
* [Assertions](json-schema-validation.html#rfc.section.3.2)
* [Annotations](json-schema-validation.html#rfc.section.3.3)

Note that `definitions` does not fit any of these categories, nor do the
dollar-prefixed Core keywords.

### Collecting Annotation Values

[Annotation keywords](json-schema-validation.html#rfc.section.10) (formerly
called [metadata keywords](../draft-06/json-schema-validation.html#rfc.section.7)
now provide guidance on
[how to collect multiple values](json-schema-validation.html#rfc.section.3.3)
that apply to the same location in the instance.  By default, all values
that are not found within
[negated schemas](json-schema-validation.html#rfc.section.3.3.1) are collected
as an unordered set.  The following exceptions are documented under the
approprite keywords:

* `readOnly` and `writeOnly` should be logically ORed
* `examples` should be flattened into a single collected array

### JSON Schema in Hypermedia Environments

These changes are not relevant to many Validation use cases, and are more
of interest to Hyper-Schema users.  However, even without Hyper-Schema,
if you are accessing instance documents over HTTP or through other hypermedia
environments, you may find this section useful.

#### Linking Instances and Schemas

After discussions with the author of the "profile" specification, we concluded
that its use for JSON Schema was not correct.  The new guidance for
[what relations to use](json-schema-core.html#rfc.section.11.1)
to link instances to schemas is:

link relation | change | notes
---- | ---- | ----
"describedBy" | no change | network-accessible URL
"profile" | removed; use "schema" | opaque identifying URI
"schema" | added | opaque identifying URI

"schema", like "profile" in past drafts, can also be used as a
[media type parameter](json-schema-core.html#rfc.section.11.2).

#### Instance Media Type

Changes in the section are more relevant to JSON Hyper-Schema than to
Validation, but as they are part of the core specification, they are
explained here.

Media types determine their own parameters, as well as their own
URI fragment syntax(es).  `application/json` does not allow any parameters
or URI fragments.

[`application/schema-instance+json`](json-schema-core.html#rfc.section.4.2.2)
 is an optional media type for use with instances that supports "schema"
as a media type parameter, and allows for URI fragments using the JSON Pointer
syntax.

Supporting "schema" as a media type parameter allows for
schema-based content negotiation in hypermedia environments.

Supporting JSON Pointer URI fragments is primarily useful with JSON Hyper-Schema,
as many URIs involved in hyper-schema usage originate from or point to
locations within an instance document rather than the entire document.
