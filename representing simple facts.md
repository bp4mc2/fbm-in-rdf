#Representing simple facts

## Facts, Fact types and Entity types

A simple fact is a fact that can be represented as a sentence with only a subject, a predicate and an object. For example:

1. `[John] [works for] [Wallmark].`
2. `[John] [is a] [Person].`
3. `[Wallmark] [is a] [Company].`

These simple facts can be translated directly into RDF triples:

1. `_:John _:worksFor _:Wallmark.`
2. `_:John _:isA _:Person.`
3. `_:Wallmark _:isA _:Company.`

The first fact corresponds to the fact type that can be verbalises as:

`Person works for Company`

We know that this verbalisation is correct, because John is a Person and Wallmark is a Company. But this "knowledge" is not actually in the representation of the facts above. A more correct representation of the facts that inhibit this knowledge is presented below:

1. `_:John _:worksFor _:Wallmark.`
2. `_:John a _:Person.`
3. `_:Wallmark a _:Company.`

We use the `a` predicate (short for `rdf:type`) as a way to state that these two facts are examples of a specific kind of fact type (called an entity type) that simply states that there is such a thing as a `Person` and a `Company` in our domain, So we can verbalise these entity types as:

2. `There is such a thing that we call a Person`
3. `There is such a thing that we call a Company`

## Representing entity types in RDF

Representing an entity type in RDF is easy, as this already following from the facts:

```
_:John rdf:type _:Person.
∧
rdf:type rdfs:range rdfs:Class.
⇒
_:Person a rdfs:Class.
```

The first fact is from our example, the second fact is a fact from the RDFS ontology itself (see: [section 3.3](https://www.w3.org/TR/rdf-schema/#ch_type)), the third fact is a derived fact (using the formal knowledge of the RDFS ontology) from the first two.

So we can represent the two Entity types of our example as:

2. `_:Person a rdfs:Class`: There is such a thing that we call a Person - this thing can be classified as a Person
3. `_:Company a rdfs:Class`: There is such a thing that we call a Company - this thing can be classified as a Company

## Representing simple fact types in RDF

The challenge with RDF, is that we have identified the triple als the simple fact. So the fact type should be targeting the triple itself, not one of its constituents (subject, predicate, object). We might state that a simple fact in RDF is actually a statement that we might also formulate as:

```
_:JohnWorksForWallmark a rdf:Statement;
  rdf:subject _:John;
  rdf:object _:Wallmark;
  rdf:property _:worksFor
.
```

The corresponding "Shape" for this statement can be considered the corresponding fact type:

```
_:PersonWorksForCompany a sh:NodeShape;
  sh:property [
    sh:path rdf:subject;
    sh:class _:Person;
  ];
  sh:property [
    sh:path rdf:property;
    sh:hasValue _:worksFor;
  ]
  sh:property [
    sh:path rdf:object;
    sh:class _:Company
  ];
.
```

This is not a very satisfying conclusion, as this shape doesn't target our original triple, but only the reification of this triple as an rdf:Statement. It does however makes it easy to extend this to complex fact types and also the inclusion of roles.

## Roles and fact types

Some languages for Fact Based Modeling include the notion of "role". A role is de identification of the role some thing has in a fact. For example:

`{Employee} [John] [works for] {Employer} Wallmark`

In this example `Employee` is the role of `John` in the fact type `Person works for Company` and `Employer` is the role of `Wallmark`. This notion of role translates very well in the rdf:Statement representation of the fact:

```
_:JohnWorksForWallmark a rdf:Statement;
  rdf:employee _:John;
  rdf:employer _:Wallmark;
  rdf:property _:worksFor
.
```

And the corresponding shape:

```
_:PersonWorksForCompany a sh:NodeShape;
  sh:property [
    sh:path rdf:employee;
    sh:class _:Person;
  ];
  sh:property [
    sh:path rdf:property;
    sh:hasValue _:worksFor;
  ]
  sh:property [
    sh:path rdf:employer;
    sh:class _:Company
  ];
.
```

## Verbalisation and attribution

But how can we solve the problem of specifying the original triple? The "solution" is the special relationship between the subject, the predicate and the object. We have stipulated that a simple fact is a fact with only a subject, a predicate and an object. So the fact type actually *has* direction: the order in which the two entity types are used in the fact type is not random, it has a specific meaning.

Although fact based modeling doesn't have explicit attribution, the special case of simple facts have attribution: the simple fact states a fact *about* the subject. We can attribute the simple fact to the subject, so we might state:

_:PersonWorksForCompanyAttribution a sh:NodeShape;
  sh:targetClass _:Person;
  sh:property [
    sh:path _:worksFor;
    sh:class _:Company
  ]
.

## Terminology and Fact types

Facts are rooted in the meaning of terms. So we should "link" all individual elements of a fact type to term.

### Entity types

For entity types, this is easy: we can directly link the entity type to a term:

```
_:Person a rdfs:Class;
  rdfs:label "Person";
.
```

We could do "punning": stating that `_:Person` is both a `rdfs:Class` and a `skos:Concept`. This might create problems, because `skos:Concept` itself is an subclass of `rdfs:Class`, so `_:Person` will be both a fact *and* a fact type. We can avoid this problem by adding another level of abstraction, using `dct:subject`:

```
_:PersonEntityType a rdfs:Class;
  rdfs:label "Person";
  dct:subject _:PersonConcept
.
_:PersonConcept a skos:Concept;
  skos:prefLabel "Person"
.
```

### Simple fact types

If we consider the role version, we have 4 terms that might need to be added:

1. The fact type 'Person works for Company';
2. The predicate 'works for';
3. The role 'Employee';
4. The role 'Employer'.

We would argue that the fact type itself doesn't require a term to be defined: term 'Person works for Company' doesn't mean anything different than the fact type itself: it is what it is formally defined to be: the fact type! So we only have to define its constituencies. As we have already defined the subject and object (as Entity types), we only have to define the predicate and the roles.
