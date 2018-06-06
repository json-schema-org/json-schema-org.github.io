---
layout: page
title: Building a product schema
---

Let's pretend we're interacting with a JSON based product catalog. This catalog has a product which has:

* An identifier: `productId`
* A product name: `name`
* A selling cost for the consumer: `price`
* An optional set of tags: `tags`.

For example:

```json
{
  "productId": 1,
  "name": "A green door",
  "price": 12.50,
  "tags": [ "home", "green" ]
}
```

While generally straightforward, the example leaves some open questions. For example, one may ask:

* What is `productId`?
* Is name `required`?
* Can the `price` be zero (0)?
* Are all of the `tags` string values?

When you're talking about a data format, you want to have metadata about what keys mean, including the valid inputs for those keys. **JSON Schema** is a proposed IETF standard how to answer those questions for data.

## Starting the schema

To start a schema definition, let's begin with a basic JSON schema.

We start with four properties called **keywords** which are expressed as [JSON](https://www.json.org/) keys.

> Yes. the standard uses a JSON data document to describe data documents, most often that are also JSON data documents but could be in any number of other content types like `text/xml`.

* The [`$schema`](http://json-schema.org/latest/json-schema-core.html#rfc.section.7) keyword states that this schema is written according to the a specific draft of the standard and used for a variety of reasons, primarily version control.
* The [`$id`](http://json-schema.org/latest/json-schema-core.html#rfc.section.8.2) keyword defines a URI for the schema, and the base URI that other URI references within the schema are resolved against.
* The [`title`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.10.1) and [`description`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.10.1) annotation keywords are descriptive only. They do not add constraints to the data being validated. The intent of the schema is stated with these two keywords.
* The [`type`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.1.1) validation keyword defines the first constraint on our JSON data and in this case it has to be a JSON Object.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product",
  "description": "A product in the catalog",
  "type": "object"
}
```

## Defining the properties

Next let's answer our previous questions about this API, starting with `productId`.

### What is productId?

`productId` is a numeric value that uniquely identifies a product. Since this is the canonical identifier for a product, it doesn't make sense to have a product without one, so it is required.

In JSON Schema terms, we update our schema to add:

* The [`properties`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.4) keyword.
* The `productId` key.
  * `description` and `type` keywords for `productId`.
* The [`required`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.3) keyword listing `productId`.


```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product",
  "description": "A product from Acme's catalog",
  "type": "object",
  "properties": {
    "productId": {
      "description": "The unique identifier for a product",
      "type": "integer"
    }
  },
  "required": [ "productId" ]
}
```

### Is name required?

Yes.

* `name` is a string value that describes a product. Since there isn't much to a product without a name it also is required.
* Since the `required` keyword is an array of strings we can note multiple keys as required, now including `name`.

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product",
  "description": "A product from Acme's catalog",
  "type": "object",
  "properties": {
    "productId": {
      "description": "The unique identifier for a product",
      "type": "integer"
    },
    "name": {
      "description": "Name of the product",
      "type": "string"
    }
  },
  "required": [ "productId", "name" ]
}
```

### Can price be zero (0)?

No. According to the store owner there are no free products. ;)

* We need to specify this using the [`exclusiveMinimum` validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.5).
  * If we wanted to include zero as a valid price we would have specified the [`minimum` validation keyword](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.4).

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product",
  "description": "A product from Acme's catalog",
  "type": "object",
  "properties": {
    "productId": {
      "description": "The unique identifier for a product",
      "type": "integer"
    },
    "name": {
      "description": "Name of the product",
      "type": "string"
    },
    "price": {
      "description": "The price of the product,",
      "type": "number",
      "exclusiveMinimum": 0
    }
  },
  "required": ["productId", "name", "price"]
}
```

### Are all tags strings?

Finally, we come to the *tags* property. Unlike the previous properties, tags have many values, and is represented as a JSON array. If, according to Acme's docs, all tags must be strings, but you aren't required to specify tags; we would simply leave *tags* out of the list of required properties.

However, Acme's docs add two constraints:

-   there must be at least one tag,
-   all tags must be unique.

The first constraint can be added with *minItems*, and the second one by specifying *uniqueItems* as being true:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product",
  "description": "A product from Acme's catalog",
  "type": "object",
  "properties": {
    "productId": {
      "description": "The unique identifier for a product",
      "type": "integer"
    },
    "name": {
      "description": "Name of the product",
      "type": "string"
    },
    "price": {
      "type": "number",
      "exclusiveMinimum": 0
    },
    "tags": {
      "type": "array",
      "items": {
        "type": "string"
      },
      "minItems": 1,
      "uniqueItems": true
    }
  },
  "required": ["productId", "name", "price"]
}
```

## Summary

The above example is by no means definitive of all the types of data JSON schema can define. For more definitive information see the [full standard draft](#definitions).

As a final example, here's a spec for an array of products, with the products having 2 new properties. The first is a *dimensions* property for the size of the product, and the second is a *warehouseLocation* field for where the warehouse that stores them is geographically located.

And also, since JSON Schema defines a reference schema for a geographic location, instead of coming up with our own, we'll reference the [canonical one](http://json-schema.org/geo).

### Set of products:

```json
[
  {
    "productId": 2,
    "name": "An ice sculpture",
    "price": 12.50,
    "tags": ["cold", "ice"],
    "dimensions": {
      "length": 7.0,
      "width": 12.0,
      "height": 9.5
    },
    "warehouseLocation": {
      "latitude": -78.75,
      "longitude": 20.4
    }
  },
  {
    "productId": 3,
    "name": "A blue mouse",
    "price": 25.50,
    "dimensions": {
      "length": 3.1,
      "width": 1.0,
      "height": 1.0
    },
    "warehouseLocation": {
      "latitude": 54.4,
      "longitude": -32.7
    }
  }
]
```

### Set of products schema:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/product.schema.json",
  "title": "Product set",
  "type": "array",
  "items": {
    "title": "Product",
    "type": "object",
    "properties": {
      "productId": {
        "description": "The unique identifier for a product",
        "type": "number"
      },
      "name": {
        "type": "string"
      },
      "price": {
        "type": "number",
        "exclusiveMinimum": 0
      },
      "tags": {
        "type": "array",
        "items": {
          "type": "string"
        },
        "minItems": 1,
        "uniqueItems": true
      },
      "dimensions": {
        "type": "object",
        "properties": {
          "length": {"type": "number"},
          "width": {"type": "number"},
          "height": {"type": "number"}
        },
        "required": ["length", "width", "height"]
      },
      "warehouseLocation": {
        "description": "Coordinates of the warehouse with the product",
        "$ref": "http://json-schema.org/geo"
      }
    },
    "required": ["productId", "name", "price"]
  }
}
```
