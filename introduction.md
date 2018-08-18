## What is Hibernate?
* An ORM tool
    * What is ORM?

* Used in data layer of applications.
* Implemenets JPA -- Define


## Whats is the problem Hiberante is trying to solve?

| User Class |          
|------------|
|     id     |
|    name    |
|   address  |
|    phone   |
|     dob    |

| id | name | address | phone | dob |
|----|------|---------|-------|-----|
|    |      |         |       |     |
|    |      |         |       |     |
|    |      |         |       |     |
|    |      |         |       |     |

A object of a user class corressponds to a row in the table.
Normally we use JDBC and an SQL insert query to persist the user object.
To fetch we use SELECT query.

The problem here is that we have objects in Java but we don't have objects in DB , so we have to use some biolerplate code which takes each  and exvery property of the object and maps it with a SQL query.
The problem is that we need to map each field in insert Query and
To fetch all records we have to convert ResultSet into Java Object.
This is to be done for each and every object that interactes with DB.


## The Problem
* Mapping member variables to columns.
* Mapping relationships
* Handling Data Types.
* Managing changes to the user state.



Gap between OO Design and ORM design.

Suppose we have 5 users and 5 correspoding tables, so most of the time we wont worry about establishing relationship between objects.

e.g a user has a particular address.
i.e A user row will have a row in address table and mapped by a foreign key.

So ideally to respreset this relationsip as an object then i would have to associate an Address object with User object. Now since this is a pain establishing this relationship I would keeo objects seperate and establish the relatioship based on keys even in the JAVa layer.