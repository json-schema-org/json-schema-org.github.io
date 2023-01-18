---
layout: page
title: Implementations
permalink: /implementations.html
---

_**NOTE:** This page lists implementations with (or actively working towards) support for draft-06 or later._

_For implementations supporting only draft-04 or older, or that are no longer in development, see the [Obsolete Implementations](obsolete-implementations) page._


* TOC
{:toc}

Implementations below are written in different languages, and support part, or all, of at least one recent version of the specification.


Implementations are classified based on their functionality. When known, the license of the project is also mentioned.

If you have updates to this list, make a pull request on the [GitHub repo](https://github.com/json-schema-org/json-schema-org.github.io).

Listing does not signify a recommendation or endorsement of any kind.

Validators
----------

<nav class="intra" markdown="1">

{% assign validator-libraries = site.data.validator-libraries-modern | sort: 'name' %}

{% for language in validator-libraries %}
-   [{{ language.name }}](#validator-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %})
{% endfor %}

</nav>

<!-- To add a validator, add it in _data/validator-libraries-modern.yml -->

<ul>
  {% for language in validator-libraries %}
  <li>
    <strong>{{language.name}}</strong><a id="validator-{% if language.anchor-name %}{{ language.anchor-name }}{% else %}{{ language.name | downcase }}{% endif %}"></a>
    {% if language.notes %}
        <p>{{ language.notes }}</p>
    {% endif %}
    <ul>
    {% for implementation in language.implementations %}
      <li>
        <a name="{{ implementation.anchor-name | default: implementation.name }}"></a>
        <a href="{{ implementation.url}}">{{ implementation.name }}</a>
        <ul>
          <li><em>Supports:</em>
            {% if implementation.date-draft %}
                {{ implementation.date-draft | sort | reverse | join: ", " }}
            {% endif %}
            {% if implementation.draft %}
                draft-0{{ implementation.draft | sort | reverse | join: ", -0" }}
            {% endif %}
          </li>

        {% if implementation.compliance %}
          <li><em>Compliance:</em>
          {% if implementation.compliance.config.docs %}
            This implementation <a href="{{ implementation.compliance.config.docs }}">documents</a> that you must
          {% endif %}
          {% if implementation.compliance.config.instructions %}
            <strong>{{ implementation.compliance.config.instructions | markdownify | remove: '<p>' | remove: '</p>' }}</strong> to produce specification-compliant behavior.
          </li>
          {% endif %}
        {% endif %}

        {% if implementation.built-on %}
          <li><em>Built on:</em>
            <a href="#{{ implementation.built-on.anchor-name | default: implementation.built-on.name }}">{{ implementation.built-on.name }}</a>
          </li>
        {% endif %}

        {% if implementation.license %}
          <li><em>License:</em>
            {{ implementation.license | join: ", " }}
          </li>
        {% endif %}

        {% if implementation.notes %}
          <li><em>Notes:</em>
            {{ implementation.notes | markdownify | remove: '<p>' | remove: '</p>' }}
          </li>
        {% endif %}

        {% if implementation.last-updated %}
          <li><em>Information last updated:</em>
            {{ implementation.last-updated }}
          </li>
        {% endif %}

        </ul>
      </li>
    {% endfor %}
    </ul>
  </li>
  <p />
  {% endfor %}
</ul>

#### Benchmarks

Benchmarks that compare at least two implementations supporting draft-06+ may be listed here.

-   Go
    -   [validator-benchmarks](https://github.com/TheWildBlue/validator-benchmarks) - benchmark of Go JSON Schema validators based on official test suite

-   JavaScript
    -   [json-schema-benchmark](https://github.com/ebdrup/json-schema-benchmark) - an independent benchmark for Node.js JSON-schema validators based on JSON-Schema Test Suite (MIT)

-   PHP
    -   [php-json-schema-bench](https://github.com/swaggest/php-json-schema-bench) - comparative benchmark for JSON-schema PHP validators using JSON-Schema Test Suite and z-schema/JSCK (MIT)

#### API documentation

-   JavaScript
    -   [@cloudflare/doca](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/doca) ([JSON Schema Tools](https://github.com/cloudflare/json-schema-tools)), _draft-04, -06, -07, and Doca extensions_ (UI forthcoming)
    -   [@adobe/jsonschema2md](https://github.com/adobe/jsonschema2md) makes it easier by providing a number of scripts that can turn JSON Schema files into readable Markdown documentation that is ready for consumption on GitHub or processed using Jekyll or other static site generators. _JSON Schema 2019-09_ ([partial](https://github.com/adobe/jsonschema2md/blob/master/schemasupport.md))

-   Python
    -   [FastAPI](https://github.com/tiangolo/fastapi) (MIT) is an API framework based on Python 3.6+ types that generates **OpenAPI 3** schemas, including **JSON Schemas** for all the models declared.

#### Link Description Object utilities

-   JavaScript
    -   [@cloudflare/json-hyper-schema](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/json-hyper-schema) _draft-07, -06, -04_ (BSD-3-Clause)


Schema generators
-----------------

Schema generators need not support generating every schema keyword.
For schema generators, compatibility with a draft means that either:

* Schemas produced explicitly set the draft with `$schema`
* Schemas produced lack `$schema` but are valid against the appropriate meta-schema

For example, the only incompatibilities between draft-04 and draft-06 involve `exclusiveMinimum`, `exclusiveMaximum`, and `id` vs `$id`.  If a generator does not set `$schema` and does not ever emit those keywords, then it is compatible with draft-06 even if it was written with draft-04 in mind.

#### From code

-   .NET
    -   [Json.NET](https://www.newtonsoft.com/jsonschema) (AGPL-3.0) - generates schemas from .NET types
    -   [NJsonSchema](https://github.com/RSuter/NJsonSchema/) - (Ms-PL) - generates schemas from .NET types, see issue [574](https://github.com/RSuter/NJsonSchema/issues/574) for draft-06+ support progress
    -   [JsonSchema.Net.Generation](https://github.com/gregsdennis/json-everything) (MIT) - generates schemas from .NET types
-   Go
    -   [jsonschema](https://github.com/invopop/jsonschema) - (MIT) - generate schemas from Go structs. Supports Draft 2020-12.
-   PHP
    -   [Liform](https://github.com/Limenius/liform) (MIT) - generates schemas from Symfony forms
-   TypeScript
    -   [typescript-json-schema](https://github.com/YousefED/typescript-json-schema)
-   Python
    -   [Pydantic](https://pydantic-docs.helpmanual.io/) (MIT) - generates schemas from Python models based on Python 3.6+ type hints.
    -   [msgspec](https://jcristharif.com/msgspec/) (BSD-3-Clause) - generates schemas from Python type annotations.
-   Java
    -   [jsonschema-generator](https://github.com/victools/jsonschema-generator) (Apache 2.0) - generates schemas from Java types *supports Draft 7 and Draft 2019-09*
-   Scala
    -   [scala-jsonschema](https://github.com/andyglow/scala-jsonschema) (Apache 2.0) - generates schemad out of Scala case classes
-   Ruby
    -   [Shale](https://github.com/kgiszczak/shale) (MIT) - generates schema from Ruby models *supports Draft 2020-12*

#### From data

-   Java
    -   [saasquatch/json-schema-inferrer](https://github.com/saasquatch/json-schema-inferrer) _draft-07, -06, -04_ (Apache 2.0) - Java library for inferring JSON Schemas from one or multiple JSON samples.
-   Scala
    -   [Schema Guru](https://github.com/snowplow/schema-guru) (Apache 2.0) - CLI util, Spark Job and Web UI for deriving JSON Schemas out of corpus of JSON instances; see issue [178](https://github.com/snowplow/schema-guru/issues/178) for progress towards draft-06+ support
    -   [JSONoid](https://github.com/michaelmior/jsonoid-discovery/) (MIT) - command line tool and Spark application for inferring schemas from JSON documents, supports draft 2019-09
-   Clojure
    -   [luposlip/json-schema](https://github.com/luposlip/json-schema) (Apache 2.0) - infer JSON Schema from Clojure data
-   Online (web tool)
    -   [jsonschema.net](https://www.jsonschema.net/) - generates schemas from example data
    -   [Liquid Online Tools](https://www.liquid-technologies.com/online-json-to-schema-converter) - infer JSON Schema from sample JSON data
    -   [quicktype.io](https://app.quicktype.io/#l=schema) - infer JSON Schema from samples, and generate TypeScript, C++, go, Java, C#, Swift, etc. types from JSON Schema

Generators from schemas
-----------------------

Tools that generate artifacts from schemas need not support every keyword,
as not all keywords work well for generative use cases.

Generators are considered compatible with a draft if they support (or benignly
ignore) the appropriate `$schema` value, and interpret the keywords that they
do support according to that draft.

For example, if a generator that was originally written for draft-04 does not
support `id`, `exclusiveMinimum`, or `exclusiveMaxium`, then as long as it does
not require a draft-04 `$schema`, it is compatible with draft-06 since those
are the only keywords that changed.

#### Code generation

-   Elm
    -  [json-schema-to-elm](https://github.com/dragonwasrobot/json-schema-to-elm) - generates Elm types, JSON decoders+encoders, and fuzz tests from one or more JSON Schema files, using [dragonwasrobot/json_schema](https://github.com/dragonwasrobot/json_schema) *supports Draft 7*
-   Go
    -  [go-jsonschema](https://git.sr.ht/~emersion/go-jsonschema) - generates Go types and helpers from JSON schema files *supports Draft 2020-12* 
-   Java
    - [jsonCodeGen](https://github.com/schlothauer-wauer/jsoncodegen) (MIT) - Groovy based generation tasks from JSON schema. Already includes templates/generators for Java Beans, Swagger specification files and PlantUML diagrams.
    - [jsonschema2pojo](https://github.com/joelittlejohn/jsonschema2pojo) (Apache 2.0) - generates Java types from JSON Schema (or example JSON) and can annotate those types for data-binding with Jackson 2.x or Gson. *draft-07*
    - [jsonschematypes](https://github.com/jimblackler/jsonschematypes) (Apache 2.0) - Java library to generate Java or TypeScript classes from standard JSON Schemas. *JSON Schema 2019-09, draft-07, -06, -04, -03*
    - [jsongenerator](https://github.com/jimblackler/jsonschematypes/tree/master/codegen) *JSON Schema 2019-09, draft-07, -06, -04, -03* (Apache-2.0)
-   Kotlin
    - [json-kotlin-schema-codegen](https://github.com/pwall567/json-kotlin-schema-codegen) (MIT) - Generates Kotlin data classes, Java classes or TypeScript interfaces from JSON Schema.
-   Online (web tool)
    -   [quicktype.io](https://app.quicktype.io/#l=schema) - infer JSON Schema from samples, and generate TypeScript, C++, go, Java, C#, Swift, etc. types from JSON Schema
-   PHP
    -  [php-code-builder](https://github.com/swaggest/php-code-builder)(MIT) - generates PHP mapping structures defined by JSON schema using [swaggest/json-schema](https://github.com/swaggest/php-json-schema) *supports Draft 7*
-   Python
    - [yacg](https://github.com/OkieOth/yacg) (MIT) - parse JSON Schema and OpenApi files to build a meta model from them. This meta model can be used in Mako templates to generate source code, other schemas or plantUml.
    - [statham](https://github.com/jacksmith15/statham-schema) (MIT) - generate type-annotated models from JSON Schema documents.
-   Rust
    - [schemafy](https://github.com/Marwes/schemafy/) - generates Rust types and serialization code from a JSON schema. *supports Draft 4*
-   TypeScript
    - [jsongenerator](https://github.com/jimblackler/jsonschematypes/tree/master/codegen) *JSON Schema 2019-09, draft-07, -06, -04, -03* (Apache-2.0)
-   Ruby
    - [Shale](https://github.com/kgiszczak/shale) (MIT) - generates Ruby models from a JSON schema *supports Draft 2020-12*

#### Web UI generation

_TODO: Sort by draft support._

Various levels of support for UI generation primarily from the validation vocabulary or combined with UI specific definition.

-   JavaScript
    -   [Alpaca Forms](http://www.alpacajs.org/) (ASL 2.0)
    -   [Angular Schema Form](https://github.com/json-schema-form/angular-schema-form) (MIT)
    -   [Angular2 Schema Form](https://github.com/makinacorpus/angular2-schema-form) *unrelated to Angular Schema Form* (MIT)
    -   [Angular6-json-schema-form](https://github.com/hamzahamidi/Angular6-json-schema-form) (MIT)
    -   [Dashjoin JSON Schema Form](https://github.com/dashjoin/json-schema-form) (Apache 2) *draft-06 (minus oneOf, anyOf, allOf, not)*
    -   [JSON Editor](https://github.com/json-editor/json-editor) (MIT)
    -   [JSON Form (joshfire)](https://github.com/joshfire/jsonform) (joshfire) (MIT)
    -   [Json Forms (brutusin)](https://github.com/brutusin/json-forms) (brutusin) (MIT)
    -   [JSONForms (jsonforms.io)](https://jsonforms.io/) (EclipseSource) (MIT)
    -   [Liform-react](https://github.com/Limenius/liform-react) (MIT)
    -   [React JSON Schema Form (mozilla)](https://github.com/mozilla-services/react-jsonschema-form) (Apache 2)
    -   [React Json Schema Form (Mui)](https://github.com/vip-git/react-jsonschema-form-material-ui) (MIT)
    -   [React Schema Form (networknt)](https://github.com/networknt/react-schema-form) (MIT)
    -   [Restspace Schema Form](https://github.com/restspace/schema-form) (MIT)
    -   [uniforms (Vazco)](https://github.com/vazco/uniforms) (MIT)
    -   [UI Schema for React](https://github.com/ui-schema/ui-schema) (MIT) *2019-09 / draft-08, -07, -06, -04 (incompatible `type=integer`)*

#### Data from schemas

-   .Net
    -   [JsonSchema.Net.DataGeneration](https://github.com/gregsdennis/json-everything) (MIT) Data generation from JSON schemas, powered by the Bogus testing data generation library.
-   Python
    -   [hypothesis-jsonschema](https://github.com/Zac-HD/hypothesis-jsonschema) (MPL) *draft-07, -06, -04*;  takes any schema, even with complex and interacting constraints, and returns a [Hypothesis](https://hypothesis.works/) strategy which can generate valid documents for testing.
-   Java
    -   [jsongenerator](https://github.com/jimblackler/jsongenerator) *JSON Schema 2019-09, draft-07, -06, -04, -03* (Apache-2.0)

Utilities
---------

Draft compatibility for utilities is generally specific to the purpose of
the utility, and decided on a case-by-case basis.

#### General processing

-   JavaScript
    -   [json-schema-ref-parser](https://github.com/BigstickCarpet/json-schema-ref-parser) (MIT) Tools for dereferencing non-cyclic schemas, bundling referenced schemas into a single file, and other `$ref` processing.
    -   [json-schema-library](https://github.com/sagold/json-schema-library) (MIT) - Exposes tools to work with json-schema, including: data creation from json-schema, `$ref` processing, walk through schemas, etc.
    -   [@cloudflare/json-schema-walker](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/json-schema-walker) ([JSON Schema Tools](https://github.com/cloudflare/json-schema-tools)), _draft-07, -06, -04, and Cloudflare's Doca extensions_ Walks schemas and runs pre- and post-walk callbacks.  Can modify schemas in place. (BSD-3-Clause)
    -   [@hyperjump/json-schema-core](https://github.com/jdesrosiers/json-schema-core)
        (MIT) Tools for working with schemas that handle identifiers and
        references. Build vocabularies and other JSON Schema based tools.

#### Schema to Schema

-   JavaScript
    -   [@cloudflare/json-schema-transform](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/json-schema-transform) ([JSON Schema Tools](https://github.com/cloudflare/json-schema-tools)), (BSD-3-Clause) Utilities using @cloudflare/json-schema-walker for transformations including `allOf` merging and example roll-up.
    -   [mokkabanna/json-schema-merge-allof](https://github.com/mokkabonna/json-schema-merge-allof) (MIT)
    -   [mokkabanna/json-schema-compare](https://github.com/mokkabonna/json-schema-compare) (MIT)
    -   [loganvolkers/json-schema-resolve-allof](https://github.com/loganvolkers/json-schema-resolve-allof) (_license not stated_)
    -   [JSON-Schema-Instantiator](https://github.com/tomarad/JSON-Schema-Instantiator) (MIT)

#### Schema draft migration

-   JavaScript
    -    [AlterSchema](https://github.com/sourcemeta/alterschema) _JSON Schema 2020-12, 2019-09, draft-07, -06, -04_

#### Format converters

-   OpenAPI
    -   [JSON Schema to OpenAPI Schema](https://github.com/wework/json-schema-to-openapi-schema) _draft-04_ Draft-06 and -07 planned per README (MIT)
-   Orderly
    -   [Orderly](https://github.com/lloyd/orderly) (BSD-3-Clause)
-   RAML
    -   [ramldt2jsonschema](https://github.com/raml-org/ramldt2jsonschema) _draft-06, 04_ (Apache-2.0)
-   Webpack
    -   [@cloudflare/json-schema-ref-loader](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/json-schema-ref-loader) ([JSON Schema Tools](https://github.com/cloudflare/json-schema-tools)), (BSD-3-Clause) Webpack loader for dereference-able schemas in JSON, JSON5, YAML, or JavaScript
    -   [@cloudflare/json-schema-apidoc-loader](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/json-schema-apidoc-loader) ([JSON Schema Tools](https://github.com/cloudflare/json-schema-tools)), Back-end for [@cloudflare/doca](https://github.com/cloudflare/json-schema-tools/tree/master/workspaces/doca), _draft-04, -06, -07, and Doca extensions_
-   XSD
    -   [Oxygen XSD to JSON Schema](https://www.oxygenxml.com/json_converter.html#xsd-to-json-schema) - Generate from an XSD file a similar JSON Schema structure.
    -   [XSD to JSON Schema IntellJ plugin](https://plugins.jetbrains.com/plugin/19024-xsd-to-json-schema) - Plugin for converting an XML Schema (XSD) file to a JSON Schema file.

#### Testing

-   Python
    -   [hypothesis-jsonschema](https://github.com/Zac-HD/hypothesis-jsonschema) (MPL) *draft-07, -06, -04*;  takes any schema, even with complex and interacting constraints, and returns a [Hypothesis](https://hypothesis.works/) strategy which can generate valid documents for testing.

#### Editors

-   [Altova XMLSpy 2019r3](https://www.altova.com/xmlspy-xml-editor#json_schema) - *Graphical JSON Schema editor for draft-06 and draft-7, as well as validation of JSON files based on JSON Schema*
-   [Dashjoin JSON Schema editor](https://dashjoin.github.io/#/schema) - *Graphical online JSON Schema editor for draft-06 (minus oneOf, anyOf, allOf, not). The generated schema can be tested immediately via a form that is driven by it.*
-   [Hackolade Studio](https://hackolade.com/help/JSONSchemaEditor.html) - *Visual JSON Schema editor for draft-04, draft-06, draft-07, 2019-09, 2020-12, as well as data modeling tool for NoSQL databases, storage formats, REST APIs, and JSON in RDBMS.  Also converts to and from: different draft specifications, DDL, XSD, Swagger, OpenAPI, YAML, Avro, Parquet, Protobuf, and most of the NoSQL script syntaxes.  Includes a GUI for Entity-Relationship Diagrams and a Command-Line Interface.*
-   [JSONBuddy](https://www.json-buddy.com/) - *Text and grid-style JSON editor and validator with JSON schema analyzer, context sensitive entry-helpers and sample data generation based on JSON schema. Support for draft-4, draft-6, draft-7 and 2019-09. JSON Schema validation debugger: Step through the validation process and set breakpoints.*
-   [JSONEditor Online](https://jsoneditoronline.org/) - *View, edit, format, and validate JSON online* Support draft-4, draft-6, and draft-7.
-   [Liquid JSON Schema Editor](https://www.liquid-technologies.com/json-schema-editor) - *Graphical JSON Schema editor for draft-04, draft-06, draft-07 and 2019-09, with split source code and grphical editing. Includes validation of JSON files based on JSON Schema, JSON Sample Generator and JSON Schema Documentation Generator.*
-   [Oxygen JSON Schema Editor](https://www.oxygenxml.com/xml_developer/json_schema_editor.html) - *JSON Schema editor with a variety of editing features and helper views (Design/Text/Author). Support for validation and editing JSON Schema draft-4, draft-6, and draft-7, 2019-09 (partial), 2020-12 (partial). Validation and editing of JSON files based on JSON Schema.*
-   [Perseid Modeler](https://www.datensen.com/data-modeling/perseid-modeler-for-json-schema.html) - *a modeling tool for JSON Schema and OpenAPI. Key features include: visual JSON schema creation using tree and ERD-like diagrams, support for JSON schema structures including operators and conditions, import of existing schemas from files, creation of detailed HTML reports, export to PDF, script generation, and more. Supported versions: draft 4, 6, 7, 2019-09 and 2020-12*
-   [Stoplight Studio](https://stoplight.io/) - *JSON Schema IDE (text-based and GUI) with support for JSON/YAML linting, which can also be based on JSON Schema rules via Spectral. Support for draft-4, draft-6 and draft-7.*
-   [Visual Studio Code](https://code.visualstudio.com/) - *Schema driven code completion, hovers and validation for editing JSON files (including schemas)*
-   [WebStorm](https://www.jetbrains.com/webstorm/), [IntelliJ IDEA](https://www.jetbrains.com/idea/), and other [JetBrains IDEs](https://www.jetbrains.com/products.html?fromMenu#type=ide) - *Code completion, documentation, and validation for JSON and YAML files using JSON Schema. Support for draft-4, draft-6, and draft-7.*
-   [Eclipse IDE](https://www.eclipse.org/downloads/eclipse-packages) - *Rich JSON edition supporting schema for instantaneous validation and error reporting, completion, documentation.*

#### Documentation generators


- [json-schema-static-docs](https://tomcollins.github.io/json-schema-static-docs/) - Generates human friendly markdown documentation from JSON Schema. Includes links between pages based on $ref values. Supports draft-7.
- [jsonschematic](https://github.com/yanick/jsonschematic/) - Svelte-based schema viewer. Runs as a local web app. Supports draft-7.
- [docson](https://github.com/lbovet/docson) - Javascript-based schema viewer.  Runs as a local web app. Supports draft-4.
- [json-schema-for-humans](https://pypi.org/project/json-schema-for-humans/) - Generate HTML representation of a schema. Python-based. Supports draft-7.
- [oXygen JSON Schema Documentation](https://www.oxygenxml.com/json_converter.html#generate-json-schema-documentation) - Generate JSON Schema documentation in HTML format including diagrams.
- [wetzel](https://github.com/CesiumGS/wetzel) - Generates Markdown and AsciiDoc. With some limitations, supports draft-3, draft-4, draft-7, and 2020-12.

Schema Repositories
-------------------

-   [SchemaStore.org](https://schemastore.org/json/) - validate against common JSON Schemas


Schema Linter
-------------

-   [json-schema-linter](https://www.json-schema-linter.com/) - Lint/validate/parse json-schema itself, and find typos, missing properties, missing required keys, etc. Supports draft 4, 6, and 7.
-   [Stoplight Spectral](https://stoplight.io/open-source/spectral) - A flexible JSON/YAML linter for creating automated style guides, with baked in support for OpenAPI v2/v3 and JSON Schema. Supports draft 4, 6, and 7.

Hyper-Schema
---------------------

<nav class="intra" markdown="1">

{% assign hyper-schema-libraries = site.data.hyper-libraries-modern | sort:"name" %}

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
            <em>draft-0{{ implementation.draft | join: ", -0" }}</em>
        {% endif %}

        {{implementation.notes | markdownify | remove: '<p>' | remove: '</p>'}}

        {% if implementation.license %}
            ({{ implementation.license | join: ", " }})
        {% endif %}

        {% if implementation.last-updated %}
          <br />
          <em>Information last updated:</em>
          {{ implementation.last-updated }}
        {% endif %}


        </li>
    {% endfor %}
    </ul>
  </li>
  {% endfor %}
</ul>
