---
layout: page
title: JSON Schema Glossary
---

This document collects short explanations of terminology one may encounter within the JSON Schema community.

Whilst many of the entries below have precise technical definitions, preference is given to explanations of their conversational use, with additional references linked for further information.
This page is not meant to be [normative](#normative), nor is it meant to contain fully original research or explanation.
It is meant to aid the understanding of those less familiar with formal language used within JSON Schema, or within specifications more broadly.
(In fact, entries below make effort to avoid terminology like "normative" itself for reasons just mentioned.)

If you encounter a term you wish were defined here, please feel free to [file an issue requesting it](https://github.com/json-schema-org/json-schema-org.github.io/issues/new?title=Add%20a%20glossary%20entry%20for%20).

The entries on this page can be linked to via anchor links (e.g. `https://json-schema.org/learn/glossary.html#vocabulary`) when sharing a definition with others.

### dialect

A cohesive collection of [keywords](#keyword) available for use within a schema, often representing a use-case specific single release of the JSON Schema specification.

Dialects, particularly the 2019-09 and 2020-12 dialects, are often defined via a collection of [vocabularies](#vocabulary).

Each dialect is identified by a URI, its *dialect identifier*, which [schemas](#schema) may then reference in their `$schema` [keyword](#keyword).
Doing so identifies the schema as being written in the dialect, and thereby indicates which keywords are usable within it, along with their intended meaning.

The JSON Schema specification defines a number of dialects, each of which enable vocabularies suitable for the dialect's specific use case.
These vocabularies are [described](https://json-schema.org/specification.html#general-purpose-meta-schema) in meta-schemas.

### draft

An individual release of the JSON Schema specification.

JSON Schema drafts are not intended to be provisional documents, as the layman's use of the word "draft" might indicate.

While future drafts may introduce new behavior or changes to existing behavior, each draft is a completed, released document, batching together changes to the specification, and intended for implementation and use.

The current list of drafts can be found [here](https://json-schema.org/specification-links.html#published-drafts).

### JSON

A pervasive data interchange format used for representing and transmitting data as human readable text.
JSON is extremely widely used, and parsers which can read and write it exist for essentially every commonly-used programming language.

JSON Schema, distinctly, is built *on top* of JSON, in that JSON [schemas](#schema) are themselves JSON objects which describe other JSON objects.
The two are, however, entirely different pieces of the conceptual puzzle, with JSON being a concrete format for *representing* data and JSON Schema being a way to *schematize* data which is written in the JSON format.

The JSON format is an open format, with its own [homepage](https://www.json.org/), and specifications published in the [ECMA-404](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) and [RFC-8259](https://datatracker.ietf.org/doc/html/rfc8259) documents from ECMA and the IETF respectively.
In particular, it is not managed or developed by the JSON Schema team, who simply make use of the format.


### keyword

A property appearing within a [schema](#schema) object.

The [JSON Schema specification](https://json-schema.org/specification.html) defines behavior for a large library of keywords which can be used to describe [instances](#instance).

### instance

A piece of JSON data which is to be described by a [schema](#schema).

JSON Schema can be used to describe JSON values of any type (as well as values from many JSON-like formats which can be reasonably represented as JSON).

The JSON Schema specification makes no broad assumptions about the structure of instances themselves beyond those of the JSON specification itself.
In particular it does not reserve any properties within a JSON object for its own use, or require parsers of JSON to support features beyond those already mandated of JSON implementations.

### meta-schema

A [schema](#schema) which is itself intended to describe other *schemas*.

JSON Schema defines a language for describing any [instance](#instance) using a schema written in JSON.
Since schemas are themselves JSON values, they may be also be treated as *instances*, and therefore described by other schemas.

We refer to the schema-of-a-schema as a "meta-schema" to express this use.

### normative

In the context of JSON Schema, and formal specifications more broadly, a document which outlines standardized behavior.
This is as distinct from *non*-normative or informational documents, meant to explain, simplify or offer opinions.

Distinguishing between whether a document is normative or not is intended to clarify to those using the document whether its contents are allowed to contradict or augment behavior described in other normative documents.
JSON Schema's normative documents notably include its [specification](https://json-schema.org/specification.html).
This page for instance, not being a normative document, is not able to proscribe new JSON Schema behavior not already covered by the specification.

#### See also

* [normative](https://developer.mozilla.org/en-US/docs/Glossary/Normative) and [non-normative](https://developer.mozilla.org/en-US/docs/Glossary/non-normative) in the Mozilla Glossary, and its links

### schema

A document, written according to the proscribed structure of the JSON Schema specification, which can be used to describe [instances](#instance).

The rules constituting which schemas are conformant, as well as the rules governing their behavior when validating instances, are defined by the [JSON Schema specification](https://json-schema.org/specification.html).

Strictly speaking, according to the specification, schemas are themselves [JSON documents](#JSON), though it is somewhat common for them to be authored or maintained in other languages which are easily translated to JSON, such as YAML.

In recent [drafts](#draft) of the specification, a schema is either a JSON object or a JSON boolean value.

### subschema

A [schema](#schema) which is itself contained within a surrounding parent schema.
Like schemas themselves, in recent [drafts](#draft) of JSON Schema, subschemas are either [JSON](#JSON) objects or JSON boolean values.

Within the JSON Schema specification and its [dialects](#dialect), a number of [keywords](#keyword) take subschemas as part of their values.
For example, the `not` keyword takes a subschema value and inverts its result, succeeding whenever the subschema does not succeed, such that the [instance](#instance) `12` is invalid under `{"type": "string"}` but valid under `{"not": {"type": "string"}}`, where `{"type": "string"}` is a subschema contained in the full schema.

Some subschemas may appear in more complex nested locations within a parent schema.
The `allOf` keyword, for instance, takes an array of multiple subschemas and succeeds whenever all of the subschemas do individually.

Whether something that otherwise *appears* to be a schema (based on its contents) actually *is* a subschema can be misleading at first glance without context or knowlege about its location within the parent schema.
Specifically, in our above example, `{"type": "string"}` was a subschema of a larger schema, but in the schema `{"const": {"type": "string"}}`, it is *not* a subschema.
Even though as a value it looks the same, the `const` keyword, which compares instances against a specific expected value, does *not* take a subschema as its value, its value is an opaque value with no particular meaning (such that in this schema, the number 12 would be invalid, but the precise instance `{"type": "string"}` is valid).
Said more plainly, whether a particular value is a subschema or not depends on its precise location within a parent schema, as interpretation of the value depends on the defined behavior of the keyword(s) it lives under.

Subschemas may themselves contain sub-subschemas, though colloquially one generally uses the term "subschema" regardless of the level of nesting, further clarifying which larger schema is the parent schema whenever needed.

### vocabulary

A tightly related collection of [keywords](keyword), grouped to facilitate re-use.

A vocabulary is specified by a prose document or specification which explains the semantics of its keywords in a way suitable for implementers and users of the vocabulary.
It often also includes a [meta-schema](#meta-schema) (or multiple metaschemas) which define the syntax of its keywords.

Anyone can create and publish a vocabulary, and implementations generally will include facilities for extending themselves with support for additional vocabularies and their keywords.
The JSON Schema specification includes a number of vocabularies which cover each of the keywords it defines.

In some [dialects](#dialect) of JSON Schema, the `$vocabulary` keyword can be used to include the keywords defined by a vocabulary into the dialect, as well as to indicate whether implementations must specifically recognize the vocabulary in order to be able to process schemas written in the dialect or not.

#### See also

* [`json-schema-vocabularies`](https://github.com/json-schema-org/json-schema-vocabularies), a repository which collects known third-party JSON Schema vocabularies
