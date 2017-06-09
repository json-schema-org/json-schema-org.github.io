---
layout: page
title: Building a product schema
---

Let's pretend we're interacting with a JSON based product catalog. This catalog has a product which has an *id*, a *name*, a *price*, and an optional set of *tags*.

### Example JSON data for a product API

An example product in this API is:

```json
{% include example1/instance.json %}
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
{% include example1/schema1.json %}
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
{% include example1/schema2.json %}
```

### Is name required?

*name* is a string value that describes a product. Since there isn't much to a product without a name, it also is required. Adding this gives us the schema:

```json
{% include example1/schema3.json %}
```

### Can price be 0?

According to Acme's docs, there are no free products. In JSON schema a number can have a minimum. By default this minimum is inclusive, so we need to specify *exclusiveMinimum*. Therefore we can update our schema with *price*:

```json
{% include example1/schema4.json %}
```

### Are all tags strings?

Finally, we come to the *tags* property. Unlike the previous properties, tags have many values, and is represented as a JSON array. According to Acme's docs, all tags must be strings, but you aren't required to specify tags. We simply leave *tags* out of the list of required properties.

However, Acme's docs add two constraints:

-   there must be at least one tag,
-   all tags must be unique.

The first constraint can be added with *minItems*, and the second one by specifying *uniqueItems* as being true:

```json
{% include example1/schema5.json %}
```

Summary
-------

The above example is by no means definitive of all the types of data JSON schema can define. For more definitive information see the [full standard draft](#definitions).

As a final example, here's a spec for an array of products, with the products having 2 new properties. The first is a *dimensions* property for the size of the product, and the second is a *warehouseLocation* field for where the warehouse that stores them is geographically located.

And also, since JSON Schema defines a reference schema for a geographic location, instead of coming up with our own, we'll reference the [canonical one](http://json-schema.org/geo).

### Set of products:

```json
{% include example1/set_instance.json %}
```

### Set of products schema:

```json
{% include example1/set_schema.json %}
```
