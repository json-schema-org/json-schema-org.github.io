---
layout: default
title: JSON Schema
permalink: /
---


**JSON Schema** is a vocabulary that allows you to **annotate** and **validate** JSON documents.


## Benefits

<div class="block" markdown="1">

* Describes your existing data format(s).
* Provides clear human- and machine- readable documentation.
* Validates data which is useful for:
    * Automated testing.
    * Ensuring quality of client submitted data.

</div>

## Announcements and Feedback Solicitation: Specification Process

* The JSON Schema media types (`application/schema+json` and `application/schema-instance+json`) will be published as an [IETF RFC](https://datatracker.ietf.org/doc/draft-ietf-httpapi-rest-api-mediatypes/), which has already been adopted by the HTTP APIs working group.
* As an [Incubation-status OpenJS Foundation project](https://openjsf.org/projects/#incubating), we continue to work through our governance [todo list](https://github.com/json-schema-org/community/issues/129) to move to either At-Large or Impact status.
* The bulk of our specification will be published under a new process currently [under public discussion](https://github.com/orgs/json-schema-org/discussions/234).  All are encouraged to provide feedback!  Our goals with this process include:
    * In the next release, offer stability guarantees for long-stable aspects of JSON Schema.
    * Provide clarity regarding which other aspects are close to a stable form, and which are more experimental.
    * Publish our specifications in a way similar to OpenAPI and AsyncAPI, which are also part of the Linux Foundation (the larger umbrella under which the OpenJS Foundation exists).
* We are working on finding the right path for Relative JSON Pointer to reach standardization in the near future.  An IETF RFC currently remains the most likely path, although several details are still being worked out.

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

Office Hours are every first Tuesday of the month at 15:00 UTC, and by appointment.

Open Community Working Meetings are every Monday at 14:00 PT.

If either of these are cancelled or moved for any reason, we will aim to announce such via the Slack announcement channel and Twitter.
See our [community calendar](https://calendar.google.com/calendar/u/0/embed?src=c_8r4g9r3etmrmt83fm2gljbatos@group.calendar.google.com) (and make sure to check the time zone).

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

2022-06-10: A patch release of Draft 2020-12 has been published with no functional changes.

The new IETF document IDs are of the form `draft-bhutton-*-01`.

2021-02-01: Draft 2020-12 has been published!

The IETF document IDs are of the form `draft-bhutton-*-00`.

We are using dates for meta-schemas, which are what implementations should use to determine behavior,
so we will usually refer to `2020-12` (without the word "draft") on this web site.

See the [Specification page](specification.html) for details about naming and numbering.

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

* [GitHub](https://github.com/json-schema-org/json-schema-spec)
* [GitHub Discussions](https://github.com/json-schema-org/community/discussions)
* [Google Groups](https://groups.google.com/forum/#!forum/json-schema)
* [Slack](/slack)
* [Open Collective](https://opencollective.com/json-schema)
