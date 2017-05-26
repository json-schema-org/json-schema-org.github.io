---
layout: page
title: Specification
---

The latest Internet-Drafts at the IETF are the draft-wright-json-schema\*-01 documents, which correspond to the draft-06 meta-schemas. These were published on 2017-04-15. (Due to a change in authorship the I-D numbering was reset with the previous draft).  Please see the [release notes](draft-06/README.md) for more information on this release and information on migrating from previous drafts.

The specification is split into three parts, Core, Validation, and Hyper-Schema:

|--------------------------------------------------------------|-------------------------------------------------|
| [JSON Schema Core](latest/json-schema-core.html)             | defines the basic foundation of JSON Schema     |
| [JSON Schema Validation](latest/json-schema-validation.html) | defines the validation keywords of JSON Schema  |
| [JSON Hyper-Schema](latest/json-schema-hypermedia.html)      | defines the hyper-media keywords of JSON Schema |

They are also available on the IETF main site: [core (draft-wright-json-schema-01)](http://tools.ietf.org/html/draft-wright-json-schema-01), [validation (draft-wright-json-schema-validation-01)](http://tools.ietf.org/html/draft-wright-json-schema-validation-01) and [hyper-schema (draft-wright-json-schema-hyperschema-01)](http://tools.ietf.org/html/draft-wright-json-schema-hyperschema-01).

Meta-schemas
------------

The meta-schemas are schemas against which other schemas can be validated. They are self-descriptive: the JSON Schema meta-schema validates itself, while the JSON Hyper-Schema meta-schema both validates itself and defines its own "self" link.
The latest meta-schema is draft-06.

|--------------------------------------------------------------|------------------------------------------------------------|
| [Core/Validation meta-schema](http://json-schema.org/schema) | Used for schemas written for pure validation.              |
| [Hyper meta-schema](http://json-schema.org/hyper-schema)     | Used for schemas written for validation and hyper-linking. |

**If you are accessing the above meta-schema links from a web browser, you will need to save the file then open it as a JSON file.**

Other versions
--------------

Please see [Specification Links](specification-links.md) for older drafts and the latest unreleased version of the specification.
