# Identifiers and references

Let's consider the following simple fact:

`John is a Person.`

In this fact, someone who is *named* 'John', is a person. So we might actually say that this is a complex fact:

`Someone named "John" is a person`.

Or what about:

`Someone uniquely identified with number 12345678 is a person`.

Even the most simple fact will contain references to some other facts, using a name or identifier to reference this other fact. We might use names in the verbalisation of the fact, but names might not be unique, so the use of identifiers is need to really refer to a unique fact, consider this:

`Mary is owned by John`

This might be a strange facts, but maybe:

`Mary is a boat`

And:

`Mary is the daughter of John`

So Mary is a name that refers to both the boat and to a person.

In RDF we might state:

```
_:BoatMary _:isOwnedBy _:PersonJohn; #Mary is owned by John
  a _:Boat; #It's a boat;
  rdfs:label "Mary"; #This boat is named "Mary"
  .
_:Mary _:isDaughterOf _:John; #Mary is the daughter of John
  a _:Person;
  rdfs:label "Mary"; #This person is named "Mary"
.
```

So we conclude that:

- You might refer to something as "that one", "it", "She", "He";
- You might refer to something as "that boat, that person";
- You might refer to something by its name.

In all cases, the reference **itself** might include a fact!

As fact based modeling languages cannot deal with this knowledge directly we should understand that by translating facts and fact types to triples, we might need to state more facts and fact types than originally available.
