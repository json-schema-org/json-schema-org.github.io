## FAQ for migrating from zyp-04 and fge-00 (draft-04) to wright-01 (draft-06)

* [**Q: What are the changes between draft-04 and draft-06?**](#changes)
* [**Q: What happened to all of the discussions around re-using schemas with `"additionalProperties"`?](#addlprop)
* [**Q: What are key issues under consideraton for draft-07?**](#consideration)

## _A note on draft naming and numbering:_

IETF Internet-Drafts (I-Ds) are named with the editor's name and a sequential number which resets with each new editor.  Meta-schemas are numbered sequentially.  Additionally, drafts 00-03 used one document for all three current specs.  So, for core and validation, the correspondences are:

Core/Validation IETF Drafts | Meta-Schema URI
---- | ----
[draft-zyp-json-schema-00](https://tools.ietf.org/html/draft-zyp-json-schema-00) | [http://json-schema.org/draft-00/schema#](http://json-schema.org/draft-00/hyper-schema#)
[draft-zyp-json-schema-01](https://tools.ietf.org/html/draft-zyp-json-schema-01) | [http://json-schema.org/draft-01/schema#](http://json-schema.org/draft-01/hyper-schema#)
[draft-zyp-json-schema-02](https://tools.ietf.org/html/draft-zyp-json-schema-02) | [http://json-schema.org/draft-02/schema#](http://json-schema.org/draft-02/hyper-schema#)
[draft-zyp-json-schema-03](https://tools.ietf.org/html/draft-zyp-json-schema-03) | [http://json-schema.org/draft-03/schema#](http://json-schema.org/draft-03/hyper-schema#)
[draft-zyp-json-schema-04](https://tools.ietf.org/html/draft-zyp-json-schema-04) and [draft-fge-json-schema-validation-00](https://tools.ietf.org/html/draft-fge-json-schema-validation-00) | [http://json-schema.org/draft-04/hyper-schema#](http://json-schema.org/draft-04/schema#)
[draft-wright-json-schema-00](https://tools.ietf.org/html/draft-wright-json-schema-00) and [draft-wright-json-schema-validation-00](https://tools.ietf.org/html/draft-wright-json-schema-validation-00) | draft-05; [not published](https://github.com/json-schema-org/json-schema-spec/wiki/Specification-Links)
[draft-wright-json-schema-01](https://tools.ietf.org/html/draft-wright-json-schema-01) and [draft-wright-json-schema-validation-01](https://tools.ietf.org/html/draft-wright-json-schema-validation-01) | [http://json-schema.org/draft-06/schema#](http://json-schema.org/draft-06/hyper-schema#)

Most people find it easier to remember the sequential meta-schema numbers, so this FAQ will use those, including draft-05 for draft-wright-json-schema-hyperschema-00.

## Questions and Answers

<a id="changes"></a>
### Q: What are the changes between draft-04 and draft-06?

#### Backwards-incompatible changes

keyword | change | consequence
---- | ---- | ----
`"id"` | replaced by `"$id"` | no longer easily confused with instance properties named `"id"`
`"$id"` | replaces `"id"` | behavior is identical, `$` prefix matches the other two core keywords
`"$ref"` | only allowed where a schema is expected | it is now possible to describe instance properties named `"$ref"`
`"exclusiveMinimum"` and `"exclusiveMaximum"` | changed from a boolean to a number to be consistent with the principle of keyword independence | wherever one of these would be true before, change the value to the corresponding `"minimum"` or `"maximum"` value and remove the `"minimum"`/`"maximum"` keyword

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

Implementations offering a translation from draft-04 to draft-06 may want to offer an option to convert `"uri"` formats to `"uri-reference"`, although any such option should be disable by default for strict conformance.

<a id="draft05"></a>
### Q: What happened to draft-05?

The draft-05 core and validation specifications were intended to be more clear and readible rewrites of draft-04, to give us a strong base for draft-06 changes.  Implementors should **not** implement or advertise support for "draft-05".

Implementations that supported "draft-05" by implementing proposals from right after the publication of draft-04 should either remove that support or give it a different name to avoid confusion.

<a id="addlprop"></a>
### Q: What happened to all of the discussions around re-using schemas with `"additionalProperties"`?

There are several competing proposals for making the re-use of schemas that set `"additionalProperties"` to something other than `true`.  Most people specifically care about the case where it is `false`, but the same behavior occurs with any non-`true` value.

The difficulty is that if you attempt to do this:

```JSON
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

validation will always fail for any non-empty object instance.  `"additionalProperties"` only knows about immediately adjacent `"properties"` and `"patternProperties"`, in order to ensure that each subschema means the same thing whether it is being used with another subschema or on its own.

So in this example, if the instance has a "bar" property, it will fail the first subschema because "bar" is not "foo".  If it has a "foo" property, it will fail the second subschema because "foo" is not "bar".  And any other property will fail both schemas.

A workaround is available with the new `"propertyNames"` keyword:

```JSON
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

_*TODO:* Link to all of the discussions about other use cases and proposed solutions._

<a id="consideration"></a>
### Q: What are key issues under consideraton for draft-07?

We are just starting to consider what to prioritize for the next draft.

There are only some fairly minor items to consider for the core specification, so we'd like to wrap that up and get it ready for submission to a working group.  The question of which link relation to use for connecting schemas to instances is the main one.

For validation, there are a number of competing proposals.  We will update this document as we get agreement on priorities.
