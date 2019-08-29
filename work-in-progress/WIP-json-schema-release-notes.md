---
title: JSON Schema Draft-2019/09 Release Notes
layout: page
---

There are a few non-breaking changes with keywords being deprecated and slates
for removal, and a few newer keywords based on community feedback. Internally
there has been a lot of restructuring through a new concept called vocabularies,
but on the whole things are mostly the same.

* TOC
{:toc}

- Update to RFC 8359 for JSON specification
- Add the concept of formal vocabularies, and how they can be recognized through meta-schemas
- Formalized annotation collection
- Moved applicator keywords from the Validation specification as their own vocabulary
- Define "$ref" behavior in terms of the assertion, applicator, and annotation model
- Note undefined behavior for "$ref" targets involving unknown keywords
- Additional guidance on initial base URIs beyond network retrieval
- Allow "schema" media type parameter for "application/schema+json"

### Keywords

* Eight brand new keywords were added
* One keyword was renamed
* One keyword was split in half
* One keyword in Core changed behavior
* One keyword in Hyper-Schema changed behavior

keyword | change | notes
---- | ---- | ----
[`"definitions"`](json-schema-core.html#rfc.section.TODO) | **renamed** | use new "$defs" core keyword
[`"unevaluatedProperties" and "unevaluatedItems"`](json-schema-core.html#rfc.section.TODO) | **added** |
[`"$ref"`](json-schema-core.html#rfc.section.TODO) | **changed** | can now have siblings (keywords next to it)
[`"$defs"`](json-schema-core.html#rfc.section.10.TODO) | **added** | moved over to core from validation
`"dependencies"` | **removed** | use "dependentRequired" or "dependentSchemas"
[`"dependentRequired"`](json-schema-core.html#rfc.section.10.TODO) | **added** | added to core schema
[`"dependentSchemas"`](json-schema-core.html#rfc.section.10.TODO) | **added** | added to core schema
[`"minContains" and "maxContains"`](json-schema-validation.html#rfc.section.TODO) | added |
[`"contentSchema"`](json-schema-validation.html#rfc.section.TODO) | added | allows applying a schema to a string-encoded document
[`"deprecated"`](json-schema-validation.html#rfc.section.TODO) | added |
[`"rel"`](json-schema-hyperschema.html#rfc.section.TODO) | **changed** | Can now be an array of values instead of just a string

### Formats

Two formats were added.

format | change | notes
---- | ---- | ----
[`"uuid"`](json-schema-validation.html#rfc.section.7.3.5) | added | A string instance is valid against this attribute if it is a valid string representation of a UUID, according to RFC4122
[`"duration"`](json-schema-validation.html#rfc.section.7.3.5) | added | The duration format is from the ISO 8601 ABNF as given in Appendix A of RFC 3339
