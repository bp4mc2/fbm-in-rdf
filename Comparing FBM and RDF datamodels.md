# Comparing FBM and RDF

## Introduction

RDF is in essence a way to store data using IRI's and literals in triples. FBM is a way to model data starting from facts expressed as readable sentences. 
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

An `IRI`, short for `Internationalized Resource Identifier` is a string formatted as described in [RFC2396](https://www.rfc-editor.org/rfc/rfc2396). An IRI is supposed to denote something, that 'something' being a resource. A resource can be anything that has identity [RFC2396](https://www.rfc-editor.org/rfc/rfc2396). It can be a location on the web, a fysical object (me, the person  who is writing this) or the abstract concept Person. In case that the resource is a location on the Webthe IRI is an `URL`, a `Uniform Resource Location`.
IRI's are intended to be used and interpreted by computers. Browsers can use inputted URL's to return the content of the location. Some IRI's are 'dereferencable', meaning that there is a (standard) procedure of converting it to an URL or a IRI of an `Information Resource' which can be returned to the supplier of the original URI. 
 An 'Information Resource' is a Resource that is completely determined by it's digital representation. 

A triple can be seen as a declarative sentence. The triple `_:John _:worksFor _:WalMart`, where every string starting with '_:' is an IRI, could be a machine readable counterpart of declarative sentence "John works for WalMart". 

FBM, especially FCO-IM, is designed to work for people. It's intention is to be readable for IT layman in specifying the data that's relevant in a domain that's going to be supported by an information system. 
In FBM data is a compact way tot write down the relevant declarative sentences. Starting point are the **human readable and understandable** declarative sentences. How the actual data is stored is of no concern to FBM; data is seen as a set of (abstract) `facts` of some `fact type`. 
(In the rest of this article we use FCO-IM as a representative of FBM. Mostly because the specifications of that method are free available [here](https://www.fco-im.nl/pdfFiles/FCO-IM%20book.pdf)

FCO-IM is a method that leads to a formal grammar that can be used to produce formal `declarative sentences`. Every possible relevant declarative sentence in natural language in the domain has at least one look-a-like formal sentence. Furthermore the formal sentences with the same meaning are linked. 
So if we assume that 'John is married to Ann', 'Ann is married to John' and 'John and Ann are married' have the same meaning, then these sentences are linked together by saying that they represent the same fact represented as 'Marriage { (Husband, John), (Wife, Ann) }'  . 

If you want to refer to something, FCO-IM uses the common technic used for that in natural languages: a name or a `referring expression`.

> A `referring expression` is any expression used in an utterance to refer to something or someone (or a clearly delimited collection of things or people), i.e. used with a particular referent in mind. (from: Semantics a Coursebook).

FCO-IM assumes that changing the referring expressions in a `declarative sentence` has no effect on the meaning of that sentence. (Which is true in most cases, but not for `translations`). Furthermore the 'Sense' of a declarative sentence is supposed to be known if the referees are unknown but you know the type of the referees. So the sense of the declarative sentence 'a person is married to a person' is supposed to be known (and in some way equal to the sense of 'John is married to Ann') .

> To turn from reference to sense, the SENSE of an expression is its place in a system of semantic relationships with other expressions in the language. The first of these semantic relationships that we will mention is sameness of meaning, an intuitive concept which we will illustrate by example. (from: Semantics a Coursebook).

For declarative sentences with the same syntactic structure there are production rules containing `Non-Terminals (NT)` in the places where a referring expression is expected. A Fact Type can be seen as a production rule in an abstract synax. The rule contains a 'Predicate' and 1 or more 'Roles': a Role for every NT in the concrete syntax. 

There are special Fact Types that state the existence of an object of an implied certain Object Type, so called `existence postulating fact expressions`. 
They state the existence of an identifiable thing. e.g. 'there is a person called John'. 
Notice that 'John is married to Ann' implies 'there is a person called John'. 

----
Because an abstract syntax describes a graph the mapping of FCO-IM cannot be that complex. A first guess.

```
\# level 0 - general upper ontology vocabular

owl:Class a owl:Class .
rdf:Property a owl:Class
owl:ObjectProperty rdfs:subClassOf rdf:Property .
owl:DataProperty  rdfs:subClassOf rdf:Property .
rdfs:label a owl:DataProperty .
# and all dataypes from xsd:  ...
skos:Concept a owl:Class .
dct:subject a owl:ObjectProperty .

\# level 1 - fco-im vocabular

fcoim:FactType rdfs:subClassOf owl:Class.
fcoim:ObjectType rdfs:subClassOf fcoim:FactType.
fcoim:Role rdfs:subClassOf rdf:Property .
fcoim:Fact  rdfs:subClassOf owl:Class .
fcoim:FactTypeExpression a owl:Class . 
fcoim:FactTypeExpression-LTL a owl:Class ; rdfs:subClassOf fcoim:FactTypeExpression . #LTL is Label Type Level 
fcoim:FactTypeExpression-OLT a owl:Class ; rdfs:subClassOf fcoim:FactTypeExpression . #OTL is Object Type Level
fcoim:ObjectTypeExpression a owl:Class .

fcoim:ote  a owl:ObjectProperty .
fcoim:fte  a owl:ObjectProperty .
fcoim:role  a owl:ObjectProperty .
fcoim:expression a owl:ObjectProperty .

\# level 2 - domain vocabular

## Student
ex-ont:Student a fcoim:FactType , fcoim:ObjectType ;
   fcoim:ote ex-ont:OteStudentFnSn ;
   fcoim:fte ex-ont:ExistsStudentFnSn ;
   fcoim:role ex-ont:roleStudentFn , ex-ont:roleStudentSn ;
   dct:subject ex-concept:Student ;
   . 
ex-ont:OteStudentFnSn a fcoim:ObjectTypeExpression ; 
   fcoim:expression ("student" ex-ont:roleFirstname ex-ont:roleSurname ) ;
   .
ex-ont:ExistsStudentFnSn a fcoim:FactTypeExpression ; 
   fcoim:expression ("there is a student" ex-ont:roleFirstname ex-ont:roleSurname ) ;
   .
ex-ont:roleFirstname a fcoim:Role ;
   rdfs:label "firstname" ;
   rdfs:range xsd:string ;
   . 
ex-ont:roleSurname a fcoim:Role ;
   rdfs:label "surname" ;
   rdfs:range xsd:string ;
   . 
## Teacher    
ex-ont:Teacher a fcoim:FactType , fcoim:ObjectType.
   fcoim:ote ex-ont:OteTeacherCode ;
   fcoim:fte ex-ont:ExistsTeacherCode ;
   fcoim:role ex-ont:roleTeacherCode ;
   dct:subject ex-concept:Teacher ;
   .
## ....

## Mentorship
ex-ont:Mentorship a fcoim:FactType .
   fcoim:fte ex-ont:FteTeacherMentorsStudent ;
   coim:role ex-ont:roleMentorship1 , ex-ont:roleMentorship2 ;
   dct:subject ex-concept:Mentorship ;
   .
ex-ont:FteTeacherMentorsStudent  a fcoim:FactTypeExpression ; 
   fcoim:expression ("the mentor of "ex-ont:roleMentorship1 "is" ex-ont:roleMentorship2 ) ;
   .  
ex-ont:roleMentorship1 a fcoim:Role ;
   rdfs:label "Student:O1" ;
   rdfs:range ex-ont:Student ;
   .
ex-ont:roleMentorship2 a fcoim:Role ;
   rdfs:label "Teacher:O2" ;
   rdfs:range ex-ont:Teacher ;
.

ex-concept:Mentorship a skos:Concept ;
   skos:definition """a Teacher is the mentor of a Student if the Student has 
   requested mentorship from the Teacher and the Teacher has accepted the request."""@en . 
   ## definitions should be objectively verifiable (in the domain that is communicated about) .  
   
ex-data-rule:Mentorship a sh:SPARQLRule ; 
  ## een regel om de logische triples af te leiden uit de data opslag 
  ## (en die kan zowel in triples, tuples of records zitten. ##
  sh:construct """
  CONSTRUCT { #hoe maak je een uri???# 
      ?mentorship a ex-ont:Mentorship ;
                  rdfs:label #de regel mbv de expression het label af te leiden # ;
                  ex-ont:roleMentorship1 ?student ;
                  ex-ont:roleMentorship2 ?teacher ;
                  . }
   WHERE {
       ?student a ex-ont:Student ;
                ex-ont:firstname  ?firstname  ;
                ex-ont:surname ?surname ;
                ex-ont:mentor ?teacher  ;
                .
       ?teacher a ex-ont:Teacher ;
                ex-ont:code ?teacherCode  ;
                .
          }
   """" ; .

\#level 3 - facts - logical level - on this level inferencing is applied

ex:y a ex-ont:Mentorship ;
   rdfs:label "the mentor of student Peter Johnson is BLC" ;
   ex-ont:roleMentorship1 ex:x ;
   ex-ont:roleMentorship2 ex:z ;
.
ex:x a ex-ont:Student ;
   ex-ont:roleFirstname "Peter" ;
   ex-ont:roleSurname "Johnson";
   rdfs:label "student Peter Johnson" ; #label is generated using de OTE .
   .
ex:z a ex-ont:Teacher ;
   ex-ont:roleTeacherCode "BLC" ;
   rdfs:label "BLC" ;
   .

\#level 4 - data - storage level - the data is interpreted to get the facts of level 3

ex:x a ex-ont:Student ;
   ex-ont:firstname  "Peter" ;
   ex-ont:surname "Johnson";
   ex-ont:mentor ex:z .
ex:z a ex-ont:Teacher ;
   ex-ont:code "BLC" ;
   .


```
Visual
 
![FBM en RDF vbld](https://user-images.githubusercontent.com/48671644/206912188-2231056e-581e-4f27-9953-d385f9f5bedc.png)

![FBM en RDF vbld facts](https://user-images.githubusercontent.com/48671644/206912222-2c8e9142-1ce4-4f53-841f-e5ee12b65686.png)

toDo:
- a lot of IRI's have to be generated . What are the rules?
-  "there is a student <first name> <surname>." and  "there is only one student <first name> <surname>." are not the same. FCO-IM treats all OTE as the latter. Or not?
- een Fact Type omvat/bestaat uit of In een Fact Type herken je de betrokken object typen én het predicaat. in de betekenis van het predicaat zijn de rollen opgenomen. trouwen depends on persoon.
- how do we handle two name/code systems for the same objecttype? 
('the country with  ISO2 code <ISO2countrycode> is te same as  <ISO countryname>' ?? )
- A proposition is the meaning of a declarative sentence. Fact = Proposition ??

