Thought
=======

## Where to start

[JSON-LD] is a very good start at making Linked Data easier,
and JSON is a popular format among developers,
so this seems like a good starting point.

In the following, unless stated otherwise,
I will make the assumption that LD0 will be based on [JSON-LD],
possibly requiring some pre- or post-processing steps.
In particular, [JSON-LD] requires the data to be explicitly linked to a context.
In LD0, a default context would be implicitly provided.


## Syntax

While JSON is very popular and widely known,
it can be verbose, and annoying in several respects
(no comments, no trailing comma).
So using a variant such as [HJSON] or [YAML] (or possibly [Toml] or [Property list])
could make it better looking
(although less recognizable by some developers).

My sweet point would probably be recommend [HJSON],
but have an option in all documentations to switch to plain JSON.
Differently inclined readers could then chose which they prefer.


## IRIs

At first, people should not have to worry about IRIs,
neither for their vocabulary nor their resource identifiers.
The most natural way, for me, is to resort to relative IRIs.

But I would like people to be able to write plain identifiers,
not encumbered with a leading hash or colon.
So, I would like to be able to write something like
```json
{
    "id": "pchampin",
    "knows": "cygri"
}
```
and that it resolve to the following Turtle
```
<#pchampin> <#knows> <#cygri>.
```
> I'm leaving aside the problem of determining that the value of `knows`
is an identifier rather than a plain string.

This is [not trivial currently](http://tinyurl.com/y4638hhq) in [JSON-LD]
(especially for the `id`)
but some pre- or post-processing should make it possible.

### Alternative: bite the hash-bullet

Alternatively,
we can accept the trouble of prefixing resource IRIs with a hash;
we can relate to fragment identifiers in HTML to justify that.
This would align more smoothly with [JSON-LD] and Linked Data principles.

An added-value would be that properties expecting identifiers (instead of strings)
would be easier to spot:
```
{
    "id": "#pchampin",
    "name": "Pierre-Antoine Champin",
    "knows": "#cygri"
}
```

### Using existing vocabularies

Of coure, using relative IRIs for the vocabulary (properties and types)
should not be mandatory in LD0.
It should still be possible to switch to using an existing vocabulary
(*e.g.* http://schema.org/)
by overriding the default context.

However, may be there should be some limitation on how it can be overridden
(see the corner case in the end of Section *Value typing* below).


## Value typing

One added value of [JSON-LD] over plain JSON is the ability to declare
(in the context)
how the string values of a property should be interpreted.
For example, in
```json
{
  "id": "#pchampin",
  "name": "Pierre-Antoine Champin",
  "joined": "2019-04-03",
  "homepage": "http://champin.net/",
  "knows": "#cygri"
}
```
the [JSON-LD] context would allow to make the following statements:
* `name` expects a string
* `joined` expects a date
* `homepage` and `knows` expect IRIs

In LD0, we could rely on smart heuristics to automatically infer the datatype.
If we accept the "bite the hash-bullet" alternative above,
recognizing local resource identifiers becomes trivial.

If the same property holds values of different (unreconcilable) datatypes,
that would be an error in LD0.

### Interaction with external contexts

Assuming that we automatically cast to `xsd:date` strings of the form YYYY-MM-DD,
what would happen in LD0 with the following data?
```
{
  "@context": "http://schema.org",
  "birthDate": "2019-03-01"
}
```
The definition of `birthDate` in Schema.org states that its values are to be interpreted with the `schema:Date` datatype,
which contradicts the default interpretation (`xsd:date`).
So which one should prevail?

Maybe it is safer to limit LD0 to using `@vocab`
(and possibly prefix definitions),
but not full-fledged contexts, in order to avoid such conflicts.
LD0 would be limited to its own "smart" interpretation;
customized interpretations such as those defined by contexts would require to switch to [JSON-LD].


## Querying

While my initial idea was to extend SPARQL with [JSON-LD] context,
I'm having second thoughts.
The SPARQL syntax looks very unfamiliar to many developers.
Relying on JSON for expressing data,
then switching to something totally foreign for querying it,
does not seem right.

I like very much the idea of [GraphQL-LD].
It uses the [GraphQL] syntax,
but instead of [GraphQL] service description,
it relies on a [JSON-LD] context to translate the GraphQL query into a SPARQL query.
It offers a nice entry point to querying linked data using a JSON-like syntax,
and producing JSON results (but also, possibly, standard SPARQL bindings).




[JSON-LD]: http://json-ld.org/
[HJSON]: https://hjson.org/
[YAML]: https://yaml.org/
[Toml]: https://github.com/toml-lang/toml
[Property list]: https://en.wikipedia.org/wiki/Property_list 
[GraphQL-LD]: https://gist.github.com/rubensworks/9d6eccce996317677d71944ed1087ea6
[GraphQL]: https://graphql.org/
