---
layout: page
title: Work In Progress Feedback
permalink: /work-in-progress
---

* TOC
{:toc}

## The Next Draft (by whatever name) Is Ready for Final Pre-Publication Review!

The forthcoming draft is now feature-frozen and has gone through the initial four-week pre-publication review period.

_**UPDATE: July 28th, 2019** We continue to incorporate feedback and clarify sections of the new draft.  We expect to finally publish the draft during August._

The following sorts of feedback are particularly desired:

* Clarity, readability, and accessibility to new readers
* Contradictions, inconsistencies and other bugs
* Anything so egregious that implementation would be infeasible or impossible

New features or major changes to existing features will be deferred to a future draft,
and work on the next draft will begin immediately after this one is published.

## What Is This Draft Called, Anyway?

We have typically referred to this as `draft-08`, following our own convention of just
incrementing the draft number for meta-schemas regardless of the IETF identifier.

However, this is frowned upon by the IETF and has in fact been a source of confusion.
It worked well up to and including `draft-04`, as even after the split into multiple
documents, `draft-zyp-json-schema-04` (the core spec) at least ended in `-04`!
Unfortunately, after that point, the numbers stopped matching in any way.

Assuming this new draft gets published more or less as presented in this WIP,
and by the end of June, it will be:

* IETF identifiers: `draft-handrews-*-02`
* Meta-schema identifiers: `draft/2019-08`

Meta-schemas will now be identified with dates, which can be correlated with the publication
and expiration dates of the IETF documents.  This will make it more clear when a bugfix
meta-schema is produced, and also make it more obvious that the IETF and meta-schema identifiers
are not expected to match exactly.

## Change Log

To see what changes are involved, read the [release notes](./WIP-json-schema-release-notes.md).

## Pre-Built Review Documents

To encourage as many reviewers as possible, pre-built documents are available.  These may
slightly lag those in the GitHub repository, although all efforts will be made to keep them
in sync during the review period.  After the review period, these will be removed.  In particular,
using "WORK IN PROGRESS" meta-schemas **is not allowed** and **will break** as soon as the files
are moved to their permanent homes.

### Specifications:

As always, these are the actual normative documents:

_**NOTE:** Links to meta-schemas and among the drafts will not work correctly in these review copies._

* [WIP: JSON Schema (core)](/work-in-progress/WIP-jsonschema-core.html)
* [WIP: JSON Schema Validation](/work-in-progress/WIP-jsonschema-validation.html)
* [WIP: JSON Schema Hyper-Schema](/work-in-progress/WIP-jsonschema-hyperschema.html)
* [WIP: Relative JSON Pointer](/work-in-progress/WIP-relative-json-pointer.html)

### General use meta-schemas:

These are the traditional non-normative meta-schemas, which serve the same purpose as
in previous drafts, although their internal structure is different.

_**NOTE:** when published, the `.json` will be removed from the final URI_

* [WIP: schema.json](/work-in-progress/WIP-schema.json) (core and validation)
* [WIP: links.json](/work-in-progress/WIP-links.json) (individual link description object)
* [WIP: hyper-schema.json](/work-in-progress/WIP-hyper-schema.json) (hyper-schema, references schema and links)

### Single-vocabulary meta-schemas:

The new draft introduces the concept of modular vocabularies.  Most schema authors will not directly
reference these meta-schemas.  Instead, they are combined in useful ways by the general use meta-schemas.
However, those wishing to build custom meta-schemas may find it useful to choose different subsets of
the standard keywords depending on the custom meta-schema's intended purpose.

_**NOTE:** when published, the `.json` will be removed from the final URI_

* [WIP: meta/core.json](/work-in-progress/meta/WIP-core.json) (core keywords, from the core spec)
* [WIP: meta/applicator.json](/work-in-progress/meta/WIP-applicator.json) (applicator keywords, from the core spec)
* [WIP: meta/validation.json](/work-in-progress/meta/WIP-validation.json) (validation assertions, from the validation spec)
* [WIP: meta/meta-data.json](/work-in-progress/meta/WIP-meta-data.json) (meta-data annotations, from the validation spec)
* [WIP: meta/format.json](/work-in-progress/meta/WIP-format.json) (the format keyword, from the validation spec)
* [WIP: meta/content.json](/work-in-progress/meta/WIP-content.json) (content keywords, from the validation spec)
* [WIP: meta/hyper-schema.json](/work-in-progress/meta/WIP-hyper-schema.json) (hyper-schema keywords, from the hyper-shema spec)

### Output schema:

The new draft also introduces recommended output formats for reporting errors and annotations.

_**NOTE:** when published, the `.json` will be removed from the final URI_

* [WIP: schema.json](/work-in-progress/output/WIP-schema.json) (schema for recommended output formats)
* [WIP: verbose-example.json](/work-in-progress/output/WIP-verbose-example.json) (example of the most verbose output format)
* [WIP: hyper-schema.json](/work-in-progress/output/WIP-hyper-schema.json) (format used by the proposed hyper-schema test suite, and used in examples in the specification)

## Providing Feedback and Tracking Progress in GitHub

We are **particularly interested** in feedback on whether the wording and concepts is an
improvement in terms of how easy it is to understand and learn the specifications.

Note that _all drafts have Changelog appendicies_, for a concise list of notable changes.

* The active sources are on the
["master" branch of json-schema-org/json-schema-spec](https://github.com/json-schema-org/json-schema-spec)
* The [draft-08 milestone](https://github.com/json-schema-org/json-schema-spec/milestone/6)
  tracks all issues and PRs for this draft
  _(yes, it's still called draft-08 here because GitHub doesn't handle milestone renames well)_
* Check the [open PRs](https://github.com/json-schema-org/json-schema-spec/pulls)
  to see what is already being changed from other feedback
* [file an issue](https://github.com/json-schema-org/json-schema-spec/issues/new?milestone=draft-08)
  or [join us on Slack](https://join.slack.com/t/json-schema/shared_invite/enQtMjk1NDcyNDI2NTAwLTcyYmYwMjdmMmUxNzZjYzIxNGU2YjdkNzdlOGZiNjIwNDI2M2Y3NmRkYjA4YmMwODMwYjgyOTFlNWZjZjAyNjg) to submit feedback
  _(technically there is also a [mailing list](https://groups.google.com/forum/#!forum/json-schema) but it gets very little traffic and is not closely monitored)_

