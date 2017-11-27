---
title: JSON Hyper-Schema Draft-07 Release Notes
layout: page
---

JSON Hyper-Schema [draft-07](json-schema-hypermedia.html) completes the
reworking of Hyper-Schema that was begun in draft-06.

Hyper-Schema is now solely focused on adding hyperlinks to JSON documents,
so keywords used for other purposes (`readOnly` and `media`) have been
[moved to the Validation specification](json-schema-release-notes.html).

The [new draft](json-schema-hypermedia.html) has been completely rewritten
for clarity and accessibility, so it is best to simply approach it as a new
proposal.  We hope to add tutorial material at some point, but that is
outside of the scope of release notes.

However, if you wish to migrate from an earlier draft, this page is a guide
to the key _changes_.  The additions, which are much more numerous,
should be learned directly from the new specification until we can provide
tutorials.

* TOC
{:toc}


### Migrating from draft-06

No draft-06 features were changed, although two keywords were renamed
for clarity and consistency:

* `mediaType` -> `targetMediaType`
* `submissionEncType` -> `submissionMediaType`

Additionally, `hrefSchema` was somewhat confusing, so a great deal
more effort has gone into explaining how it works, and how it fits
into the overall link resolution process.

### Migrating from draft-05

See the [draft-06 release notes](../draft-06/json-hyper-schema-migration-faq.html)
for information related to draft-05.

### Migrating from draft-04

In the ideal draft-07 world, links and
[operations](http://json-schema.org/draft-07/json-schema-hypermedia.html#rfc.section.3.1)
are not the same concept.  Using terminology borrowed from
[OpenAPI's Operation Object](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md#operationObject), HTTP methods are operations, and each
link (as described by a single LDO) can support multiple operations.

Therefore, unlike draft-04, draft-07 hyper-schemas
[do not have separate links for each operation](json-schema-hypermedia.html#rfc.section.8.1).  This makes the migration guidelines below approximate at best.

For a more detailed explanation of how draft-04's `method` and `targetSchema`
were typically used to create single-operation links, and how that poses
a challenge for migrating to multi-operation links, see the
[draft-06 release notes](../draft-06/json-hyper-schema-migration-faq.html).
Those release notes also explain what happened to the link relations defined
in draft-04 and subsequently removed, and the changes in how the instance
base URI is determined.

Beyond those changes, a minimal migration would be something along the
following lines, although the
[intentional lack of explicit response descriptions](json-schema-hypermedia.html#rfc.appendix.A.2)
(except when the response happens to be a representation of the target resource)
means that some uses of draft-04 do not have direct analogues in draft-07.

Any keyword not mentioned in a list below is unchanged for that link operation.

#### GET

* `"method": "GET"` -> `"targetHints": {"allow": ["GET"]}`
* `mediaType` -> `targetMediaType`
* `schema` -> `hrefSchema` with parameters added to `href`
* `encType` -> drop if `application/x-www-form-urlencoded`, contact the mailing list otherwise

#### PUT

If you have a PUT link where `schema`/`encType` differ from
`targetSchema`/`mediaType`, where `targetSchema`/`mediaType`
describe a non-representation response, then those fields do
not have a direct replacement.

* `"method": "PUT"` -> `"targetHints": {"allow": ["PUT"]}`
* `schema` -> `targetSchema`
* `encType` -> `targetMediaType`

#### DELETE

DELETE does not take a request payload, so `schema` and `encType`
should be unused.  If `targetSchema` and `mediaType` were being
used for a response other than the just-deleted resource's representation,
then they do not have a direct replacement.

* `"method": "DELETE"` -> `"targetHints": {"allow": ["DELETE"]}`
* `mediaType` -> `targetMediaType` (if describing the representation)

#### POST

In most cases, the response of a POST is **not** a representation of the
target resource, but rather some sort of result or status of whatever
the POST attempted to do.  Therefore `targetSchema` and `mediaType`
should almost certainly be dropped.  Other than that:

* `"method": "POST"` -> `"targetHints": {"allow": ["POST"]}`
* `schema` -> `submissionSchema`
* `encType` -> `submissionMediaType`

#### PATCH

It was never entirely clear how to model a propert PATCH (that uses
a patch media type rather than `application/json` in the request) in Hyper-Schema.
One option was to treat it identically to PUT except with the patch media type
in `encType`.  Assuming that approach (and the same `taregetSchema`/`mediaType`
caveats as for PUT):

* `"method": "PATCH"` -> `"targetHints": {"allow": ["PATCH"]}`
* `schema` -> `targetSchema`
* `"encType": "..."` -> `"targetHints": {"accept-patch": "..."}`

