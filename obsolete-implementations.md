---
layout: page
title: Obsolete Implementations
permalink: /obsolete-implementations.html
---

_**NOTE:** Due to the long gap after draft-04, many projects that implemented that draft became inactive by the time draft-06 was published, or are looking for new contributors to move forward.  Such projects are listed here_

_For implementations supporting (or actively working towards) draft-06 or later, see the main [Implementations](implementations) page._

_Projects supporting only draft-03 or earlier are no longer listed._

* TOC
{:toc}

Implementations below are written in different languages, and support part, or all, of the specification.

Implementations are classified based on their functionality. When known, the license of the project is also mentioned.

If you have updates to this list, make a pull request on the [GitHub repo](https://github.com/json-schema-org/json-schema-org.github.io).

Validators
----------

### Libraries

<nav class="intra" markdown="1">

{% assign validator-libraries = site.data.validator-libraries-obsolete | sort:"name" %}

{% for language in validator-libraries %}
-   [{{ language.name }}](#validator-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %})
{% endfor %}

</nav>

<!-- To add a validator library, add it in _data/validator-libraries-obsolete.yml -->

<ul>
  {% for language in validator-libraries %}
  <li>
    {{language.name}} <a id="validator-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %}"></a>
    <ul>
    {% for implementation in language.implementations %}
        <li>
        <a href="{{implementation.url}}">{{ implementation.name }}</a>

        {% if implementation.draft %}
            <em>supports draft {{ implementation.draft | join: ", draft " }}</em>
        {% endif %}

        {{implementation.notes | markdownify | remove: '<p>' | remove: '</p>'}}
        ({{ implementation.license | join: ", " }})

        </li>
    {% endfor %}
    </ul>
  </li>
  {% endfor %}
</ul>


### Command Line

<!-- To add a validator library, add it in _data/validator-libraries-obsolete.yml -->

{% for tool in site.data.validator-cli %}
- [{{ tool.name }}]({{ tool.url }}) [draft {{ tool.draft | join: ", draft " }}] ({{ tool.license | join: ", " }}){% if tool.notes %}
  - {{ tool.notes }} {% endif %}{% endfor %}


### Benchmarks

-   Java
    -   [json-schema-validator-benchmark](https://github.com/networknt/json-schema-validator-perftest) - compares performance of three JSON schema validator implementations (only one of which supports draft-06+) in Java(Apache 2.0)

<!-- -->

-   JavaScript
    -   [z-schema validator benchmark](https://github.com/zaggino/z-schema#benchmarks) - compares performance in the individual tests from JSON-Schema Test Suite (MIT)
    -   [JSCK validator benchmark](https://github.com/pandastrike/jsck#benchmarks) - shows performance for JSON-schemas of different complexity (MIT)


Hyper-Schema
---------------------

<nav class="intra" markdown="1">

{% assign hyper-schema-libraries = site.data.hyper-libraries-obsolete | sort:"name" %}

{% for language in hyper-schema-libraries %}
-   [{{ language.name }}](#hyper-schema-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %})
{% endfor %}

</nav>

<!-- To add a hyper-schema library, add it in _data/hyper-schema-libraries.yml -->

<ul>
  {% for language in hyper-schema-libraries %}
  <li>
    {{language.name}} <a id="hyper-schema-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %}"></a>
    <ul>
    {% for implementation in language.implementations %}
        <li>
        <a href="{{implementation.url}}">{{ implementation.name }}</a>

        {% if implementation.draft %}
            <em>supports draft {{ implementation.draft | join: ", draft " }}</em>
        {% endif %}

        {{implementation.notes | markdownify | remove: '<p>' | remove: '</p>'}}
        ({{ implementation.license | join: ", " }})

        </li>
    {% endfor %}
    </ul>
  </li>
  {% endfor %}
</ul>

Schema Generators
-----------------

-   Python
    -   [JSL](https://github.com/aromanovich/jsl) (BSD) - a Python DSL for defining JSON Schemas
-   JavaScript
    -   [json-schema-generator](https://github.com/krg7880/json-schema-generator) (MIT) - Node.js library usable both as a CLI util and as a Node module
-   TypeScript
    -   [Typson](https://github.com/lbovet/typson) (Apache 2.0)
-   Visual Studio
    -   [JSON Schema Generator](https://visualstudiogallery.msdn.microsoft.com/b4515ef8-a518-41ca-b48c-bb1fd4e6faf7) - free extension
-   Sparx Enterprise Architect
    -   [API-Add-In](https://github.com/bayeslife/api-add-in) - Sparx EA extension for exporting JSON Schema from UML models

Generators from schemas
-----------------------

#### Data from schemas

-   JavaScript
    -   [json-schema-generator](https://github.com/json-schema-faker) (MIT) - JSON-Schema + fake data generators

Data Parsing and Code Generation
--------------------------------

-   Delphi
    - [DJsonSchema](https://github.com/schlothauer-wauer/DJsonSchema) (MIT) - JSON Schema reader and code generator for Delphi.
-   Haskell
    -   [aeson-schema](https://github.com/Fuuzetsu/aeson-schema) (MIT) - generates code for a parser
-   Ruby
    -   [autoparse](https://github.com/google/autoparse) (ASL 2.0)
-   Scala
    -   [json-schema-codegen](https://github.com/VoxSupplyChain/json-schema-codegen) - Tool and SBT plugin for generating Scala, TypeScript models and parsers from Json-Schema definitions, *supports draft 4* (Apache 2.0)
    -   [Argus](https://github.com/aishfenton/argus) (MIT) - Macros for building models from JSON Schemas
-   Swift
    -   [Bric-à-brac](https://github.com/glimpseio/BricBrac) (MIT) - generates idiomatic swift structs and parser/serializer from JSON schemas
-   Golang
    -  [gojsonschema](https://github.com/andy-zhangtao/gojsonschema)(Apache 2.0) - golang package for generating golang struct *support for Draft 4*. [Demo](http://json.golang.chinazt.cc)

UI Generation
-------------

_TODO: Sort by draft support._

Various levels of support for UI generation primarily from the validation vocabulary or combined with UI specific definition.

-   JavaScript
    -   [JSON Editor](https://github.com/jdorn/json-editor) (MIT)
    -   [JSONForms](https://jsonforms.io) (EclipseSource) (MIT)
    -   [Jsonary](https://jsonary.com/) (MIT)
    -   [Metawidget](https://metawidget.org/) (LGPL)
    -   [pure-form webcomponent](https://github.com/john-doherty/pure-form) (MIT)

Editors
-------

-   [Liquid XML Studio 2016](https://www.liquid-technologies.com/json-schema-editor) - *Graphical JSON schema editor for draft 4, context sensitive intellisense for JSON documents.*
-   [ReSharper 2016.1](https://www.jetbrains.com/resharper/) - *code completion, inspections and quick fixes for JSON schema in Visual Studio 2010 - 2015, including support for JSON Path and regular expressions for schema editing. Support for draft-4*
-   [Visual Studio 2013](http://www.visualstudio.com/) - *Auto-completion and tooltips based on JSON schema draft 3 and draft 4*
-   [JSON Schema Editor](https://json-schema-editor.tangramjs.com) - *An intuitive editor for JSON schema online*
-   [JSON Editor](https://json-editor.tangramjs.com) - *An online, schema-aware editor for JSON document*

Compatibility
-------------

-   JavaScript
    -   [JSON Schema Compatibility](https://github.com/geraintluff/json-schema-compatability) - *converts draft 3 to draft 4* (Public Domain)


Documentation generation
------------------------

-   JavaScript
    -   [Matic](https://github.com/mattyod/matic) (MIT)
    -   [Docson](https://github.com/lbovet/docson) (Apache 2.0)
    -   [doca](https://github.com/cloudflare/doca/) (BSD) See [@cloudflare/doca](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/doca) for draft-06+ support
    -   [prmd](https://github.com/interagent/prmd) (MIT)

Other
-----

-   JavaScript
    -   [Dojo](https://www.dojotoolkit.org/) (AFL or BSD) - supports some aspects of JSON Schema
    -   [JSON Schema Random](https://github.com/andreineculau/json-schema-random) (Apache 2.0)
