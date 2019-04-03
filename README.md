Linked-Data Zero (LD0)
======================


The goal of this project is to provide an easy path to using [Linked Data] for beginners.
More precisely, the goal is to provide languages and tools for beginners to express,
publish and query [Linked Data].

## Motivation

[JSON-LD] is a very good start at making Linked Data easier,
but it is still designed as a *general purpose* language,
aiming to be regular and exhaustive.

LD0 would be a language dedicated to beginners.
* It would not support all the JSON variants that [JSON-LD] supports.
* It would not be able to produce all possible RDF graphs.
* In return, it would require less understanding of the underlying data model.
* Some design choices, if deemed useful for beginners,
  may impede the expressiveness required by more advanced users
  (who are expected to turn to [JSON-LD] or other RDF concrete syntaxes).

[Linked Data]: https://www.w3.org/wiki/LinkedData
[JSON-LD]: http://json-ld.org/

