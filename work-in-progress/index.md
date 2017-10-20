---
layout: page
title: Work In Progress Feedback
permalink: /work-in-progress
---

* TOC
{:toc}

The next draft (informally known as **draft-07**, formal IETF draft names to be determined)
is now feature-frozen and entering the final feedback month before publication.  The feedback
period will last from October 20th to November 20th, 2017.

The timeline may be compressed if, after at least two weeks, there is a clear consensus that
the drafts are solid and it would be best to publish sooner rather than later.

The following sorts of feedback are particularly desired:

* Clarity, readability, and accessibility to new readers
* Contradictions, inconsistencies and other bugs
* Anything so egregious that implementation would be infeasible or impossible

New features or major changes to existing features will be deferred to a future draft,
and work on the next draft will begin immediately after this one is published.

## Pre-Built Review Documents

To encourage as many reviewers as possible, pre-built documents are available.  These may
slightly lag those in the GitHub repository, although all efforts will be made to keep them
in sync during the review period.  After the review period, these will be removed.  In particular,
using "WORK IN PROGRESS" meta-schemas **is not allowed** and **will break** as soon as the files
are moved to their permanent homes.

* Specifications:
    * [WIP: JSON Schema (core)](/work-in-progress/WIP-jsonschema-core.html)
    * [WIP: JSON Schema Validation](/work-in-progress/WIP-jsonschema-validation.html)
    * [WIP: JSON Schema Hyper-Schema](/work-in-progress/WIP-jsonschema-hyperschema.html)
    * [WIP: Relative JSON Pointer](/work-in-progress/WIP-relative-json-pointer.html)
* Meta-schemas:
    * [WIP: schema.json](/work-in-progress/WIP-schema.json) (core and validation)
    * [WIP: links.json](/work-in-progress/WIP-links.json) (individual link description object)
    * [WIP: hyper-schema.json](/work-in-progress/WIP-hyper-schema.json) (hyper-schema, references
                                                                schema and links)
* Output schema:
    * [WIP: hyper-schema-output.json](/work-in-progress/WIP-hyper-schema-output.json) (format used by the proposed hyper-schema test suite, and used in examples in the specification)


## Providing Feedback and Tracking Progress in GitHub

* The active sources are on the
["master" branch of json-schema-org/json-schema-spec](https://github.com/json-schema-org/json-schema-spec)
* The [draft-07 milestone](https://github.com/json-schema-org/json-schema-spec/milestone/5)
  tracks all issues and PRs for this draft
* Check the [open PRs](https://github.com/json-schema-org/json-schema-spec/pulls)
  to see what is already being changed from other feedback
* [file an issue](https://github.com/json-schema-org/json-schema-spec/issues/new?milestone=draft-07)
  or [join the mailing list](https://groups.google.com/forum/#!forum/json-schema) to submit feedback

## Summary of Changes

The primary focus of this draft has been JSON Hyper-Schema, which has been given a top-to-bottom
rewrite in addition to numerous new features.  As part of this, we have (with the original
author's permission), revived the Relative JSON Pointer proposal to be submitted alongside
of hyper-schema.

In the course of that rewrite, several concepts that apply across all JSON Schema specifications
became more clear, so in addition to new keywords in both Core and Validation, some of the
introductory and other text has been reworked to fit into the new conceptual model.

We are **particularly interested** in feedback on whether the wording and concepts is an
improvement in terms of how easy it is to understand and learn the specifications.

Note that _all drafts have Changelog appendicies_, for a concise list of notable changes.

### Changes to Concepts

These are properly explained in the documents as noted.  They are listed here to
highlight their importance: If you read the drafts and do not understand these
concepts, please file an issue or let us know on the mailing list.

* _applicability_ (defined in the validation spec)
* _assertions_ (defined in the validation spec, also mentioned in core)
* `true` and `false` as trivial assertions (explained in core)
* _annotations_ (defined in the validation spec)
* _generic user agent_ (defined in hyper-schema)
* _client application_ and _client input_ (defined in hyper-schema)

### Changes to Specification Boundaries

The `readOnly` and `media` keywords, previously in Hyper-Schema, are often used (or people
ask about using them) outside of Hyper-Schema.  As a result, they have been moved to the
validation spec:

* `readOnly` is now a metadata keyword in the validation spec
* `media` and its `type` and `binaryEncoding` keywords get their own validation spec section as `contentMediaType` and `contentEncoding`

Hyper-schema now solely describes the Link Description Object (LDO), plus the two keywords
(`base` and `links`) necessary to connect the LDO to schemas.

### Changes to the Core Specification

* `application/schema-instance+json` optional instance media type
* Proposed "schema" link relation type in place of using "profile"
* Added `$comment` (also added to Hyper-Schema's LDO)
* Better wording around `$id` and fragments
* Note the challenges of extending meta-schemas more clearly

### Changes to the Validation Specification

Only `if`/`then`/`else` is a mandatory new implementation requirement.  Everything else
is either an annotation (no defined requirements) or gives implementations the option
to **not** implement validation (format and content keywords).

* Added `if`, `then`, and `else`
    * Note: imperative vs declarative [exhaustively discussed](https://github.com/json-schema-org/json-schema-spec/issues/180)
    * Note: we [may remove the schema form of `dependencies`](https://github.com/json-schema-org/json-schema-spec/issues/442)
* Added many new formats, clarified the "json-pointer" format
* Added `writeOnly` alongside `readOnly` (moved from Hyper-Schema)
* Moved the hyper-schema `media` object to the `content*` keywords in validation

### Changes to the Hyper-Schema specification


Basically everything is different.  It's probably best to just read it top to bottom
and pretend you've never heard of it before.

* Added overview explaining the general purpose and scope
* Aligned LDO with how links are described in [RFC 5988bis](https://mnot.github.io/I-D/rfc5988bis/)
* Context adjustment with `anchor` and `anchorPointers`
* Pseudocode specification for URI Template resolution
* `templatePointers` and `templateRequired` added to URI Template resolution process
* `mediaType` and `submissionEncType` are now `targetMediaType` and `submissionMediaType`
* `targetHints` and `headerSchema` for working with HTTP request and response headers
* HTTP guidance has its own section
* API usage has an appendix, including a subsection on static docs and code
* Collections and items are recognizable based on link relation types

## Known Issues

* Hyper-Schema needs a lot more examples (suggestions encouraged!)
* Hyper-Schema examples would be better if they fit together to tell a single story
* Hyper-Schema needs to be consistent about cross-references

The primary author of this draft (and this page) notoriously struggles to write
effective examples.  Help on this aspect of the spec would be very much appreciated.
