---
layout: page
title: Specification
---

The latest Internet-Drafts at the IETF are the **draft-handrews-json-schema\*-00** documents, which correspond to the **draft-07** meta-schemas. These were published on **2017-11-19**. (Due to a change in author/editorship the I-D numbering was reset to -00 again).

Please see the **[migration notes](draft-06/README.md)** (which will soon be updated for draft-07) for more information on this release and on migrating from draft-04.

The specification is split into three parts, Core, Validation, and Hyper-Schema, along with a related specification, Relative JSON Pointers:

|--------------------------------------------------------------|-------------------------------------------------------|
| [JSON Schema Core](latest/json-schema-core.html)             | defines the basic foundation of JSON Schema           |
| [JSON Schema Validation](latest/json-schema-validation.html) | defines the validation keywords of JSON Schema        |
| [JSON Hyper-Schema](latest/json-schema-hypermedia.html) ([errata](https://github.com/json-schema-org/json-schema-spec/pull/508))     | defines the hyper-media keywords of JSON Schema       |
| [Relative JSON Pointers](latest/relative-json-pointer.html)  | extends the JSON Pointer sytnax for relative pointers |

They are also available on the IETF main site:
* [draft-handrews-json-schema-00 (core)](http://tools.ietf.org/html/draft-handrews-json-schema-00)
* [draft-handrews-json-schema-validation-00](http://tools.ietf.org/html/draft-handrews-json-schema-validation-00)
* [draft-handrews-json-schema-hyperschema-00](http://tools.ietf.org/html/draft-handrews-json-schema-hyperschema-00)
* [draft-handrews-relative-json-pointer-00](https://tools.ietf.org/html/draft-handrews-relative-json-pointer-00)

Meta-schemas
------------

The meta-schemas are schemas against which other schemas can be validated. They are self-descriptive: the JSON Schema meta-schema validates itself, while the JSON Hyper-Schema meta-schema both validates itself and defines its own "self" link.
The latest meta-schema is **draft-07**.

|--------------------------------------------------------------|------------------------------------------------------------|
| [Core/Validation meta-schema](http://json-schema.org/draft-07/schema) | Used for schemas written for pure validation.              |
| [Hyper meta-schema](http://json-schema.org/draft-07/hyper-schema)     | Used for schemas written for validation and hyper-linking. |

_If you are accessing the above meta-schema links **from a web browser**, you will need to **save the file** then open it as a JSON document._

Migrating from draft-04
-------------

_**NOTE:** The release notes will be updatead for draft-07 soon_

The release notes discuss the changes impacting users and implementors:

- [JSON Schema Core and Validation](draft-06/json-schema-migration-faq.md)
- [JSON Hyper-Schema](draft-06/json-hyper-schema-migration-faq.md)

Older drafts
------------

Please see [Specification Links](specification-links.md) for older drafts and the latest unreleased version of the specification.
