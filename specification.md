---
layout: page
redirect_from: "/documentation.html"
permalink: /specification.html
title: Specification
---

The current version is *2019-09*!

Specification documents
-----------------------

See also the [release notes / change log](draft/2019-09/release-notes.html).

The specification is split into three parts, Core, Validation, and Hyper-Schema, along with a related specification, Relative JSON Pointers:

|--------------------------------------------------------------|-------------------------------------------------------|
| [JSON Schema Core](draft/2019-09/json-schema-core.html)             | defines the basic foundation of JSON Schema           |
| [JSON Schema Validation](draft/2019-09/json-schema-validation.html) | defines the validation keywords of JSON Schema        |
| [JSON Hyper-Schema](draft/2019-09/json-schema-hypermedia.html)      | defines the hyper-media keywords of JSON Schema       |
| [Relative JSON Pointers](draft/2019-09/relative-json-pointer.html)  | extends the JSON Pointer syntax for relative pointers |

They are also available on the IETF main site:
* [draft-handrews-json-schema-02 (core)](https://tools.ietf.org/html/draft-handrews-json-schema-02)
* [draft-handrews-json-schema-validation-02](https://tools.ietf.org/html/draft-handrews-json-schema-validation-02)
* [draft-handrews-json-schema-hyperschema-02](https://tools.ietf.org/html/draft-handrews-json-schema-hyperschema-02)
* [draft-handrews-relative-json-pointer-02](https://tools.ietf.org/html/draft-handrews-relative-json-pointer-02)

Meta-schemas
------------

The meta-schemas are schemas against which other schemas can be validated. They are self-descriptive: the JSON Schema meta-schema validates itself, while the JSON Hyper-Schema meta-schema both validates itself and defines its own "self" link.

The latest meta-schema is **2019-09**.  For an explanation of the change to date-based identifiers, see the [Specification Links](specification-links.html) page.

_If you are accessing these JSON document links **from a web browser**, you will need to **save the file** then open it as a JSON document.  This is due to limitations with GitHub Pages._

## General-purpose meta-schemas

These serve the same function as the analogous meta-schemas from past drafts, although note that the core and validation schema is no longer self-contained.

|--------------------------------------------------------------|------------------------------------------------------------|
| [Core/Validation meta-schema](draft/2019-09/schema) | Used for schemas written for pure validation.              |
| [Hyper meta-schema](draft/2019-09/hyper-schema)     | Used for schemas written for validation and hyper-linking. |

## Single-vocabulary meta-schemas

These are relevant primarily to people who want to write their own meta-schemas that build on specific parts of the existing specification.

- [Core Vocabulary meta-schema](draft/2019-09/meta/core)
- [Applicator Vocabulary meta-schema](draft/2019-09/meta/applicator)
- [Validation Vocabulary meta-schema](draft/2019-09/meta/validation)
- [Format Vocabulary meta-schema](draft/2019-09/meta/format)
- [Content Vocabulary meta-schema](draft/2019-09/meta/content)
- [Meta-Data Vocabulary meta-schema](draft/2019-09/meta/meta-data)
- [Hyper-Schema Vocabulary meta-schema](draft/2019-09/meta/hyper-schema)

## Output schemas and examples
- [JSON Schema recommended output schema](draft/2019-09/output/schema)
- [JSON Hyper-Schema recommended output schema](draft/2019-09/output/hyper-schema)
- [JSON Schema verbose output example](draft/2019-09/output/verbose-example)

Migrating from older drafts
-------------

The release notes discuss the changes impacting users and implementers:

- JSON Schema Core and Validation
    - [Draft-07 to 2019-09](draft/2019-09/release-notes.html)
    - [Draft-06 to Draft-07](draft-07/json-schema-release-notes.html)
    - [Draft-04 to Draft-06](draft-06/json-schema-release-notes.html)
- JSON Hyper-Schema
    - [Draft-07 to 2019-09](draft/2019-09/release-notes.html#hyper-schema-vocabulary)
    - [Draft-04 to Draft-07](draft-07/json-hyper-schema-release-notes.html)
    - [Draft-04 to Draft-06](draft-06/json-hyper-schema-release-notes.html)

Older drafts
------------

Please see [Specification Links](specification-links.md) for older drafts and the latest unreleased version of the specification.
