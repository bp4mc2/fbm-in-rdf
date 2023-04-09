# Value type

A value type in fact based modeling, is a fact type about certain facts that are only values by themselves, for example:

1. `"2022-11-13" is a Date.`
2. `"Amsterdam" is a Piece of text`
3. `"25.12" is a number`

The facts above correspond to simple value types. You can also have complex value types, for example:

4. `"25.12 euro" is a currency value`
5. `"Kerkstraat 10, Amsterdam" is an address`
6. `"POINT(486592 120733)" is a geospatial coordinate`

Value types can only be identified by their value, if the value changes - you get something else.

## Representing value-types in RDF

A simple value type can in most cases be represented by a rdf:Datatype:

1. `"2022-11-13"^^xsd:date`
2. `"Amsterdam"^^xsd:string`
3. `"25.12"^^xsd:number`

Most fact based modeling languages have some pre-defined value types you can use, these should be matched with the predefined values types in RDF (in most cases these are xsd datatypes).

When values types are restricted, for example a value type with only the natural number 1 to 10, we might need a sh:PropertyShape:

```
_:Numbers1To10ValueType a sh:PropertyShape;
  sh:datatype xsd:integer;
  sh:minInclusive 1;
  sh:maxInclusive 10;
.
```

For complex value types, we need a sh:NodeShape:

```
_:AddressValueType a sh:NodeShape;
  sh:property [
    sh:name "street";
    sh:datatype xsd:string
  ];
  sh:property [
    sh:name "number";
    sh:datatype xsd:integer;
  ];
  sh:property [
    sh:name "city";
    sh:datatype xsd:string
  ];
```

Please notice that in this complex value type, we **do not** consider the address a complex fact type (which might have a role played by the entity type City), but **only** a complex value type!

So we might say something like:

`John lives at Kerstraat 10, Amsterdam`

Which translates to triples as:

`_:John _:livesAt [_:street "Kerstraat"; _:number 10; _:city "Amsterdam"].`
