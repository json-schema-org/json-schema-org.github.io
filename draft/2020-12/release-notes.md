---
title: JSON Schema 2020-12 Release Notes
layout: page
---
The previous draft (2019-09) introduced a lot of new concepts including
`$recursiveRef`/`$recursiveAnchor`, `unevaluatedProperties`/`unevaluatedItems`,
vocabularies, and more. Since then, these new features have seen multiple
implementations and usage in real schemas. This draft is mostly dedicated to
changes related to applying the lessons we've learned about implementing and
using these new features in the wild.

This document attempts to put information most useful to schema authors toward
the top and information for implementation authors toward the bottom.

## Changes to items and additionalItems
The keywords used for defining arrays and tuples have been redesigned to help
lower the learning curve for JSON Schema. Since the `items` keyword was used for
both types, we would often see people mistakenly defining a tuple when they
meant to define an array and not understand why only the first item in the array
was validating.

The `items` and `additionalItems` keywords have been replaced with `prefixItems`
and `items` where `prefixItems` has the same functionality as the
array-of-schemas for of the old `items` and the new `items` keyword has the same
functionality as the old `additionalItems` keyword.

Although the meaning of `items` has changed, the syntax for defining arrays
remains the same. Only the syntax for defining tuples has changed. The idea is
that an array has items (`items`) and optionally has some positionally defined
items that come before the normal items (`prefixItems`).

Here are some examples to illustrate the changes.

### Open tuple
<table>
  <tr>
    <th>Draft 2019-09</th>
    <th>Draft 2020-12</th>
  </tr>
  <tr>
    <td>
      <pre>{
  "items": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ]
}</pre>
    </td>
    <td>
      <pre>{
  "prefixItems": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ]
}</pre>
    </td>
  </tr>
</table>

### Closed tuple
<table>
  <tr>
    <th>Draft 2019-09</th>
    <th>Draft 2020-12</th>
  </tr>
  <tr>
    <td>
      <pre>{
  "items": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ],
  "additionalItems": false
}</pre>
    </td>
    <td>
      <pre>{
  "prefixItems": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ],
  "items": false
}</pre>
    </td>
  </tr>
</table>

### Tuple with constrained additional items
<table>
  <tr>
    <th>Draft 2019-09</th>
    <th>Draft 2020-12</th>
  </tr>
  <tr>
    <td>
      <pre>{
  "items": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ],
  "additionalItems": { "$ref": "#/$defs/baz" }
}</pre>
    </td>
    <td>
      <pre>{
  "prefixItems": [
    { "$ref": "#/$defs/foo" },
    { "$ref": "#/$defs/bar" }
  ],
  "items": { "$ref": "#/$defs/baz" }
}</pre>
    </td>
  </tr>
</table>

## $dynamicRef and $dynamicAnchor
The `$recursiveRef` and `$recursiveAnchor` keywords were replaced by the more
powerful `$dynamicRef` and `$dynamicAnchor` keywords. `$recursiveRef` and
`$recursiveAnchor` were introduced in the previous draft to solve the problem of
extending recursive schemas. As the "recursive" keywords got some use and we
understood them better, we discovered how we could generalize them to solve even
more types of problems. The name change reflects that these keywords are useful
for more than just extending recursive schemas.

A `$dynamicAnchor` can be thought of like a normal `$anchor` except that it can
be referenced across schemas rather than just in the schema where it was
defined. You can think of the old `$recursiveAnchor` as working the same way
except that it only allowed you to create one anchor per schema, it had to be at
the root of the schema, and the anchor name is always empty.

`$dynamicRef` works the same as the old `$recursiveRef` except that fragments
are no longer empty (`"$dynamicRef": "#my-anchor"` instead of `"$recursiveRef":
"#"`) and non-fragment-only URIs are allowed. When a `$dynamicRef` contains a
non-fragment-only URI-Reference, the schema the URI-Reference resolves to is
used as the starting point for dynamic resolution.

Here's how you would covert a schema using `$recursiveRef` to use `$dynamicRef`.

<table>
  <tr>
    <th>Draft 2019-09</th>
    <th>Draft 2020-12</th>
  </tr>
  <tr>
    <td><pre>// tree schema, extensible
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://example.com/tree",
  "$recursiveAnchor": true,

  "type": "object",
  "properties": {
    "data": true,
    "children": {
      "type": "array",
      "items": { "$recursiveRef": "#" }
    }
  }
}

// strict-tree schema, guards against misspelled properties
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://example.com/strict-tree",
  "$recursiveAnchor": true,

  "$ref": "tree",
  "unevaluatedProperties": false
}</pre></td>
    <td><pre>// tree schema, extensible
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/tree",
  "$dynamicAnchor": "node",

  "type": "object",
  "properties": {
    "data": true,
    "children": {
      "type": "array",
      "items": { "$dynamicRef": "#node" }
    }
  }
}

// strict-tree schema, guards against misspelled properties
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://example.com/strict-tree",
  "$dynamicAnchor": "node",

  "$ref": "tree",
  "unevaluatedProperties": false
}</pre></td>
  </tr>
</table>

## contains and unevaluatedItems
In the previous draft, it wasn't specified how or if the `contains` keyword
affects the `unevaluatedItems` keyword. This draft specifies that any item in an
array that passes validation of the `contains` schema is considered "evaluated".

This allows you to use `contains` to express some constraints more cleanly than
you could in previous drafts. This example show how you can express an array
that has some item matching one schema and everything else matching another
schema.

<table>
  <tr>
    <th>Draft 2019-09</th>
    <th>Draft 2020-12</th>
  </tr>
  <tr>
    <td><pre>{
  "type": "array",
  "contains": { "type": "string" },
  "items": {
    "anyOf": [
      { "type": "string" },
      { "type": "number" }
    ]
  }
}</pre></td>
    <td><pre>{
  "type": "array",
  "contains": { "type": "string" },
  "unevaluatedItems": { "type": "number" }
}





</pre></td>
  </tr>
</table>

Unfortunately, this change means you may not be able to use `contains` in some
situations you did before. Consider this draft 2019-09 schema describing a tuple
of two strings where one of the two must be three or more characters long and
any additional items are not allowed.

```json
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "type": "array",
  "items": [{ "type": "string" }, { "type": "string" }],
  "contains": { "type": "string", "minLength": 3 },
  "unevaluatedItems": false
}
```

Given this schema, the instance `["a", "b", "ccc"]` will fail because `"ccc"` is
considered unevaluated and fails the `unevaluatedItems` keyword. Now let's
naively convert that example to a draft 2020-12 schema.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "array",
  "prefixItems": [{ "type": "string" }, { "type": "string" }],
  "contains": { "type": "string", "minLength": 3 },
  "unevaluatedItems": false
}
```

Given this schema, the instance `["a", "b", "ccc"]` will pass because `"ccc"` is
considered evaluated and doesn't not apply to the `unevaluatedItems` keyword. To
fix this problem we can use the same boolean algebra transformation we used to
use before we had the `contains` keyword.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "type": "array",
  "prefixItems": [{ "type": "string" }, { "type": "string" }],
  "not": {
    "items": {
      "not": { "type": "string", "minLength": 3 }
    }
  },
  "unevaluatedItems": false
}
```

Given this schema, the instance `["a", "b", "ccc"]` will fail because `"ccc"` is
considered unevaluated and fails the `unevaluatedItems` keyword like it did in
previous drafts.

## Regular Expressions
Regular expressions are now required to support unicode characters. Previously,
this was unspecified and implementations may or may not support this unicode in
regular expressions.

## Media Type Changes
JSON Schema defines two media types, `application/schema+json` and
`application/schema-instance+json`. This draft drops support for the `schema`
media type parameter. It's caused a lot of confusion and disagreement. Since we
haven't seen any evidence of anyone actually using it, it was decided to remove
it for now.

## Embedded Schemas and Bundling
In Draft 2019-09, the meaning of `$id` in a sub-schema changed from indicating a
base URI change within the current schema to indicating an embedded schema
independent of the parent schema. A schema that contains one or more embedded
schemas is called a "Compound Schema Document". This draft introduces guidance
on how bundlers should embedded schemas to create Compound Schema Documents.

If you reference an external schema, that schema can declare its own `$schema`
and that may be different than the `$schema` of the referencing schema.
Implementations need to be prepared to switch processing modes or throw an
error if they don't support the `$schema` of the referenced schema. Embedded
schemas work exactly the same way. They may declare a `$schema` that is not the
same as the parent schema and implementations need to be prepared to handle the
`$schema` change appropriately.

A notable consequence of embedded schemas having a different `$schema` than its
parent is that implementations can't validate Compound Schema Documents directly
against the meta-schema. The Compound Schema Document needs to be decomposed and
each Schema Resource needs to be validated individually against the appropriate
meta-schema for that schema.

This draft introduces official guidance on how to use embedded schemas to
bundle schemas into a Compound Schema Document. The approach is designed to not
have to modify schemas (other than adding to `$defs`) so that output results
remain as similar as possible whether you are validating the bundled schema or
following external references. Here's an example of a customer schema with
external references that we want to bundle.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12",
  "$id": "https://example.com/schema/customer",

  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "phone": { "$ref": "/schema/common#/$defs/phone" },
    "address": { "$ref": "/schema/address" }
  }
}
```

```json
{
  "$schema": "https://json-schema.org/draft/2020-12",
  "$id": "https://example.com/schema/address",

  "type": "object",
  "properties": {
    "address": { "type": "string" },
    "city": { "type": "string" },
    "postalCode": { "type": "/schema/common#/$defs/usaPostalCode" },
    "state": { "type": "/$defs/states" }
  },

  "$defs": {
    "states": {
      "enum": [...]
    }
  }
}
```

```json
{
  "$schema": "https://json-schema.org/draft/2019-09",
  "$id": "https://example.com/schema/common",

  "$defs": {
    "phone": {
      "type": "string",
      "pattern": "^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$"
    },
    "usaPostalCode": {
      "type": "string",
      "pattern": "^[0-9]{5}(?:-[0-9]{4})?$"
    },
    "unsignedInt": {
      "type": "integer",
      "minimum": 0
    }
  }
}
```

To bundle these schemas, we simply add each of the referenced schemas as
embedded schemas using `$defs`. Here's what the bundled schema would look like.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12",
  "$id": "https://example.com/schema/customer",

  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "phone": { "$ref": "/schema/common#/$defs/phone" },
    "address": { "$ref": "/schema/address" }
  }

  "$defs": {
    "https://example.com/schema/address": {
      "$id": "https://example.com/schema/address",

      "type": "object",
      "properties": {
        "address": { "type": "string" },
        "city": { "type": "string" },
        "postalCode": { "type": "/schema/common#/$defs/usaPostalCode" },
        "state": { "type": "#/$defs/states" }
      },

      "$defs": {
        "states": {
          "enum": [...]
        }
      }
    },
    "https://example.com/schema/common": {
      "$schema": "https://json-schema.org/draft/2019-09",
      "$id": "https://example.com/schema/common",

      "$defs": {
        "phone": {
          "type": "string",
          "pattern": "^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$"
        },
        "usaPostalCode": {
          "type": "string",
          "pattern": "^[0-9]{5}(?:-[0-9]{4})?$"
        },
        "unsignedInt": {
          "type": "integer",
          "minimum": 0
        }
      }
    }
  }
}
```

Here are a few things you might notice from this example.

1. No `$ref`s were modified. Even local references are unchanged.
2. `https://example.com/schema/common#/$defs/unsignedInt` got pulled in with the
common schema even though it isn't used. It's allowed to trim out the extra
definitions, but not necessary.
3. `https://example.com/schema/address` doesn't declare a `$schema`. Because it
uses the same `$schema` as `https://example.com/schema/customer`, it can skip
that declaration and use the `$schema` from the schema it's embedded in.
4. `https://example.com/schema/common` uses a different `$schema` than the
document it's embedded in. That's allowed.
5. Definitions from `https://example.com/schema/common` are used in both of the
other schemas and only needs to be included once. It isn't necessary for
bundlers to embed a schema inside another embedded schema.

## Annotations
Implementations that collect annotations should now include annotations for
unknown keywords in the "verbose" output format. The annotation value for an
unknown keyword is the keyword's value.

## Vocabulary Changes
The `unevaluatedProperties` and `unevaluatedItems` keywords have been moved from
the applicator vocabulary to their own designated vocabulary which is required
in the default meta-schema. In Draft 2019-09, these keywords were expected to
throw an error if not implemented. This was a special-case behavior of the
applicator vocabulary. Moving the "unevaluated" keywords into their own
vocabulary allows us to remove that special-case and also allowing for dialects
to be constructed that don't require these keywords.

The format vocabulary was broken into two separate vocabularies. The
"format-annotation" vocabulary treats the `format` keyword as an annotation and
the "format-assertion" vocabulary treats the `format` keyword as an assertion.
The "format-annotation" vocabulary is used in the default meta-schema and is
required. In Draft 2019-09, `format` should be evaluated as an annotation by
default and implementations could provide configuration to change the behavior
to evaluate `format` as an assertion. The separate vocabularies allow for
removing the special configuration requirements and just use the vocabulary
system to express which behavior should be used.
