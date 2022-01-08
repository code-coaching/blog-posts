---
postUuid: 90bdd1f8-10ba-4d53-96cb-3623332a3abc
title: Databases
slug: databases
tags:
  - Databases
categories:
  - Backend
---

Databases are organized collections of data. Database management systems are systems to work with the stored data in a database. Paradigms/models describe how data in a database is stored.

## NoSQL databases

NoSQL is a term for all database models that differ from the relational model. An example of a NoSQL database is a document database.

A person with two cars could be stored as shown below.

```text
{
    "id": "0",
    "firstName": "John",
    "lastName": "Duck",
    "age": 29,
    "sex": "man",
    "cars": [
        {
            "id": "0",
            "brand": "Opel",
            "type": "Manta 400"
        },
        {
            "id": "1",
            "brand": "Ferrari",
            "type": "F50"
        }
    ]

}
```

Examples of document databases are MongoDB, RethinkDB ...

## Relational databases

Relational databases use tables to hold data, where the tables have some relationship to each other. As an example, a person can be represented by a row in a table called `people`. A car can be represented by a row in a table called `cars`. To show that a person is the owner of a car, the car can have a column called `Owner` that refers to a person.

Examples of relational databases are MySQL, MariaDB, PostgreSQL ...

People

| id  | firstName | lastName | age | sex  |
| :-: | :-------- | :------- | :-- | :--- |
|  0  | John      | Duck     | 29  | male |
| ... | ...       | ...      | ... | ...  |

Cars

| id  | brand   | type      | owner |
| :-: | :------ | :-------- | :---- |
|  0  | Opel    | Manta 400 | 0     |
|  1  | Ferrari | F50       | 0     |
| ... | ...     | ...       | ...   |

Looking at the tables it becomes clear that one person can own several cars. The `owner` column contains the `id` of the owner from the table `people`.

A row in a table, is called a record.

### SQL

SQL (Structured Query Language) is a language made to query data. An example to retrieve all first and last names of all persons who are eighteen or older: `SELECT firstName, lastName FROM people WHERE age >= 18`.

### Data normalization

To know which tables to create, data normalization is applied. Normalization ensures that data is stored in the simplest form possible, without storing data twice. In this blog post, normalization is not explained step by step, it is important to know that this process precedes determining which tables to create.

In a company, this is usually done by a software architect or a backend developer.

### ERD

An ERD (Entity Relationship Diagram) is a diagram showing all relationships between different entities/tables. As a frontend developer, it is important that an ERD can be interpreted correctly. Creating an ERD is not covered in this blog post, understanding an ERD is important.

## ERD

An ERD (Entity Relationship Diagram) shows the different relationships between different entities/tables. Representing the relationships can be done using different techniques. In this blog post, the crow's foot notation is used.

Zero or one

![zero or one](/img/blog/zero-or-one.png)

Exactly one

![exactly one](/img/blog/exactly-one.png)

Many

![many](/img/blog/many.png)

One or many

![one or many](/img/blog/one-or-many.png)

Zero, one or many

![zero, one or many](/img/blog/zero-one-or-many.png)

### Opbouw van een ERD

![qownnotes-media-JM5579](/img/blog/erd-creation.en.png)

Each table from a database is represented in an ERD by a table with one column. The header (the dark gray part in the image) of the table (in the ERD) is the collective name for what is stored in the table (in the database). Each column from the table of the database, gets a row in the table of the ERD.

Each row in the table of a database, is called a record. Each row in the table of an ERD, contains the column name of a record from the table of a database.

#### Example

People

| id  | firstName | lastName | age | sex    |
| :-: | :-------- | :------- | :-- | :----- |
|  0  | John      | Duck     | 29  | male   |
| ... | ...       | ...      | ... | ...    |
| 100 | Sylvia    | Duisters | 28  | female |
| ... | ...       | ...      | ... | ...    |

This table stores people. Each person has an "id" by which they can be uniquely identified, a first name, a last name, an age and a gender.

This is represented in an ERD as:

![people table](/img/blog/relation-example.en.png)

<br/>

### One to one relation

An example with exactly one:

<br/>

![exactly one example](/img/blog/exactly-one-example.en.png)

The ERD can be read as:

- a person has exactly one car
- a vehicle has exactly one person as owner

It can also be deduced from the ERD that the relationship is kept in the table of cars in a field called owner.

People

| id  | firstName | lastName | age | sex    |
| :-: | :-------- | :------- | :-- | :----- |
|  0  | John      | Duck     | 29  | male   |
| ... | ...       | ...      | ... | ...    |
| 100 | Sylvia    | Duisters | 28  | female |
| ... | ...       | ...      | ... | ...    |

Cars

| id  | brand   | type      | owner |
| :-: | :------ | :-------- | :---- |
|  0  | Opel    | Manta 400 | 0     |
|  1  | Ferrari | F50       | 100   |
| ... | ...     | ...       | ...   |

<br/>

An example with zero or one:

<br/>

![zero or one example](/img/blog/zero-or-one-example.en.png)

The ERD can be read as:

- a person has zero cars or one car
- a car has exactly one person as owner

From the ERD it can also be derived that the relation is kept in the table with cars in a field called owner. In case a person has no cars, no row will be found in the table with cars for this person.

People

| id  | firstName | lastName | age | sex    |
| :-: | :-------- | :------- | :-- | :----- |
|  0  | John      | Duck     | 29  | male   |
|  1  | Harry     | Potter   | 8   | male   |
| ... | ...       | ...      | ... | ...    |
| 100 | Sylvia    | Duisters | 28  | female |
| ... | ...       | ...      | ... | ...    |

Cars

| id  | brand   | type      | owner |
| :-: | :------ | :-------- | :---- |
|  0  | Opel    | Manta 400 | 0     |
|  1  | Ferrari | F50       | 100   |
| ... | ...     | ...       | ...   |

<br/>

### One to many relation

An example:

![één op veel voorbeeld](/img/blog/one-to-many-example.en.png)

The ERD can be read as:

- a person has zero cars, one car or many cars
- a car has exactly one person as owner

From the ERD it can also be derived that the relation is kept in the table with cars in a field called owner. In case a person has no cars, no row will be found in the table with cars for this person. In case a person has exactly one car, there will be exactly one row in the table of cars for this person. In case a person has multiple cars, there will be multiple rows in the table of cars for this person.

Translated with www.DeepL.com/Translator (free version)

People

| id  | firstName | lastName | age | sex    |
| :-: | :-------- | :------- | :-- | :----- |
|  0  | John      | Duck     | 29  | male   |
|  1  | Harry     | Potter   | 8   | male   |
| ... | ...       | ...      | ... | ...    |
| 100 | Sylvia    | Duisters | 28  | female |
| ... | ...       | ...      | ... | ...    |

Cars

| id  | brand   | type      | owner |
| :-: | :------ | :-------- | :---- |
|  0  | Opel    | Manta 400 | 0     |
|  1  | Ferrari | F50       | 100   |
|  2  | Opel    | Corsa     | 100   |
| ... | ...     | ...       | ...   |

<br/>

### Many to many relation

In the case that a person may have multiple cars and a car may also belong to multiple owners, an additional table is needed to enable this relationship. An intermediate table, also called a pivot table.

An example:

![many to many example](/img/blog/many-to-many-example.en.png)

The ERD can be read as:

- a person has one or more cars
- a car has one person or more persons as owner

From the ERD it can also be deduced that the relationship is maintained in the table called `Owner_Car`.

As an example, we take the people from previous examples. John has an Opel Manta 400. Harry has no car. Sylvia has a Ferrari F50 and an Opel Corsa. As an additional scenario, John and Sylvia bought a Tesla Model S together.

People

| id  | firstName | lastName | age | sex    |
| :-: | :-------- | :------- | :-- | :----- |
|  0  | John      | Duck     | 29  | male   |
|  1  | Harry     | Potter   | 8   | male   |
| ... | ...       | ...      | ... | ...    |
| 100 | Sylvia    | Duisters | 28  | female |
| ... | ...       | ...      | ... | ...    |

Cars

| id  | brand   | type      |
| :-: | :------ | :-------- |
|  0  | Opel    | Manta 400 |
|  1  | Ferrari | F50       |
|  2  | Opel    | Corsa     |
| ... | ...     | ...       |
| 45  | Tesla   | Model S   |
| ... | ...     | ...       |

Owner_Car

| id  | owner | car |
| :-: | :---- | :-- |
|  0  | 0     | 0   |
|  1  | 100   | 1   |
|  2  | 100   | 2   |
|  3  | 0     | 45  |
|  4  | 100   | 45  |
| ... | ...   | ... |

<br/>

## SQL - Structured Query Language

SQL is a standard way of working with data from relational databases.

This is best learned through the application of queries. [w3schools](https://www.w3schools.com) is used for this purpose.

This part is done through self-study. And making the exercises.

Check out the [theory](https://www.w3schools.com/sql) from `SQL Syntax` to `SQL Group By` (in the side menu).

To prepare for the test, take the [quiz](https://www.w3schools.com/sql/sql_quiz.asp) and the [exercises](https://www.w3schools.com/sql/sql_exercises.asp).
The `SQL Database` section of the exercises is not part of the subject matter.

## API

API staat voor Application Programming Interface, dit is een manier om applicaties
met elkaar te laten communiceren. Het meest voorkomende type API bij het
ontwikkelen van webapplicaties is een RESTful API.
Een ander type dat meer en meer voorkomt tegenwoordig, is een GraphQL API.

### Protocollen

Terminology used:

- `client`: A device (computer, smartphone, tablet ...) that is used by a user.
- `server`: A device that is always on which contains files that can be retrieved via a protocol (e.g. HTTP).
- request`: A request made from the client to the server.
- response`: A response sent from the server to the client.

#### HTTP/HTTPS

HTTP stands for *H*yper*t*ext *T*ransfer *P*rotocol. The `S` in HTTP*S* stands for *S*ecure.

The difference between HTTP and HTTPS is that when a password is sent via HTTP, it is in plain text. In case this communication is intercepted, the the password can simply be read by the intermediary. With HTTPS, the password is sent encrypted. If this communication is intercepted, the intermediary sees the encrypted password.

HTTP(S) is a request/response protocol. A request is sent is sent from a client to a server. The server processes this request and sends back a response.

HTTP(S) is unidirectional, which means that the communication is only one way. A client will always send a request, a server will always reply.

##### POST/GET/PUT/DELETE & CRUD

Different types of requests can be sent to the server.
A POST indicates that an item needs to be created.
A GET indicates that an item has to be read.
A PUT indicates that an item has to be changed (Update).
A DELETE indicates that an item should be deleted.

A term that is going to come back more often is `CRUD`. This term means that *C*reate, *R*ead, *U*pdate and *D*elete can be performed.

#### WS/WSS

WS stands for *W*eb*S*ocket.
The `S` in WS*S* stands for *S*ecure.

As with HTTPS, WSS ensures that the communication is encrypted.

WS(S) is bidirectional, this means that both the client can send messages to the server and the server can send messages to the client.

An example of what this can be used for is Instant Messaging.

### RESTful

REST stands for **RE**presentational **S**tate **T**ransfer. This is an architecture that can be followed to build an API.

Suppose there is a domain (e.g. https://thisisfake.com). On the server that this domain points to, there is an API to retrieve all the students.

Then in a RESTful API, it would be as follows:

```
HTTP GET https://thisisfake.com/api/students
```

HTTP indicates that it is over the HTTP protocol, GET indicates the type of the request. The domain name points to the server and `/api/students` is an `end point` of the API. In this case, a GET request to `/api/students` will will return a response containing all the students.

The server will handle the request. One of the possible options is for the server to do a `SELECT * FROM students` and return the result of this query back.

A RESTful API works with POST/GET/PUT/DELETE requests to perform CRUD operations.

### GraphQL

GraphQL is another way to communicate with a server. GraphQL works only with POST requests. In the content that is sent with the POST requests indicate what the request wants the server to do.

### JSON

Communication between a browser and a server can only take place via text. To ensure that complex data can still be sent, JSON is used.

JSON stands for *J*ava*S*cript *O*bject *N*otation.

An object in JavaScript can represent complex data. For example, a person. A person has a first name, last name and age.

```js
const person = { firstName: "Bart", lastName: "Duisters", age: 29 };
```

A JSON object is almost similar to a JavaScript object, but all properties are enclosed in double quotes.

```json
{
  "firstName": "Bart",
  "lastName": "Duisters",
  "age": 29
}
```

Nowadays, JSON is mostly used to exchange data between client and server.
