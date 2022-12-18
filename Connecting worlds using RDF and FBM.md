# Introduction
One of the main challenges that we are encountering in the realisation and usage of IT systems are the different groups who all work with the systems, but all with their own way of talking about the system.
The business-users have their business language talking about their business. So they could say: "We've lost a customer". This language is used to form the IT-systems. But the other way around is also possible. The 'language' of the system is entering the business domain.
The software engineers use a language that looks similar to the business language, but there are some - very important - differences. E.g. they can say: "This feature deletes the customer." Can you imagine busines people saying that? 
The designers are somewhere in between. They have to use the words in both meanings. But they are not always aware of that. 

Another challenge is to use data from another application. Most of the time there isn't a good description of the data in the database. Sometimes there are some conceptual or semantical datamodels, but most of the time they are out dated. The best you have are screen prints, report formats, XSD's and database-schema's. Nobody has a clue how these things are connected. Let alone what all this exacty means. 

In this note we present a way to connect these different worlds in a comprehensible **and operational** way so it's always clear what the meaning is of the data. 
# Main idea 
We start with a picture of the main idea. 
![Using FBM and RDF to connect 'Worlds'](https://user-images.githubusercontent.com/48671644/208295582-61a4417f-3f62-4eae-9801-31e3f2292b55.png)

To describe the data we use `fact types` and `object types`as meant in FCO-IM. For instance we've got the `object types` *Student* an *Teacher* and the `fact type` *Mentorship*. With these types come their expressions:
-  "student \<first name\> \<surname\>"
-  " \<teacher code\>"
-  "the mentor of \<Student:O1\> is \<Teacher:O2\>".

Every type has its counterpart in 'business language' with results in the 'SKOS concepts' *Student*, *Teacher* and *Mentorship*. Every concept has a definition which in plain text describes how it can be deduced that in a given real situation tou are observing an instance of this concept. So we get:
- A **student** is a person whose enrollment at the school has been accepted.
- A **teacher** is an *employee* of the school who is authorized to teach. 
- The **mentor of *student-x* is *teacher-x*** if ***teacher-x*** has accepted ***student-x***'s mentor application. 

As you can see the definitions use terms that have been defined on their own. Also terms can be introduced (*employee*) that aren't important for the datastructure, but are important for the meaning of the concepts. Furthermore the definitions are as 'objective' as possible and in case of a fact type expression contain _exact the same_  variables introduced in the definiendum. 

On the other side we got something similar. For every database that contains data of a specific fact type there is a 'service': *Student*, *Teacher* and *Mentorship*. 

Suppose that we've got an RDBMS that contains (some of) the data. Then we get:
```
RDBMS-STUDENT-service:
SELECT DISTINCT FNAME AS [first name], SNAME AS surname
FROM STUDENT . 

RDBMS-TEACHER-service:
SELECT DISTINCT CODE AS [teacher code]
FROM TEACHER .

RDBMS-MENTORSHIP-service:
SELECT DISTINCT FNAME AS [first name], SNAME AS surname, MENTOR AS [teacher code]
FROM STUDENT . 

```

If the data is stored in a (rdf) graph database we could get something like this:
```
RDF-STUDENT-service:
SELECT DISTINCT ?firstname AS (first name), ?surname AS (surname)
WHERE ?Student a ex:Student ;
          ex:firstName ?firstName ;
          ex:surname ?surname ;
          . 

RDF-TEACHER-service:
SELECT DISTINCT ?code AS [teacher code]
WHERE ?Teacher a ex:Teacher ;
          ex:teacherCode ?code ;
          .

RDF-MENTORSHIP-service:
SELECT ?Student ?Teacher 
WHERE ?Student ex:hasMentor ?Teacher. 

```

As you can see, there are no limitations on the data structures that can be used. 

In the next paragraphs we explore on the relations with UML, XSD, SHACL and OWL. 

