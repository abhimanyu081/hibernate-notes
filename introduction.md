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

An object of a user class corressponds to a row in the table.
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
It is a very bad solution. because what is happeining on user side is very different form what is haappeinging in the raltional side.
Because of this huge gap between doibng thins in a OO way and doing things in a relational way.

Since we cannot compormise on the relational side people tend to comproimise with the OO design.

So people tends to do solve this problem over the years.
hendce name Object Relational Mapping.

## Saving wihtout Hibernate
We will have to  use JDBC
* JDBC database confguration.
* The Model  Object e.g User object.
* Service method to create object.
* Database design.
* DAO layer to save the method using SQL queries.(Map objects to SQL queries.)

## The Hibernate Way.
* JDBC database confguration - Hibernate Cinfiguration.
* The Model  Object e.g User object - Annotations.
* Service method to create object - Use Hibernate API.
* Database design - Not Needed.
* DAO layer to save the method using SQL queries - Not needed. (Service layer directly calls the Hibernate APIs to then Hibernates handles all.)


## Step 1 : Configure Hibernate
 Hibernate is configured using an xml file called hibernate.cfg.xml.  This is the default file name for hibernate configurations. If we give a different name to the configuration file then we would need to pass the hiberntae config file name so that hibernare can now where to find the file.

Example hibernate.cfg.xml file for MYSQL.

```

<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
	<session-factory>
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/Book</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">mysql</property>
		<property name="hibernate.connection.pool_size">1</property>
		<property name="hibernate.current_session_context_class">thread</property>
		<property name="hibernate.show_sql">true</property>

		<!-- SQL Dialect. The language hibernate uses to communicate with DB. Although 
			most DB use SQL to communicate but different DBs have different ways of doing 
			things. e.g -->
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>

		<!-- Drop and Re-create the DB schema on startup -->
		<property name="hbm2ddl.auto">create</property>

		<!-- Names the annotated entity class -->
		<mapping resource="book.hbm.xml" />
	</session-factory>
</hibernate-configuration>
```


