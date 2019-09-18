---
title: JSON Schema Draft-2019/09 Release Notes
layout: page
---

_NOTE: This page is still being written, and is currently a fairly minimal listing of changes._

For the vast majority of schema authors, we hope that these changes are minimally disruptive.

The most likely to be frustrating is that `format` is no longer treated as a validation assertion _by default_ (although it is still possible for an application or user to configure a validator to treat it as one).  We decided this was acceptable because many schema authors are already extremely frustrated by its inconsistent behavior.

For implementors, there is a lot more to consider, and further guidance on implementation topics will be forthcoming.

* TOC
{:toc}

For a basic list of changes to each document, see their change logs:
* [Core](/draft/2019-09/json-schema-core.html#rfc.appendix.G)
* [Validation](/draft/2019-09/json-schema-validation.html#rfc.appendix.C)
* [Hyper-Schema](/draft/2019-09/json-schema-hypermedia.html#rfc.appendix.B)

### Incompatible Changes

* By default, `format` is no longer an assertion.  This has been done because the inconsistent implementation of `format` as an assertion has been an endless source of surprising problems for schema authors.  The default behavior will now be predictable, if not ideal.  There are several ways to turn on assertion functionality, as explained below.  However, we recommend doing semantic validation in the application layer.
* Plain name fragments are no longer defined with `$id`, but instead with the new keyword `$anchor` (which has a different syntax).
* `$id` cannot contain a fragment anymore (except possibly an empty fragment, although that is discouraged).
* In cases where multiple URIs could be used for the same schema, some are now discouraged.  These are believed to have rarely been used, as the behavior involved was fairly confusing and not well explained until the updated version of draft-07 (draft-handrews-json-schema-01).  If this doesn't mean much to you, you are probably safe.

### Semi-incompatible Changes

The old syntax for these keywords is not an error (and the default meta-schema still validates them), so implementations can therefore offer a compatibility mode.  However, migrating to the new keywords is straightforward and should be preferred.

* `definitions` is now `$defs`
* `dependencies` has been split into `dependentSchemas` and `dependentRequired`

### Annotations, Errors, and Outputs

[Annotation keywords](/draft/2019-09/json-schema-core.html#rfc.section.7.7) such as `title`, `readOnly`, and `default` have always been a part of JSON Schema, but without any guidance on how to make use of them.  This draft formalizes how implementations can make annotation information available to applications.

Similarly, there has not previously been guidance on what constitutes useful error reporting when validation fails.

To solve both of these problems, we now recommend that implementations support one or more of standardized [output formats](/draft/2019-09/json-schema-core.html#rfc.section.10).

### Keyword Changes

All keywords have now been organized into [vocabularies](/draft/2019-09/json-schema-core.html#rfc.section.8.1), with the Core and Validation specifications containing multiple vocabularies.  In this process, some keywords have moved from Validation into Core.

#### Core Vocabulary

[Core Specification, Section 8](/draft/2019-09/json-schema-core.html#rfc.section.8)

keyword | change | notes
---- | ---- | ----
[`$anchor`](/draft/2019-09/json-schema-core.html#rfc.section.8.2.3) | **new** | Replaces the `#plain-name` form of `$id`, with a different syntax and approach
[`$defs` (renamed from `definitions`)](/draft/2019-09/json-schema-core.html#rfc.section.8.2.5) | **renamed** | Note that the standard meta-schema still reserves `definitions` for backwards compatibility
[`$id`](/draft/2019-09/json-schema-core.html#rfc.section.8.2.2) | **changed** | Only URI-references without fragments are allowed; see `$anchor` for a replacement for plain-name fragments; all other fragments in `$id` had undefined behavior previously
[`$recursiveAnchor` and `$recursiveRef`](/draft/2019-09/json-schema-core.html#rfc.section.8.2.4.2) | **new** | Used for extending recursive schemas such as meta-schemas
[`$ref`](/draft/2019-09/json-schema-core.html#rfc.section.8.2.4) | **changed** | Other keywords are now allowed alongside of it
[`$vocabulary`](/draft/2019-09/json-schema-core.html#rfc.section.8.1) | **new** | Has effects only in meta-schemas, and is used to control what keywords an implementation must or can support in order to process a schema using that meta-schema

#### Applicator Vocabulary

[Core Specification, Section 9](/draft/2019-09/json-schema-core.html#rfc.section.9)

These keywords were formerly found in the Validation Specification.

keyword | change | notes
---- | ---- | ----
[`dependentSchemas` (split from `dependencies`)](/draft/2019-09/json-schema-core.html#rfc.section.9.2.2.4) | **split** | This is the schema form of `dependencies`; note that the standard meta-schema still reserves `dependencies` for backwards compatibility
[`unevaluatedItems`](/draft/2019-09/json-schema-core.html#rfc.section.9.3.1.3) | **new** | Similar to `additionalItems`, but can "see" into subschemas and across references
[`unevaluatedProperties`](/draft/2019-09/json-schema-core.html#rfc.section.9.3.2.4) | **new** | Similar to `additionalProperties`, but can "see" into subschemas and across references

The other applicator vocabulary keywords are `items`, `additionalItems`, `properties`, `patternProperties`, `additionalProperties`, `anyOf`, `allOf`, `oneOf`, `not`, `if`, `then`, `else`.

#### Validation Vocabulary

[Validation Specification, Section 6](/draft/2019-09/json-schema-validation.html#rfc.section.6)

keyword | change | notes
---- | ---- | ----
[`dependentRequired` (split from `dependencies`)](/draft/2019-09/json-schema-validation.html#rfc.section.6.5.4) | **split** | This is the string array form of `dependencies`; note that the standard meta-schema still reserves `dependencies` for backwards compatibility
[`maxContains` and `minContains`](/draft/2019-09/json-schema-validation.html#rfc.section.6.4.4) | **new** | Assertion for controlling how many times a subschema must be matched within an array


#### Format Vocabulary

[Validation Specification, Section 7](/draft/2019-09/json-schema-validation.html#rfc.section.7)

The `format` keywords has always been problematic due to its optional nature.  There has never been a way to ensure that the implementation processing your schema supports `format` at all, or if it does, to what degree it validates each type of format.  In theory, since each format references a standard specification, if a format is supported, it should behave consistently.  In practice, this is not the case.

There are two ways for an application to validate formats: It can rely on a JSON Schema implementation to validate them (which may or may not have the expected results), or it can note where the `format` keyword has been used and perform its own validation based on that.  This second approach is supported by treating `format` as an [annotation keyword](/draft/2019-09/json-schema-core.html#rfc.section.7.7) and supporting the [basic, detailed, or verbose output formats](/draft/2019-09/json-schema-core.html#rfc.section.10.4.2).

To impose some predictability on this system, the behavior has changed in several ways, as illustrated below.  The key difference here is that `format` validation is now predictably off by default, but can be configured to be turned on.  In draft-07, it was on (but possibly unimplemented) by default and could be configured to be turned off.

In the following charts, the "supported" column refers to whether and (for `2019-09`) to what degree the implementation claims to support the `format` keyword.  The "configuration" column refers to whether some non-default behavior for `format` is configured somehow (in a configuration file, or through a command-line option, or whatever).

**Summary of draft-07 behavior**

supported   | configuration | outcome 
----------- | ------------- | -------------
no          | n/a           | not validated
yes         | _default_ (on)| inconsistently validated
yes         | off           | not validated

Obviously, each implementation will behave consistently from schema to schema, although some formats may be supported more thoroughly than others despite the wording in the specification.  However, complex formats are, in practice, supported to different degrees in each implementation.  If they are supported at all.

**Summary of 2019-09 behavior**

The goal with this draft is to make the default behavior predictable, with the inconsistent behavior as an opt-in feature.  This is not entirely satisfactory, but we feel that it is a good first step to reduce the number of complaints seen around surprising results.  This way, there should at least be fewer surprises.

* "best effort" validation is a fairly weak requirement, which matches how things work in practice today.  Simple formats are probably fully valid, complex formats may be minimally validated or even not validated at all.

* "full syntax" validation means that you can expect a reasonably thorough syntactic validation, probably corresponding to whatever commonly available libraries can do in the implementation language.  For formats such as IP addresses and dates, this is expected to be complete validation.  For more complex formats such as email addresses, support will probably still vary significantly.  It's unclear how many implementations have ever provided this level of support.

* An outcome of _vocabulary error_ means that the implementation will refuse to process the schema as it cannot satisfy the vocabulary requirement.

supported   | configuration  | vocabulary    | outcome 
----------- | -------------- | ------------- | -------------
no          | n/a            | false         | not validated
no          | n/a            | true          | _vocabulary error_
best effort | _default_ (off)| false         | not validated
best effort | _default_ (off)| true          | _vocabulary error_
best effort | on             | false         | best effort validation
best effort | on             | true          | _vocabulary error_
full syntax | _default_ (off)| false         | not validated
full syntax | _default_ (off)| true          | full syntax validation
full syntax | on             | false         | full syntax validation
full syntax | on             | true          | full syntax validation

Note that, given that almost no draft-07 or earlier implementations have offered strict and complete validation of every single format, it seems unlikely that any implementations will support option 3 option in practice.  

Additionally, two new formats were added, and a specification reference was updated:

format | change | notes
---- | ---- | ----
[`"duration"`](/draft/2019-09/json-schema-validation.html#rfc.section.7.3.1) | **added** | The duration format is from the ISO 8601 ABNF as given in Appendix A of RFC 3339
[`"hostname"` and `"idn-hostname"`](/draft/2019-09/json-schema-validation.html#rfc.section.7.3.3) | **updated** | Use RFC 1123 instead of RFC 1034; this allows for a leading digit
[`"uuid"`](/draft/2019-09/json-schema-validation.html#rfc.section.7.3.5) | **added** | A string instance is valid against this attribute if it is a valid string representation of a UUID, according to RFC4122


#### Content Vocabulary

[Validation Specification, Section 8](/draft/2019-09/json-schema-validation.html#rfc.section.8)

These keywords are now specified purely as annotations, and never assertions.  Some guidance is provided around how an implementation can optionally offer further automatic processing of this information outside of the validation process.

keyword | change | notes
---- | ---- | ----
[`contentEncoding`](/draft/2019-09/json-schema-validation.html#rfc.section.8.3) | **updated** | Encodings from RFC 4648 are now allowed, and take precedence over RFC 2045 when there is a difference
[`contentSchema`](/draft/2019-09/json-schema-validation.html#rfc.section.8.5) | **added** | Schema for use with the decoded content string; note that it is _not_ automatically applied as not all content media types can be understood in advance

#### Meta-Data Vocabulary

[Validation Specification, Section 9](/draft/2019-09/json-schema-validation.html#rfc.section.9)

keyword | change | notes
---- | ---- | ----
[`deprecated`](/draft/2019-09/json-schema-validation.html#rfc.section.9.3) | **added** | Used to indicate that a field is deprecated in some application-specific manner

#### Hyper-Schema Vocabulary

[Hyper-Schema Specification, Sections 5 and 6](/draft/2019-09/json-schema-hypermedia.html#rfc.section.5)

keyword | change | notes
---- | ---- | ----
[`rel`](/draft/2019-09/json-schema-hypermedia.html#rfc.section.6.2.1) | **changed** | Can now be an array of values instead of just a string
