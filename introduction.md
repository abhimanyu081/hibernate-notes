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

## Step 2: Write Model Class

Why to use javax.* package?


## Step 3 : Create a Service class that instantiates the user object and calls Hibernate API to save the object.



# Steps in Using Hibernate APIs.

1. Create a SessionFactory - Only one object per App.
   what does it do?
    It creates session dependeing upon how many session we want in our App throughout execution period.

    We need to acquire session object to save object to DB. And we get this session object from SessionFactory. SessionFactory is create upfront from the xml configuration file  we have provided.

2. Create session from SessionFactory - Every time we want to intercat with DB we will have to acquire the session object and perform the DB operation.

3. Use the Session to save Model Object. 

```
@Entity(name="USER_DETAILS")
@Column(name="user_name")
Column annotation can be put  on fields as well as on getters.

```

## Few More annotations

`@Table`

When we provide @Entinty(name) we give name to the whole entity but when we give @Table(name) we name only the table. 
**No When would we call Entity Name VS Table Name?**
When we write HQL we dont address Table names but Entity Names.

@Basic- indcates HB to use defaults (datatype mappings)
It is as good as not having it.

javax.persistence.Transient :- To mark a property not to be persisted.

HB would not automatically persist @transient and static proeprties.

HB saves java.util.Date as - timestamp without timezone.

javax.persistence.@Temporal(javax.persistence.TemporalType.TIME) - default TIMESTAMP

String - VARCHER(255)

character large objecr - clob
byte stream large object - blob

@Lob - choos one of the clob/blob [String - clob], [byte[] - blob]


------------------------

# Retrieving Objects using session.get

```
SessionFactory sessionFactory=new Configuration().configure().buildSessionFactory();
Session session = sessionFactory.openSession();
session.beginTransaction();
session = sessionFactory.openSession();
user = session.get(UserDetails.class, 100001);
System.out.println(user);
```

# Primary Keys

@Id :- Primary Key

## Natural Key vs Surrogate Key

e.g unique and mandatory col
e.g. suppose we require user to provide unique email then we can define email as primary key, called Natural key.
Surrogate - We don't know if any column can be marked a sunique. WWe dont have a column that can be marked as unique or we anticipate that it could change in future so in that case we add a column and it acts as a primary key alone,  
**it does not carry any business significance**
This key is called surrogate key. e.g serial number in a table.

We can tell HB to generate surrogate key for us and we dont require to pass the key while saving.

@GeneratedValue

### @GeneratedValue strategies

1. GenerationType.AUTO **(Default strategy)**:- We let HB decide the best way to generate keys depending on DB used.
2. GenerationType.IDENTITY :- HB will use IDENTITY Columns to generate unique primary keys. Identity columns is a fetaure provided in some of the Databases.
3. GenerationType.SEQUENCE :- HB uses Sequence Database object. These are managed by databases itself.

```sql
   select next_val as id_val from hibernate_sequence for update

```

4. GenerationType.TABLE - sepearte table to store last use primary key so that we can increment aand get the next value. HB will craete this table and use this to generate primary keys.

# Value Types and Embedding Objects (vid - 8)

How does we embedded object?

## One Way - One column for each field of embedded object.

![Hibernate Embedded Object Example](/home/abhimanyu/Pictures/HibernateEmbeddedObject.png "Logo Title Text 1")

This approach works when the Embedded object is a **value object**.
In Hibernate we have two types of objects.

1. Entity Object:- It has its own existence.
2. Value Object :- doeas not have meaning of itself alone. It provides meaning to some other object. e.g Address does not have its own existence without a user object.

So we treat Embedded objects differently depending on there type.

There are few implications of having Embedded objects.

# AttributeOverrides and Embedded Object Keys  (vid -9)

How do we configure columns of the Embedded value Object e.g rename column city as city_name of Address Object.

We can use @Column(name="city_name") for the field. Then the Address object carries this column information everywhere it is embedded e.g say in a Company Object.

What if we want to embedd more than one instances of the Embeddable object. e.g we want to have home_address and office_address in UserDetails object. Now this is a problem since HB can not create two columns for same name i.e. home_address.city_name and office_address.city_name.
This can be solved by overriding default name when embeding objects.

```java
@Embedded
private Address homeAddress;

@Embedded
@AttributeOverrides({
@AttributeOverride(name = "street", column = @Column(name = "home_street")),
@AttributeOverride(name = "city", column = @Column(name = "home_city")),
@AttributeOverride(name = "state", column = @Column(name = "home_state")),
@AttributeOverride(name = "pincode", column = @Column(name = "home_pincode")),
})
// modify default column names
private Address officeAddress;

```

Each embedded objects have there own sets of columns in the table.

**@Embedded does not work with @Id**
If we are using a complex Embedded Object(set of fields acting as primary key) as primary key in the Entity class, then @Embedded would not work in this scenario. We use **@EmbeddedId** in this case.

