---
title: JSON Schema Draft-06 Release Notes
layout: page
redirect_from: "/draft-06/json-schema-migration-faq.html"
permalink: /draft-06/json-schema-release-notes.html
---

Release notes for migrating from zyp-04 and fge-00 (draft-04) to wright-01 (draft-06).

<span style="color: red; font-size: 200%">**NOTE**: draft-07 has been released</span>

_Note that draft-07 core and validation are backwards-compatible with draft-06.
For more information, see that draft's [migration notes](../draft-07/json-schema-release-notes.html)._

* TOC
{:toc}

### Q: What are the changes between draft-04 and draft-06?

#### Backwards-incompatible changes

keyword | change | consequence
---- | ---- | ----
`"id"` | replaced by `"$id"` | no longer easily confused with instance properties named `"id"`
`"$id"` | replaces `"id"` | behavior is identical, `$` prefix matches the other two core keywords
`"$ref"` | only allowed where a schema is expected | it is now possible to describe instance properties named `"$ref"`
`"exclusiveMinimum"` and `"exclusiveMaximum"` | changed from a boolean to a number to be consistent with the principle of keyword independence | wherever one of these would be true before, change the value to the corresponding `"minimum"` or `"maximum"` value and remove the `"minimum"`/`"maximum"` keyword
`"type"` | definition of `"integer"` | in draft-04, `"integer"` is listed as a primitive type and defined as "a JSON number without a fraction or exponent part"; in draft-06, `"integer"` is not considered a primitive type and is only defined in the section for keyword `"type"` as "any number with a zero fractional part"; `1.0` is thus not a valid `"integer"` type in draft-04 and earlier, but is a valid `"integer"` type in draft-06 and later; note that both drafts say that integers SHOULD be encoded in JSON without fractional parts  

#### Additions and backwards-compatible changes

keyword | change | consequence
---- | ---- | ----
booleans as schemas | allowable anywhere, not just `"additionalProperties"` and `"additionalItems"` | `true` is equivalent to `{}`, `false` is equivalent to `{"not": {}}`, but the intent is more clear and implementations can optimize these cases more easily
`"propertyNames"` | added | takes a schema which validates the *names* of all properties rather than their values
`"contains"` | added | array keyword that passes validation if its schema validates at least one array item
`"const"` | added | more readible form of a one-element `"enum"`
`"required"` | allows an empty array | an empty array indicates that no properties are required
`"dependencies"` | allows an empty array for property dependencies | an empty array indicates that there are no dependencies for the given property
`"format": "uri-reference"` | added | allows relative URI references per RFC 3986; see the section below about `"uri"` as a format
`"format": "uri-template"` | added | indicates an RFC 6570 conforming URI Template value, as is used in JSON Hyper-Schema for `"href"`
`"format": "json-pointer"` | added | indicates a JSON Pointer value such as `/foo/bar`; do _not_ use this for JSON Pointer URI fragments such as `#/foo/bar`: the proper format for those is `"uri-reference"`
`"examples"` | added | array of examples with no validation effect; the value of `"default"` is usable as an example without repeating it under this keyword

#### Formats: `"uri"` vs `"uri-reference"`

While not technically a change, the behavior of the `"uri"` format was not clearly explained and often implemented and used incorrectly (including in the draft-04 meta-schema).

`"uri"` should only be used when an absolute URI (starting with a scheme) is required.

When a relative path, fragment, or any other style of URI Reference (per RFC 3986) is allowable, use `"uri-reference"`.

Implementations offering a translation from draft-04 to draft-06 may want to offer an option to convert `"uri"` formats to `"uri-reference"`, although any such option should be disabled by default for strict conformance.

### Q: What happened to draft-05?

The draft-05 core and validation specifications were intended to be more clear and readable rewrites of draft-04, to give us a strong base for draft-06 changes.  Implementors should **not** implement or advertise support for "draft-05".

Implementations that supported "draft-05" by implementing proposals from right after the publication of draft-04 should either remove that support or give it a different name to avoid confusion.

### Q: What happened to all the discussions around re-using schemas with `"additionalProperties"`?

There are several competing proposals for making the re-use of schemas that set `"additionalProperties"` to something other than `true`.  Most people specifically care about the case where it is `false`, but the same behavior occurs with any non-`true` value.

[All of the proposals in this area](https://github.com/json-schema-org/json-schema-spec/issues?q=is%3Aissue+is%3Aopen+label%3A%22re-use+%2F+addlProps%22) will be the focus of [draft-08](https://github.com/json-schema-org/json-schema-spec/milestone/6).  While we made progress in eliminating some options during draft-07, the remaining divisions are deep enough to warrant making it the primary focus of a draft ([draft-07](https://github.com/json-schema-org/json-schema-spec/milestone/5)'s primary focus is Hyper-Schema).

The difficulty is that if you attempt to do this:

```json
{
    "type": "object",
    "allOf": [
        {"$ref": "#/definitions/foo"},
        {"$ref": "#/definitions/bar"}
    ],
    "definitions": {
        "foo": {
            "properties": {
                "foo": {"type": "string"}
            },
            "additionalProperties": false
        },
        "bar": {
            "properties": {
                "bar": {"type": "number"}
            },
            "additionalProperties": false
        }
    }
}
```

Validation will always fail for any non-empty object instance.  `"additionalProperties"` only knows about immediately adjacent `"properties"` and `"patternProperties"`, in order to ensure that each subschema means the same thing whether it is being used with another subschema or on its own.

So in this example, if the instance has a "bar" property, it will fail the first subschema because "bar" is not "foo".  If it has a "foo" property, it will fail the second subschema because "foo" is not "bar".  And any other property will fail both schemas.

A workaround is available with the new `"propertyNames"` keyword:

```json
{
    "type": "object",
    "allOf": [
        {"$ref": "#/definitions/foo"},
        {"$ref": "#/definitions/bar"}
    ],
    "anyOf": [
        {"$ref": "#/definitions/fooNames"},
        {"$ref": "#/definitions/barNames"}
    ],
    "definitions": {
        "foo": {
            "properties": {
                "foo": {"type": "string"}
            }
        },
        "fooNames": {
            "propertyNames": {"enum": ["foo"]}
        },
        "bar": {
            "properties": {
                "bar": {"type": "number"}
            }
        },
        "barNames": {
            "propertyNames": {"enum": ["bar"]}
        }
    }
}
```

This will allow an object with either "foo" or "bar" or both, but will fail validation if any other property is present.  The `"allOf"` ensures that "foo" and "bar" will each be validated correctly if present, while the `"anyOf"` allows for properties with names in either allowed set, but forbids properties that are not listed in at least one set.

It does require duplicating the names, and the awkward use of both an `"allOf"` and `"anyOf"`, but it is less repetition than other options, and can be re-used fairly robustly even if the "foo" and "bar" schemas are in separate files managed by a different person or organization.
