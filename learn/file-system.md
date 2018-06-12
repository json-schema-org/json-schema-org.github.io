---
layout: page
title: Modeling a file system with JSON Schema
---

> Not all constraints to an fstab file can be modeled using JSON Schema alone; however, it can represent a good number of them and the exercise is useful to demonstrate how constraints work.

This example shows a possible JSON Schema representation of file system mount points as represented in an [`/etc/fstab`](https://en.wikipedia.org/wiki/Fstab) file.

An entry in an fstab file can have many different forms; Here is an example:

```json
{
  "/": {
    "storage": {
      "type": "disk",
      "device": "/dev/sda1"
    },
    "fstype": "btrfs",
    "readonly": true
  },
  "/var": {
    "storage": {
      "type": "disk",
      "label": "8f3ba6f4-5c70-46ec-83af-0d5434953e5f"
    },
    "fstype": "ext4",
    "options": [ "nosuid" ]
  },
  "/tmp": {
    "storage": {
      "type": "tmpfs",
      "sizeInMB": 64
    }
  },
  "/var/www": {
    "storage": {
      "type": "nfs",
      "server": "my.nfs.server",
      "remotePath": "/exports/mypath"
    }
  }
}
```

## Creating the schema outline

We will start with a base JSON Schema expressing the following constraints:

* the list of entries is a JSON object;
* the member names (or property names) of this object must all be valid, absolute paths;
* there must be an entry for the root filesystem (ie, `/`).

Building out our JSON Schema from top to bottom:

* The [`$id`](http://json-schema.org/latest/json-schema-core.html#rfc.section.8.2) keyword.
* The [`$schema`](http://json-schema.org/latest/json-schema-core.html#rfc.section.7) keyword.
* The [`type`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.1.1) validation keyword.
* The [`required`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.3) validation keyword.
* The [`properties`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.4) validation keyword with only a `/` entry.
* The [`patternProperties`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.5) validation keyword to match other property names via a regular expression. Note: it does not match `/`).
* The [`additionalProperties`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.6) validation keyword.
  * The value here is `false` to constrain object properties to be either `/` or to match the regular expression.

> You will notice that the regular expression is explicitly anchored (with `^` and `$`): in JSON Schema, regular expressions (in `patternProperties` and in `pattern`) are not anchored by default.

```json
{
  "$id": "http://example.com/fstab",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": [ "/" ],
  "properties": {
    "/": {}
  },
  "patternProperties": {
    "^(/[^/]+)+$": {}
  },
  "additionalProperties": false,
}
```

## Starting an entry

We will start with an outline of the JSON schema which adds new concepts to what we've already demonstrated.

We saw these keywords in the prior exercise: `$id`, `$schema`, `type`, `required` and `properties`.

To this we add:

* The [`description`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.10.1) annotation keyword.
* The [`oneOf`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.7.3) keyword.
* The [`$ref`](http://json-schema.org/latest/json-schema-core.html#rfc.section.8.3) keyword.
  * In this case, all references used are local to the schema using a relative fragment URI (`#/...`).
* The [`definitions`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.9) keyword.
  * Including several key names which we will define later.

```json
{
  "id": "http://example.com/entry-schema",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "description": "JSON Schema for an fstab entry",
  "type": "object",
  "required": [ "storage" ],
  "properties": {
    "storage": {
      "type": "object",
      "oneOf": [
        { "$ref": "#/definitions/diskDevice" },
        { "$ref": "#/definitions/diskUUID" },
        { "$ref": "#/definitions/nfs" },
        { "$ref": "#/definitions/tmpfs" }
      ]
    }
  },
  "definitions": {
    "diskDevice": {},
    "diskUUID": {},
    "nfs": {},
    "tmpfs": {}
  }
}
```

## Constraining entries

Let's now extend this skeleton to add constraints to some of the properties.

* Our `fstype` key uses the [`enum`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.1.2) validation keyword.
* Our `options` key uses the following:
  * The `type` validation keyword (see above).
  * The [`minItems`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.4) validation keyword.
  * The [`items`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.1) validation keyword.
  * The [`uniqueItems`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.5) validation keyword.
  * Together these say: `options` must be an array, and the items therein must be strings, there must be at least one item, and all items should be unique.
* We have a `readonly` key.

With these added constraints, the schema now looks like this:

```json
{
  "id": "http://example.com/entry-schema",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "description": "JSON Schema for an fstab entry",
  "type": "object",
  "required": [ "storage" ],
  "properties": {
    "storage": {
      "type": "object",
      "oneOf": [
        { "$ref": "#/definitions/diskDevice" },
        { "$ref": "#/definitions/diskUUID" },
        { "$ref": "#/definitions/nfs" },
        { "$ref": "#/definitions/tmpfs" }
      ]
    },
    "fstype": {
      "enum": [ "ext3", "ext4", "btrfs" ]
    },
    "options": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "string"
      },
      "uniqueItems": true
    },
    "readonly": {
      "type": "boolean"
    }
  },
  "definitions": {
    "diskDevice": {},
    "diskUUID": {},
    "nfs": {},
    "tmpfs": {}
  }
}
```

## The `diskDevice` definition

One new keyword is introduced here:

* The [`pattern`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.3.3) validation keyword notes the `device` key must be an absolute path starting with */dev*.

```json
{
  "diskDevice": {
    "properties": {
      "type": {
        "enum": [ "disk" ]
      },
      "device": {
        "type": "string",
        "pattern": "^/dev/[^/]+(/[^/]+)*$"
      }
    },
    "required": [ "type", "device" ],
    "additionalProperties": false
  }
}
```

## The `diskUUID` definition

No new keywords are introduced here.

We do have a new key: `label` and the `pattern` validation keyword states it must be a valid UUID.

```json
{
  "diskUUID": {
    "properties": {
      "type": {
        "enum": [ "disk" ]
      },
      "label": {
        "type": "string",
        "pattern": "^[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}$"
      }
    },
    "required": [ "type", "label" ],
    "additionalProperties": false
  }
}
```

## The `nfs` definition

We find another new keyword:

* The [`format`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.7) annotation and assertion keyword.

```json
{
  "nfs": {
    "properties": {
      "type": { "enum": [ "nfs" ] },
      "remotePath": {
        "type": "string",
        "pattern": "^(/[^/]+)+$"
      },
      "server": {
        "type": "string",
        "oneOf": [
          { "format": "hostname" },
          { "format": "ipv4" },
          { "format": "ipv6" }
        ]
      }
    },
    "required": [ "type", "server", "remotePath" ],
    "additionalProperties": false
  }
}
```

## The *tmpfs* definition

Our last definition introduces two new keywords:

* The [`minimum`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.4) validation keyword.
* The [`maximum`](http://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.2) validation keword.
* Together these require the size be between 16 and 512, inclusive.

```json
{
  "tmpfs": {
    "properties": {
      "type": { "enum": [ "tmpfs" ] },
      "sizeInMB": {
        "type": "integer",
        "minimum": 16,
        "maximum": 512
      }
    },
    "required": [ "type", "sizeInMB" ],
    "additionalProperties": false
  }
}
```

## The full entry schema

The resulting schema is quite large:

```json
{
  "id": "http://some.site.somewhere/entry-schema#",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "description": "schema for an fstab entry",
  "type": "object",
  "required": [ "storage" ],
  "properties": {
    "storage": {
      "type": "object",
      "oneOf": [
        { "$ref": "#/definitions/diskDevice" },
        { "$ref": "#/definitions/diskUUID" },
        { "$ref": "#/definitions/nfs" },
        { "$ref": "#/definitions/tmpfs" }
      ]
    },
    "fstype": {
      "enum": [ "ext3", "ext4", "btrfs" ]
    },
    "options": {
      "type": "array",
      "minItems": 1,
      "items": { "type": "string" },
      "uniqueItems": true
    },
    "readonly": { "type": "boolean" }
  },
  "definitions": {
    "diskDevice": {
      "properties": {
        "type": { "enum": [ "disk" ] },
        "device": {
          "type": "string",
          "pattern": "^/dev/[^/]+(/[^/]+)*$"
        }
      },
      "required": [ "type", "device" ],
      "additionalProperties": false
    },
    "diskUUID": {
      "properties": {
        "type": { "enum": [ "disk" ] },
        "label": {
          "type": "string",
          "pattern": "^[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}$"
        }
      },
      "required": [ "type", "label" ],
      "additionalProperties": false
    },
    "nfs": {
      "properties": {
        "type": { "enum": [ "nfs" ] },
        "remotePath": {
          "type": "string",
          "pattern": "^(/[^/]+)+$"
        },
        "server": {
          "type": "string",
          "oneOf": [
            { "format": "hostname" },
            { "format": "ipv4" },
            { "format": "ipv6" }
          ]
        }
      },
      "required": [ "type", "server", "remotePath" ],
      "additionalProperties": false
    },
    "tmpfs": {
      "properties": {
        "type": { "enum": [ "tmpfs" ] },
        "sizeInMB": {
          "type": "integer",
          "minimum": 16,
          "maximum": 512
        }
      },
      "required": [ "type", "sizeInMB" ],
      "additionalProperties": false
    }
  }
}
```

## Plugging this into our main schema

Now that all possible entries have been described, we can refer to the entry schema from our main schema. We will, again, use a JSON Reference here:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "/": { "$ref": "http://some.site.somewhere/entry-schema#" }
  },
  "patternProperties": {
    "^(/[^/]+)+$": { "$ref": "http://some.site.somewhere/entry-schema#" }
  },
  "additionalProperties": false,
  "required": [ "/" ]
}
```

## Wrapping up

This example is much more advanced than the previous example; you will have learned of schema referencing and identification, you will have been introduced to other keywords. There are also a few additional points to consider.

### The schema can be improved

This is only an example for learning purposes. Some additional constraints could be described. For instance:

* it makes no sense for `/` to be mounted on a tmpfs filesystem;
* it makes no sense to specify the filesystem type if the storage is either NFS or tmpfs.

As an exercise, you can always try to add these constraints. It would probably require splitting the schema further.

### Not all constraints can be expressed

JSON Schema limits itself to describing the structure of JSON data, it cannot express functional constraints.

If we take an NFS entry as an example, JSON Schema alone cannot check that the submitted NFS server's hostname, or IP address, is actually correct: this check is left to applications.

### Tools have varying JSON Schema support

While this is not a concern if you know that the schema you write will be used by you alone, you should keep this in mind if you write a schema which other people can potentially use. The schema we have written here has some features which can be problematic for portability:

* *format* support is optional, and as such other tools may ignore this keyword: this can lead to a different validation outcome for the same data;
* it uses regular expressions: care should be taken not to use any advanced features (such as lookarounds), since they may not be supported at the other end;
* it uses *$schema* to express the need to use draft v6 compliant processing, but not all tools support draft v6.
