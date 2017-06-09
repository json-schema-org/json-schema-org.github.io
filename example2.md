---
layout: page
title: Building a mount point schema
---

This example shows a possible JSON representation of a hypothetical machine's mount points as represented in an `/etc/fstab` file.

An entry in the fstab file can have many different forms. Here is a possible representation of a full fstab:

```json
{% include example2/instance.json %}
```

Not all constraints to an fstab file can be modeled using JSON Schema alone; however, it can already represent a good number of them. We will add constraints one after the other until we get to a satisfactory result.

Base schema
-----------

We will start with a base schema expressing the following constraints:

-   the list of entries is a JSON object;
-   the member names (or property names) of this object must all be valid, absolute paths;
-   there must be an entry for the root filesystem (ie, `/`).

We also want the schema to be regarded as a draft v4 schema, we must therefore specify *$schema*:

```json
{% include example2/schema1.json %}
```

Note how the valid paths constraint is enforced here:

-   we have a *properties* keyword with only a `/` entry;
-   we use *patternProperties* to match other property names via a regular expression (note that it does not match `/`);
-   as *additionalProperties* is false, it constrains object properties to be either `/` or to match the regular expression.

You will notice that the regular expression is explicitly anchored (with `^` and `$`): in JSON Schema, regular expressions (in *patternProperties* and in *pattern*) are not anchored by default.

For now, the schemas describing individual entries are empty: we will start describing the constraints in the following paragraphs, using another schema, which we will reference from the main schema when we are ready.

The entry schema - starting out
-------------------------------

Here again we will proceed step by step. We will start with the global structure of our schema, which will be as such:

```json
{% include example2/entry_schema1.json %}
```

You should already be familiar with some of the constraints:

-   an fstab entry must be an object (`"type": "object"`);
-   it must have one property with name *storage* (`"required": [ "storage" ]`);
-   the *storage* property must also be an object.

There are a couple of novelties:

-   you will notice the appearance of JSON References, via the *$ref* keyword; here, all references used are local to the schema, and the fragment part is a URI encoded JSON Pointer;
-   you will notice the appearance of an *id*: this is the URI of this resource; we assume here that this URI is the actual URI of this schema;
-   the *oneOf* keyword is new in draft v4; its value is an array of schemas, and an instance is valid if and only if it is valid against exactly one of these schemas;
-   finally, the *definitions* keyword is a standardized placeholder in which you can define inline subschemas to be used in a schema.

### The *fstype*, *options* and *readonly* properties

The entry schema - adding constraints
-------------------------------------

Let's now extend this skeleton to add constraints to these three properties. Note that none of them are required:

-   we will pretend that we only support `ext3`, `ext4` and `btrfs` as filesystem types;
-   *options* must be an array, and the items in this array must be strings; moreover, there must be at least one item, and all items should be unique;
-   *readonly* must be a boolean.

With these added constraints, the schema now looks like this:

```json
{% include example2/entry_schema2.json %}
```

For now, all definitions are empty (an empty JSON Schema validates all instances). We will write schemas for individual definitions below, and fill these schemas into the entry schema.

The *diskDevice* storage type
-----------------------------

This storage type has two required properties, *type* and *device*. The type can only be *disk*, and the device must be an absolute path starting with */dev*. No other properties are allowed:

```json
{% include example2/storage_schema1.json %}
```

You will have noted that we need not specify that *type* must be a string: the constraint described by *enum* is enough.

The *diskUUID* storage type
---------------------------

This storage type has two required properties, *type* and *label*. The type can only be *disk*, and the label must be a valid UUID. No other properties are allowed:

```json
{% include example2/storage_schema2.json %}
```

The *nfs* storage type
----------------------

This storage type has three required properties: *type*, *server* and *remotePath*. What is more, the server may be either a host name, an IPv4 address or an IPv6 address.

For the constraints on *server*, we use a new keyword: *format*. While it is not required that *format* be supported, we will suppose that it is here:

```json
{% include example2/storage_schema3.json %}
```

The *tmpfs* storage type
------------------------

This storage type has two required properties: *type* and *sizeInMB*. The size can only be an integer. What is more, we will require that the size be between 16 and 512, inclusive:

```json
{% include example2/storage_schema4.json %}
```

The full entry schema
---------------------

The resulting schema is quite large:

```json
{% include example2/entry_schema3.json %}
```

Plugging this into our main schema
----------------------------------

Now that all possible entries have been described, we can refer to the entry schema from our main schema. We will, again, use a JSON Reference here:

```json
{% include example2/schema2.json %}
```

Wrapping up
-----------

This example is much more advanced than the previous example; you will have learned of schema referencing and identification, you will have been introduced to other keywords. There are also a few additional points to consider.

### The schema can be improved

This is only an example for learning purposes. Some additional constraints could be described. For instance:

-   it makes no sense for `/` to be mounted on a tmpfs filesystem;
-   it makes no sense to specify the filesystem type if the storage is either NFS or tmpfs.

As an exercise, you can always try and add these constraints. It would probably require splitting the schema further.

### Not all constraints can be expressed

JSON Schema limits itself to describing the structure of JSON data, it cannot express functional constraints.

If we take an NFS entry as an example, JSON Schema alone cannot check that the submitted NFS server's hostname, or IP address, is actually correct: this check is left to applications.

### Tools have varying JSON Schema support

While this is not a concern if you know that the schema you write will be used by you alone, you should keep this in mind if you write a schema which other people can potentially use. The schema we have written here has some features which can be problematic for portability:

-   *format* support is optional, and as such other tools may ignore this keyword: this can lead to a different validation outcome for the same data;
-   it uses regular expressions: care should be taken not to use any advanced features (such as lookarounds), since they may not be supported at the other end;
-   it uses *$schema* to express the need to use draft v4 syntax, but not all tools support draft v4 (in fact, most don't support it).


