#Representing complex facts

## Facts, Fact types and Entity types

A Complex fact might be:

`From [januari to march] John worked for Wallmark as a Sales person`

Using roles this is:

`From {Period} [januari to march] {Employee} [John] worked for {Employer} [Wallmark] as a {Job title} [Sales person]`.

This fact can be respresented, using the following triples:

```
_:SomeFact a _:WorksForFact;
  _:period _:JanuariToMarch;
  _:employee _:John;
  _:employer _:Wallmark;
  _:jobTitle _:SalesPerson;
.
```

As you can see, we can directly create a SHACL shape to capture the Facttype:

```
_:WorksForFactType a sh:NodeShape;
  sh:property [
    sh:path _:period;
    sh:class _:Period;
  ];
  sh:property [
    sh:path _:employee;
    sh:class _:Person;
  ];
  sh:property [
    sh:path _:employer;
    sh:class _:Company;
  ];
  sh:property [
    sh:path _:jobTitle;
    sh:class _:JobTitle;
  ];
.
```

Although the shape above seems logical, it contains some knowledge we didn't state. What are "januari to march" and "Sales person". As with `Person` and `Company`, we might think that these are Entity types. But you can't identify Periods and Sales Persons, other than by their value alone. In Fact Based Thinking, these are special type of Facts are called "Value types".

### Attribution

Attribution of a complex fact type is not as simple as the attribution of a simple fact type. We need to decompose the complex fact type into simple fact types without losing its meaning.

This means: adding context (!). We had the original statement:

`[John] works for [Wallmark]`

But we can also say that:

`[John] is a sales person` (in the context of the fact type)

And:

`[These facts] are true in the period [januari to march]`

So we might stipulate:

```
_:SomeFact {
  _:John _:worksFor _:Wallmark;
  _:John a _:SalesPerson;
  _:SomeFact _:isValid _:JanuariToMarch
}
```

We still have work to do:

- What about "Job title"?
- What about Employee and Employer?

## Title, terms and classification

A Job title is a title, or a term that references some other thing. So we might state:

`[John] has {Job title} [Sales person]`

We created a simple fact, adding the `has` part to predicate. We could state that:

`_:SalesPerson a _:JobTitle.``

But in this case, Job title is not an entity type, it's a value type. So maybe we could state that:

```
_:SalesPersonValue a _:JobTitle.
_:JobTitle rdfs:subClassOf skos:Concept.
=>
_:SalesPersonValue a skos:Concept.
```

This is quite interesting, because we have now created the possibility of classification by attribution:

```
[] a _:SalesPerson <=> [] _:hasJobTitle _:SalesPersonValue.
```

## Roles and attribution

We can do the same thing with roles:

```
_:1 _:worksFor _:2 => _:1 _:employer _:2.
_:1 _:employer _:2 <=> _:2 :employee _:1.
```
