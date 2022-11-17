# Representing Fact Based Modeling in RDF

Fact Based Modeling is a type of conceptual modelling using fact types as the prime instrument. Although Fact Based Modeling has been around since the nineteen seventies, a single definition or formalization of the language used to make a Fact Based Model does not exists. Even a wikipedia for Fact Based Modeling doesn't exist, although wikipedia pages do exist for the individual dialects that are developed over the years. For example: [ORM](https://en.wikipedia.org/wiki/Object-role_modeling), [FCO-OIM](https://en.wikipedia.org/wiki/FCO-IM) and [CogNIAM](https://en.wikipedia.org/wiki/Cognition_enhanced_Natural_language_Information_Analysis_Method).

In the paper [Towards Key Principes of Fact Based Thinking Methods and Protocols](https://www.researchgate.net/publication/330922696_Towards_Key_Principles_of_Fact_Based_Thinking_Methods_and_Protocols), Stijn Hoppenbrouwers, Erik Proper and Maurice Nijssen go a step further and imply that, although Fact Based Modelling hasn't become mainstream, many conceptual modellers follow (most of) the principes of Fact Based Thinking.

Semantic modeling, using the RDF web standards like RDFS, OWL, SKOS and SHACL, is a type of conceptual modelling that has become mainstream, as it is taken up by all the major players in the web community, like Google, Microsoft, Facebook and Amazon. IT research companies like Gartner and Forrester have predicted the need for semantically rich data structures like knowledge graphs and the need for semantically augmented data catalogs. Many governmental and commercial organizations are investigating how they can incorporate semantic modelling in their Data Management strategies.

Creating a semantic model is not that hard. But creating a good semantic model is not an easy task. This research repository investigates how a Fact Based Thinking approach might help to create good semantic models. And at the same time, we will create a formal language for fact based modelling, using *only* the available RDF web standards. In this way, we might pave the road for a mainstream adoption of (at least) Fact Based Thinking.

## Principes of Fact Based Thinking

In the paper mentioned in the introduction, ten principes of Fact Base Thinking are address. We will repeat these principes below, and link them to the corresponding concepts of the Semantic Web.

1. *Fact are first citizens*. This principe can be linked to the RDF data model in which triples are first citizens;
2. *Models should be evidenced based*. This principe can be linked to the RDF data model in which Resources represent actual things in a domain;
3. *Models should reflect the communication about a domain*. This principe can be linked to the Linked Data principle that the web is about information communication;
4. *Models should embed and use the language used by domain experts*. This principe can be linked to vocabulary-principe of both SKOS and OWL standards;
5. *The use of elementary facts is strongly favoured*. As a triple is the most elementary of facts possible, this is actually something that is not only core to the idea of RDF, but might actually be one of the shortcomings of RDF we need to tackle, as more complex facts are harder to express directly in RDF.
6. *Domain knowledge representation is the primary use*. This principe holds true for SKOS and OWL as well. The interesting fact is that this *can* hold true for SHACL, and we can use the same SHACL standard to link data models to the knowledge representation, which gives us the best of both worlds in the same package.
7. *No a-priori conceptualisation of attributes*. This principe can hold true for semantic models as well, *although* one can represent attribution as well. This might be an important aspect of RDF models in which it can bridge the gap between Fact Based Thinking and the more mainstream approach of data modelling.
8. *Modelling language and procedure are two sides of the same coin*. This principe is one of the main reasons for this research repository: the semantic modelling community lacks a rigor procedure for semantic modelling.
9. *Term definitions are an integral part of models*.  This principe can be linked to vocabulary-principe of both SKOS and OWL standards.
10. *Variation between specific situations of use*. As semantic modelling is used all over the web, it is clear that this principe holds true for semantic models. However, that does not imply that a single language and metamodel cannot exists. It's all a matter of communication and creating a global (web) community that can adhere to the language presented. A language that is already available as the combination of different semantic standards.

## Research goals

This research repository will give answers to the following questions:

1. How can we represent facts in RDF?
2. How can we represent fact types in RDF?
3. How can we verbalize facts in RDF (the communication of the facts themselves)?

## Structure of this repository

WORK IN PROGRESS
