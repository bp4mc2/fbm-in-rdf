# Comparing FBM and RDF

## Introduction

RDF is in essence a way to store data using IRI's and literals in triples. FBM is a way to model data starting from facts. 
So a priori these are two concepts dealing with data in a complete different ways.  Where is de link? 
Well there are many:
1. We could store a model made with FBM as RDF.
2. We could store data that is modelled with FBM as RDF. 
3. We could model data with FBM but also with some combination of upper ontologies as RDFS, SKOS, OWL, SHACL and DCAT. 
In this article we choose for the latter and work our way up to the first. But first we elaborate on the concept of data in both contexts.

## Comparing the Basic Principles of FBM and RDF 

RDF is designed to work for computers. 
In RDF data consists of triples. Each triple is a list of three parts. The first part is called the subject; the second part is called the predicate and the last part is called the subject of the triple.
Subject and predicate must be IRI's. The object can be an IRI or a literal. A `literal` is a date, integer, real, number, string, text etc.

An `IRI`, short for `Internationalized Resource Identifier` is a string formatted as described in [RFC2396](https://www.rfc-editor.org/rfc/rfc2396). An IRI is supposed to denote something, that 'something' being a resource. This resources can be a location on the Web in which case the IRI is an URL, a Uniform Resource Location.
IRI's are intended to be used and interpreted by computers. Browsers can use inputted URL's to return the content of the location. Some URI's are 'dereferencable', meaning that there is a (standard) procedure of converting it to an IRI of an Information Resource which can be returned to the supplier of the original URI. 
A resource can be anything that has identity [RFC2396](https://www.rfc-editor.org/rfc/rfc2396). It can be a location on the web, a fysical object (me, the person  who is writing this) or the abstract concept Person.  An Information Resource is a Resource ...

A triple can be seen as a declarative sentence. The triple `_:John _:worksFor _:WalMart`, where every string starting with '_:' is an IRI, could be a machine readable counterpart of declarative sentence "John works for WalMart". 

FBM, especially FCO-IM, is designed to work for people. It's intention is to be readable for IT layman in specifying the data that's relevant in a domain that's going to be supported by an information system. 
In FBM data is a compact way tot write down the relevant declarative sentences. Starting point are the **human readable and understandable** declarative sentences. How the actual data is stored is of no concern to FBM.
(In the rest of this article we use FCO-IM as a representative of FBM. Mostly because the specifications of that method are free available [here](https://www.fco-im.nl/pdfFiles/FCO-IM%20book.pdf)

FCO-IM is a method that leads to a formal grammar which can be used to produce formal sentences. Every possible relevant declarative sentence in the domain has at least on look-a-like formal sentence. Furthermore the formal sentences with the same meaning are linked. 
For instance. So if we assume that 'John is married to Ann', 'Ann is married to John' and 'John and Ann are married' have the same meaning then these sentences are linked together. 
If you want to refer to something FCO-IM uses the common technic used for that in natural languages: names of referring expression 
A `referring expression` is any expression used in an utterance to refer to something or someone (or a clearly delimited collection of things or people), i.e. used with a particular referent in mind.
(from: Semantics  a Coursebook) 

A proposition is the meaning of a declarative sentence.


 
