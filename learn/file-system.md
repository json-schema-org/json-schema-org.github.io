---
layout: page
title: Modeling a file system with JSON Schema
---

* [Introduction](#introduction)
* [Creating the `fstab` schema](#fstab-schema)
* [Starting the `entry` schema](#entry-schema)
* [Constraining an entry](#constraining-entry)
* [The `diskDevice` definition](#diskdevice)
* [The `diskUUID` definition](#diskuuid)
* [The `nfs` definition](#nfs)
* [The `tmpfs` definition](#tmpfs)
* [The full entry schema](#full-entry)
* [Referencing the `entry` schema in the `fstab` schema](#referencing-entry)

## <a name="introduction"></a>Introduction

> Not all constraints to an fstab file can be modeled using JSON Schema alone; however, it can represent a good number of them and the exercise is useful to demonstrate how constraints work. The examples provided are illustrative of the JSON Schema concepts rather than a real, working schema for an fstab file.

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

## <a name="fstab-schema"></a>Creating the `fstab` schema

We will start with a base JSON Schema expressing the following constraints:

* the list of entries is a JSON object;
* the member names (or property names) of this object must all be valid, absolute paths;
* there must be an entry for the root filesystem (ie, `/`).

Building out our JSON Schema from top to bottom:

* The [`$id`](https://json-schema.org/latest/json-schema-core.html#rfc.section.8.2) keyword.
* The [`$schema`](https://json-schema.org/latest/json-schema-core.html#rfc.section.7) keyword.
* The [`type`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.1.1) validation keyword.
* The [`required`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.3) validation keyword.
* The [`properties`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.4) validation keyword.
  * The `/` key is empty now; We will fill it out later.
* The [`patternProperties`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.5) validation keyword.
  * This matches other property names via a regular expression. Note: it does not match `/`.
  * The `^(/[^/]+)+$` key is empty now; We will fill it out later.
* The [`additionalProperties`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.5.6) validation keyword.
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

## <a name="entry-schema"></a>Starting the `entry` schema

We will start with an outline of the JSON schema which adds new concepts to what we've already demonstrated.

We saw these keywords in the prior exercise: `$id`, `$schema`, `type`, `required` and `properties`.

To this we add:

* The [`description`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.10.1) annotation keyword.
* The [`oneOf`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.7.3) keyword.
* The [`$ref`](https://json-schema.org/latest/json-schema-core.html#rfc.section.8.3) keyword.
  * In this case, all references used are local to the schema using a relative fragment URI (`#/...`).
* The [`definitions`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.9) keyword.
  * Including several key names which we will define later.

```json
{
  "$id": "http://example.com/entry-schema",
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

## <a name="constraining-entry"></a>Constraining an entry

Let's now extend this skeleton to add constraints to some of the properties.

* Our `fstype` key uses the [`enum`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.1.2) validation keyword.
* Our `options` key uses the following:
  * The `type` validation keyword (see above).
  * The [`minItems`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.4) validation keyword.
  * The [`items`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.1) validation keyword.
  * The [`uniqueItems`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.4.5) validation keyword.
  * Together these say: `options` must be an array, and the items therein must be strings, there must be at least one item, and all items should be unique.
* We have a `readonly` key.

With these added constraints, the schema now looks like this:

```json
{
  "$id": "http://example.com/entry-schema",
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

## <a name="diskdevice"></a>The `diskDevice` definition

One new keyword is introduced here:

* The [`pattern`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.3.3) validation keyword notes the `device` key must be an absolute path starting with */dev*.

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

## <a name="diskuuid"></a>The `diskUUID` definition

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

## <a name="nfs"></a>The `nfs` definition

We find another new keyword:

* The [`format`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.7) annotation and assertion keyword.

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

## <a name="tmpfs"></a>The `tmpfs` definition

Our last definition introduces two new keywords:

* The [`minimum`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.4) validation keyword.
* The [`maximum`](https://json-schema.org/latest/json-schema-validation.html#rfc.section.6.2.2) validation keword.
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

## <a name="full-entry"></a>The full entry schema

The resulting schema is quite large:

```json
{
  "$id": "http://example.com/entry-schema",
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
    },
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

## <a name="referencing-entry"></a>Referencing the `entry` schema in the `fstab` schema

Coming full circle we use the `$ref` keyword to add our entry schema into the keys left empty at the start of the exercise:

* The `/` key.
* The `^(/[^/]+)+$` key.

```json
{
  "$id": "http://example.com/fstab",
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": [ "/" ],
  "properties": {
    "/": { "$ref": "http://example.com/entry-schema" }
  },
  "patternProperties": {
    "^(/[^/]+)+$":  { "$ref": "http://example.com/entry-schema" }
  },
  "additionalProperties": false,
}
```
