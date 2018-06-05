---
layout: page
title: Examples
---

## Basic

This example provides a typical minimum you are going to see in JSON Schema. It contains:

* [`$id` keyword](http://json-schema.org/latest/json-schema-core.html#rfc.section.8.2)
* [`$schema` keyword](http://json-schema.org/latest/json-schema-core.html#rfc.section.7)
* [`title` link target attribute](http://json-schema.org/latest/json-schema-hypermedia.html#rfc.section.6.5.1)
* [`type` instance data model](http://json-schema.org/latest/json-schema-core.html#rfc.section.4.2.1)
* [`properties` object validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.4)
* Three keys: `firstName`, `lastName` and `age` each with their own:
  * [`description` annotation](http://json-schema.org/latest/json-schema-validation.html#rfc.section.10.1)
  * `type` instance data model (see above).
* [`minimum` validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.4) on the `age` key.

```json
{
  "$id": "https://example.com/person.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Person",
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string",
      "description": "The person's first name."
    },
    "lastName": {
      "type": "string",
      "description": "The person's last name."
    },
    "age": {
      "description": "Age in years which must be equal to or greater than zero.",
      "type": "integer",
      "minimum": 0
    }
  }
}
```

**Data**

```json
{
  "firstName": "John",
  "lastName": "Doe",
  "age": 21
}
```

## Describing geographical coordinates.

This example introduces:

* [`required` object validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.3)
* [`minimum` validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.4)
* [`maximum` validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.2)


```json
{
  "id": "https://example.com/geographical-location.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Longitude and Latitude Values",
  "description": "A geographical coordinate.",
  "required": [ "latitude", "longitude" ],
  "type": "object",
  "properties": {
    "latitude": {
      "type": "number",
      "minimum": -90,
      "maximum": 90
    },
    "longitude": {
      "type": "number",
      "minimum": -180,
      "maximum": 180
    }
  }
}
```

**Data**

```json
{
  "latitude": 48.858093,
  "longitude": 2.294694
}
```

## Arrays of things

Arrays are fundamental structures in JSON -- here we demonstrate a couple of ways they can be described:

* An array of string values.
* An array of objects.

We also introduce the following with this example:

* [`definitions` keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.9)
* [`$ref` keyword](http://json-schema.org/latest/json-schema-core.html#rfc.section.8.3)

```json
{
  "id": "https://example.com/arrays.schema.json",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "description": "A representation of a person, company, organization, or place",
  "type": "object",
  "properties": {
    "fruits": {
      "type": "array",
      "items": {
        "type": "string"
      }
    },
    "vegetables": {
      "type": "array",
      "items": { "$ref": "#/definitions/veggie" }
    }
  },
  "definitions": {
    "veggie": {
      "type": "object",
      "required": [ "veggieName", "veggieLike" ],
      "properties": {
        "veggieName": {
          "type": "string",
          "description": "The name of the vegetable."
        },
        "veggieLike": {
          "type": "boolean",
          "description": "Do I like this vegetable?"
        }
      }
    }
  }
}
```

**Data**

```json
{
  "fruits": [ "apple", "orange", "pear" ],
  "vegetables": [
    {
      "veggieName": "potato",
      "veggieLike": true
    },
    {
      "veggieName": "broccoli",
      "veggieLike": false
    }
  ]
}
```

## Walkthroughs
------------

The two examples below are step-by-step guides into building a schema:

-   [a simple example](example1.md) which covers a classical product catalog description.
-   [a more advanced example](example2.md), using JSON Schema to describe filesystem entries in a Unix-like /etc/fstab file.

The [Space Telescope Science Institute](http://www.stsci.edu/) has also published a [guide aimed at schema authors](http://spacetelescope.github.io/understanding-json-schema/).
