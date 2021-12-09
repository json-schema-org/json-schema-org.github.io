---
layout: default
title: JSON Schema
permalink: /
---


**JSON Schema** is a vocabulary that allows you to **annotate** and **validate** JSON documents. It enables the confident and reliable use of the JSON data format.

## Mission

Our mission is to enable the definition of interoperable contracts for JSON data in the form of schemas, using constraints based validation rules, while enabling and supporting use cases beyond validation.

## Benefits

<div class="block" markdown="1">

* Describes your existing data format(s).
* Provides clear human- and machine- readable documentation.
* Validates data which is useful for:
    * Automated testing.
    * Ensuring quality of client submitted data.

</div>

## What now?

Learn, Get help, Shape the Community, Chat, with the JSON Schema team and Community!

<div class="wrapper text-center buttons">
    <a class="button border button-center" href="/learn/getting-started-step-by-step">👋 Getting Started</a>
    <a class="button border button-center" href="/slack" target="_blank"><img class="small-svg-logo" src="/assets/logo-slack.svg" height="1.3em" width="1.3em"> Open JSON Schema Slack</a>
    <a class="button border button-center" href="https://github.com/json-schema-org/community/discussions" target="_blank">💬 Community Discussions</a>
</div>

## Regular Activities

We hold weekly Office Hours and twice monthly Open Community Working Meetings.

<div class="wrapper text-center buttons">
    <a class="button border button-center" href="https://github.com/json-schema-org/community/discussions/34" target="_blank">🧑‍💻 Office Hours</a>
    <a class="button border button-center" href="https://github.com/json-schema-org/community/discussions/35" target="_blank">👷 Open Community Working Meetings</a>
</div>

Office Hours are every Tuesday at 15:00 UTC.

Open Community Working Meetings follow two patterns:
- First Friday of the month at 20:00 UTC.
- Third Friday of the month at 15:00 UTC.

## Need more?

We have our other [learning resources](/learn), including the [Understanding JSON Schema documentation](/understanding-json-schema).

## About Our Community

We have an active and growing community. All are welcome to be part of our community, help shape it, or simply observe.

We want to keep our community welcoming and inclusive, so please read our [JSON Schema Organizational Code of Conduct](https://github.com/json-schema-org/.github/blob/main/CODE_OF_CONDUCT.md). (This is a combination of the Contributor Covenant and IETF BCP 54.)

The JSON Schema team and community are here to help!

At any point, feel free to join our [Slack server](/slack).

Our Slack server has limited history, so we also use [GitHub Discussions](https://github.com/json-schema-org/community/discussions).

We monitor the `jsonschema` tag on StackOverflow.

## Project Status

2021-02-01: Draft 2020-12 has been published!

The IETF document IDs are of the form `draft-bhutton-*-00`.

We are using dates for meta-schemas, which are what implementations should use to determine behavior,
so we will usually refer to `2020-12` (without the word "draft") on this web site.

See the [Specification page](specification.html) for details about naming and numbering.

### The Path to Standardization

The JSON Schema project intends to shepherd all three draft series to either: RFC status, the equivalent within another standards body, and/or join a foundation and establish self publication rules.

<details markdown="1">
<summary>Read more</summary>

Currently, we are continuing to improve our self-published Internet-Drafts. We are not actively pursuing joining a standards organisation.

We have a few contacts related to each potential path, but if you have experience with such things and would like to help, please still contact us!

In the meantime, publication of Internet-Draft documents can be tracked through the IETF:
* [JSON Schema (core)](https://datatracker.ietf.org/doc/draft-bhutton-json-schema/)
* [JSON Schema Validation](https://datatracker.ietf.org/doc/draft-bhutton-json-schema-validation/)
* [Relative JSON Pointers](https://datatracker.ietf.org/doc/draft-bhutton-relative-json-pointer/)

Internet-Drafts expire after six months, so our goal is to publish often enough to always have a set of unexpired drafts available.  There may be brief gaps as we wrap up each draft and finalize the text.
</details>

### Use of the _draft_ designation
Releases of the JSON schema specification and meta schemas use the _draft_ designation primarily for historical reasons stemming from the relationship of this specification to IETF ([explained here](https://json-schema.org/specification-links.html#understanding-draft-names-and-numbers)).
The use of this designation is under review but will continue until this review process completes to avoid changing the designation style multiple times.
<details markdown="1">
<summary>Read more</summary>

The JSON schema project recognizes, condones, and advocates for the use of the JSON schema standard in production.

Each release of the JSON schema specification is treated as a production release by the JSON schema project. All changes in each new release are made judiciously, with great care, thorough review and careful consideration of how the changes will impact existing users and implementations of the JSON schema specification.

Similarly to most specifications, the JSON schema specification will continue to evolve, and not all releases will be backwards compatible. The intention, particularly for vocabularies such as validation which have been widely implemented, is to remain as compatible as possible from release to release. However, major changes can still occur given a clear enough need validated with the user community.

When the _draft_ designation is dropped this may indicate that the frequency of releases and amount of changes in each release will decrease, but it won't indicate that no new releases will be made, or that all future releases will be backwards compatible.
</details>
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

## JSON Hyper-Schema

JSON Hyper-Schema is on hiatus / not currently maintained as of 2021.

This allows the team to focus the little time they do donate on JSON Schema core and validation.

We may revisit JSON Hyper-Schema at a later date.

## More Links

Interested? Check out:

* [Understanding JSON Schema](/understanding-json-schema/)
* The [specification](./specification.md)
* [Learning resources](./learn/index.md)
* the growing list of [JSON Schema software](./implementations.md)

We encourage updating to the latest specification where possible, which is 2020-12.

Questions? Feeling helpful? Get involved on:

* [GitHub](http://github.com/json-schema-org/json-schema-spec)
* [GitHub Discussions](https://github.com/json-schema-org/community/discussions)
* [Google Groups](https://groups.google.com/forum/#!forum/json-schema)
* [Slack](/slack)
* [Open Collective](https://opencollective.com/json-schema)
