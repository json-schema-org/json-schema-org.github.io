---
layout: page
---

Basic example
-------------

Here is a basic example of a JSON Schema:

```json
{
    "title": "Person",
    "type": "object",
    "properties": {
        "firstName": {
            "type": "string"
        },
        "lastName": {
            "type": "string"
        },
        "age": {
            "description": "Age in years",
            "type": "integer",
            "minimum": 0
        }
    },
    "required": ["firstName", "lastName"]
}
```

Example schemas
---------------

These sample schemas describe simple data structures which can be expressed as JSON:

|-----------------------------------------------------|---------------------------------------------------------------------------------|
| [Geographic Coordinate](http://json-schema.org/geo) | a location as longitude and latitude                                            |
| [Card](http://json-schema.org/card)                 | a microformat-style representation of a person, company, organization, or place |
| [Calendar](http://json-schema.org/calendar)         | a microformat-style representation of an event                                  |
| [Address](http://json-schema.org/address)           | a microformat-style representation of a street address                          |

Walkthroughs
------------

The two examples below are step-by-step guides into building a schema:

-   [a simple example](example1.md) which covers a classical product catalog description.
-   [a more advanced example](example2.md), using JSON Schema to describe filesystem entries in a Unix-like /etc/fstab file.

The [Space Telescope Science Institute](http://www.stsci.edu/) has also published a [guide aimed at schema authors](http://spacetelescope.github.io/understanding-json-schema/).
