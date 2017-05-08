---
layout: page
title: Building a product schema
---

Let's pretend we're interacting with a JSON based product catalog. This catalog has a product which has an *id*, a *name*, a *price*, and an optional set of *tags*.

### Example JSON data for a product API

An example product in this API is:

```json
{
    "id": 1,
    "name": "A green door",
    "price": 12.50,
    "tags": ["home", "green"]
}
```

While generally straightforward, that example leaves some open questions. For example, one may ask:

-   What is id?
-   Is name required?
-   Can price be 0?
-   Are all tags strings?

When you're talking about a data format, you want to have metadata about what fields mean, and what valid inputs for those fields are. JSON schema is a specification for standardizing how to answer those questions for JSON data.

Starting the schema
-------------------

To start a schema definition, let's begin with a basic JSON schema:

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object"
}
```

The above schema has four properties called *keywords*. The *title* and *description* keywords are descriptive only, in that they do not add constraints to the data being validated. The intent of the schema is stated with these two keywords (that is, this schema describes a product).

The *type* keyword defines the first constraint on our JSON data: it has to be a JSON Object.

Finally, the *$schema* keyword states that this schema is written according to the draft v4 specification.

Defining the properties
-----------------------

Next let's answer our previous questions about this API, starting with id.

### What is id?

*id* is a numeric value that uniquely identifies a product. Since this is the canonical identifier for a product, it doesn't make sense to have a product without one, so it is required.

In JSON Schema terms, we can update our schema to:

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "integer"
        }
    },
    "required": ["id"]
}
```

### Is name required?

*name* is a string value that describes a product. Since there isn't much to a product without a name, it also is required. Adding this gives us the schema:

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "integer"
        },
        "name": {
            "description": "Name of the product",
            "type": "string"
        }
    },
    "required": ["id", "name"]
}
```

### Can price be 0?

According to Acme's docs, there are no free products. In JSON schema a number can have a minimum. By default this minimum is inclusive, so we need to specify *exclusiveMinimum*. Therefore we can update our schema with *price*:

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "integer"
        },
        "name": {
            "description": "Name of the product",
            "type": "string"
        },
        "price": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
        }
    },
    "required": ["id", "name", "price"]
}
```

### Are all tags strings?

Finally, we come to the *tags* property. Unlike the previous properties, tags have many values, and is represented as a JSON array. According to Acme's docs, all tags must be strings, but you aren't required to specify tags. We simply leave *tags* out of the list of required properties.

However, Acme's docs add two constraints:

-   there must be at least one tag,
-   all tags must be unique.

The first constraint can be added with *minItems*, and the second one by specifying *uniqueItems* as being true:

```json
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product",
    "description": "A product from Acme's catalog",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a product",
            "type": "integer"
        },
        "name": {
            "description": "Name of the product",
            "type": "string"
        },
        "price": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
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
    "required": ["id", "name", "price"]
}
```

Summary
-------

The above example is by no means definitive of all the types of data JSON schema can define. For more definitive information see the [full standard draft](#definitions).

As a final example, here's a spec for an array of products, with the products having 2 new properties. The first is a *dimensions* property for the size of the product, and the second is a *warehouseLocation* field for where the warehouse that stores them is geographically located.

And also, since JSON Schema defines a reference schema for a geographic location, instead of coming up with our own, we'll reference the [canonical one](http://json-schema.org/geo).

### Set of products:

```json
[
    {
        "id": 2,
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
        "id": 3,
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
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product set",
    "type": "array",
    "items": {
        "title": "Product",
        "type": "object",
        "properties": {
            "id": {
                "description": "The unique identifier for a product",
                "type": "number"
            },
            "name": {
                "type": "string"
            },
            "price": {
                "type": "number",
                "minimum": 0,
                "exclusiveMinimum": true
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
        "required": ["id", "name", "price"]
    }
}
```

