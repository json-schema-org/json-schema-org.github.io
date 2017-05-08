---
layout: page
title: Examples
---

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

|------------------------------------------------------------------------------|-----------------------------------------------------------------|
| [Geographic Coordinate][geo.json] <br> [<small>(canonical url)</small>][geo] | a location as longitude and latitude                            |
| [Card][card.json] <br> [<small>(canonical url)</small>][card]                | a microformat-style representation of a person, company, organization, or place |
| [Calendar][cal.json] <br> [<small>(canonical url)</small>][cal]              | a microformat-style representation of an event                  |
| [Address][addr.json] <br> [<small>(canonical url)</small>][addr]             | a microformat-style representation of a street address          |

Walkthroughs
------------

The two examples below are step-by-step guides into building a schema:

-   [a simple example](example1.md) which covers a classical product catalog description.
-   [a more advanced example](example2.md), using JSON Schema to describe filesystem entries in a Unix-like /etc/fstab file.

The [Space Telescope Science Institute](http://www.stsci.edu/) has also published a [guide aimed at schema authors](http://spacetelescope.github.io/understanding-json-schema/).


[geo.json]: example/geo.json
[card.json]: example/card.json
[cal.json]: example/calendar.json
[addr.json]: example/address.json
[geo]: geo
[card]: card
[cal]: calendar
[addr]: address
