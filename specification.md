---
layout: page
redirect_from: "/documentation.html"
permalink: /specification.html
title: Specification
---

The current version is *2020-12*!
The previous version was 2019-09.

Specification documents
-----------------------

See also the release notes / change log (Work in progress).

The specification is split into two parts, Core and Validation. We also publish
the Relative JSON Pointers spec although it's not currently used by Core or
Validation in any significant way.

|--------------------------------------------------------------|-------------------------------------------------------|
| [JSON Schema Core](draft/2020-12/json-schema-core.html)             | defines the basic foundation of JSON Schema           |
| [JSON Schema Validation](draft/2020-12/json-schema-validation.html) | defines the validation keywords of JSON Schema        |
| [Relative JSON Pointers](draft/2020-12/relative-json-pointer.html)  | extends the JSON Pointer syntax for relative pointers |

They are also available on the IETF main site:
* [draft-bhutton-json-schema-01 (core)](https://tools.ietf.org/html/draft-bhutton-json-schema-01)
* [draft-bhutton-json-schema-validation-01](https://tools.ietf.org/html/draft-bhutton-json-schema-validation-01)
* [draft-bhutton-relative-json-pointer-00](https://tools.ietf.org/html/draft-bhutton-relative-json-pointer-00)

Meta-schemas
------------

The meta-schemas are schemas against which other schemas can be validated. It is self-descriptive: the JSON Schema meta-schema validates itself.

The latest meta-schema is **2020-12**.  For an explanation of the change to date-based identifiers, see the [Specification Links](specification-links.html) page.

_If you are accessing these JSON document links **from a web browser**, you will need to **save the file** then open it as a JSON document.  This is due to limitations with GitHub Pages._

## General-purpose meta-schema

Please note, additional vocabulary specific schema files are needed to fully construct and use the Core/Validation Dialect meta-schema.

|--------------------------------------------------------------|------------------------------------------------------------|
| [Core/Validation Dialect meta-schema](draft/2020-12/schema)  | Used for schemas written for pure validation.              |
| [Hyper-Schema Dialect meta-schema](draft/2020-12/hyper-schema) | Used for schemas written for validation (2020-12) and hyper-linking (2019-09).|
| [Recommended Output meta-schema](draft/2020-12/output/schema)| Recommended output structure of the application process.   |

## Single-vocabulary meta-schemas

These are relevant primarily to people who want to write their own meta-schemas that build on specific parts of the existing specification.

- [Core Vocabulary meta-schema](draft/2020-12/meta/core)
- [Applicator Vocabulary meta-schema](draft/2020-12/meta/applicator)
- [Validation Vocabulary meta-schema](draft/2020-12/meta/validation)
- [Unevaluated Vocabulary meta-schema](draft/2020-12/meta/unevaluated)
- [Format Annotation Vocabulary meta-schema](draft/2020-12/meta/format-annotation)
- [Format Assertion Vocabulary meta-schema](draft/2020-12/meta/format-assertion)
- [Content Vocabulary meta-schema](draft/2020-12/meta/content)
- [Meta-Data Vocabulary meta-schema](draft/2020-12/meta/meta-data)

## Output schemas and examples
- [JSON Schema recommended output schema](draft/2020-12/output/schema)
- [JSON Schema verbose output example](draft/2020-12/output/verbose-example)

Migrating from older drafts
-------------

The release notes discuss the changes impacting users and implementers:

- JSON Schema Core and Validation
    - [Draft 2019-09 to Draft 2020-12](draft/2020-12/release-notes.html)
    - [Draft-07 to Draft 2019-09](draft/2019-09/release-notes.html)
    - [Draft-06 to Draft-07](draft-07/json-schema-release-notes.html)
    - [Draft-04 to Draft-06](draft-06/json-schema-release-notes.html)
- JSON Hyper-Schema
    - There was no JSON Hyper-Schema draft for 2020-12 releases.
    - [Draft-07 to 2019-09](draft/2019-09/release-notes.html#hyper-schema-vocabulary)
    - [Draft-04 to Draft-07](draft-07/json-hyper-schema-release-notes.html)
    - [Draft-04 to Draft-06](draft-06/json-hyper-schema-release-notes.html)

Older drafts
------------

Please see [Specification Links](specification-links.md) for older drafts and the latest unreleased version of the specification.
