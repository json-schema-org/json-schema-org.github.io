---
title: JSON Schema 2020-12 Release Notes
layout: page
---
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
