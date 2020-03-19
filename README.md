Data Definition Specification
=============================
These are some notes of mine on musings of how to model structured data.

I've been griping about [proto3][1] and the ways in which I dislike using it to
describe structured data.  At my job we use it and also a variant of [XDR][2].
Previously I've used [ASN.1][3] as made [compatible][4] with [XSD][5].  A
colleague mentioned [Avro][6], and its documention mentions [Thrift][7].
Separately, there are schemaless structured data definitions, like [EDN][8],
and of course there's [JSON][9] (if you want a schema with that, there's
[JSON Schema][10]).

Now a [bad idea][11] would be for me to decide that I can do better and then
write yet another data definition specification.

The exercise is interesting, though, and any resulting discussion with
colleagues will be valuable.  So, here it goes.


Scalar Types
------------

Name      | Meaning
----      | -------
decimal   | decimal128, IEEE 754
integer   | decimal restricted to integers
natural   | decimal restricted to non-negative integers
--------- | ------------------------------------------------------------------
boolean   | 0 or 1
--------- | ------------------------------------------------------------------
bytes     | raw memory, prefixed by a natural size
unicode   | bytes restricted to valid UTF-8
--------- | ------------------------------------------------------------------
duration  | integer number of nanoseconds
instant   | duration relative to the Unix Epoch (UTC)
period    | an instant and a duration of time afterward
          |     TODO: not quite right, since durations can be negative
--------- | ------------------------------------------------------------------
gregorian | particular YYYY-MM-DD on the Gregorian calendar
oclock    | natural number of nanoseconds since midnight, less than 8.64e+13
          |     NOTE: there are 8.64e+13 nanoseconds in a calendar day
--------- | ------------------------------------------------------------------
uuid      | Universally Unique Identifier, RFC 4122
 

Compound Types
--------------

Category    | Meaning
--------    | -------
array       | zero or more values having a particular type
record      | a fixed number (zero or more) of named fields, each with a type
            |     TODO: figure out how express optional fields
            |           (maybe look at [clojure.spec][12])
union       | exactly one from among one or more fields, each with a type
            |     TODO: have to decide whether the alternatives are named
enumeration | one or more named, distinct integers
map         | zero or more pairs of string-labeled fields having the same type
set         | an order-irrelevant array of distinct values


Equality
--------
TODO


Ordering
--------
TODO


Hashing
-------
TODO


JSON Representation
-------------------
TODO


Schema Language
---------------
TODO


Inter-Schema Value Migrations
-----------------------------
TODO (this is the important part)
- a tree of conversion functions?
- a "migration transformer" DSL?


Serialization Test Protocol
---------------------------
TODO (probably using the schema language and JSON representation as an
implementation-agnostic *lingua franca*)


[1]: https://developers.google.com/protocol-buffers
[2]: https://en.wikipedia.org/wiki/External_Data_Representation
[3]: https://en.wikipedia.org/wiki/Abstract_Syntax_Notation_One
[4]: http://xml.coverpages.org/ASN1toXMLSchemaWhitePaper.pdf
[5]: https://en.wikipedia.org/wiki/XML_Schema_(W3C)
[6]: https://avro.apache.org/docs/current/
[7]: https://thrift.apache.org/static/files/thrift-20070401.pdf
[8]: https://github.com/edn-format/edn
[9]: https://www.json.org/json-en.html
[10]: https://json-schema.org
[11]: https://xkcd.com/927/ 
[12]: https://clojure.org/guides/spec

