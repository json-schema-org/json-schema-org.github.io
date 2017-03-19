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

Walkthroughs
------------

The two examples below are step-by-step guides into building a schema:

-   [a simple example](example1.html) which covers a classical product catalog description.
-   [a more advanced example](example2.html), using JSON Schema to describe filesystem entries in a Unix-like /etc/fstab file.

The [Space Telescope Science Institute](http://www.stsci.edu/) has also published an [guide aimed at schema authors](http://spacetelescope.github.io/understanding-json-schema/).
