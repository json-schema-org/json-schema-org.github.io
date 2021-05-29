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
