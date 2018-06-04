---
layout: post
title:      "     What is Object-Relational Mapping (ORM)? A Basic Understanding"
date:       2018-06-04 12:55:08 -0400
permalink:  what_is_object-relational_mapping_orm
---



The issue with storing data using only an Object-Oriented (OO) language is that the data it collects disappears once the application is closed. This is a problem since most apps created with OO languages require data to be persisted — that is, stored long term. The obvious solution is to store this information in some sort of database, but how do we go about this? Is there a way to use the OO language to store data in a database? Well, sort of.

OO languages and databases differ in their syntax and constructs. OO languages consist of constructs such as Classes, instances, and attributes, whereas databases respond to query languages (e.g. SQL) and consist of Tables, rows, and columns. This difference makes it difficult to access databases directly using only the syntax of an OO language. To resolve this, we need a way to relate (map) the OO language constructs to the relational database constructs in a way that makes sense. The process of doing so is called Object-Relational Mapping (ORM), which helps coders link their programs to databases to persist the data of interest (store it long-term).

To get a better understanding of this, let’s explore the relationship that the ORM technique establishes between OO language constructs and database constructs. I will forego most of the code, since my focus is more on the relationship between the constructs.

<br>


**We will start with databases first**

Let’s say we have a company that has many employees it wants to keep track of. To do so, we decide to create database to store all of the information related to the company’s employees, each of whom have unique attributes. So, we need three things to store this information: (1) A TABLE, (2) COLUMNS, (3) ROWS.

1.	The TABLE name will be “Employees.”
2.	The COLUMNS will represent the employees’ attributes. The attributes in this example include the employee’s first name, last name, age, height, and hire date. And, since we might have multiple employees with the same first name, let’s also give each employee a unique ID so that we can keep track of them without any confusion. We don’t want to accidentally give John Foo’s New Year's bonus to John Gibble, after all.
3.	We need to add a new ROW for each employee, with the appropriate information filled out in the respective COLUMN for each attribute.


We will go ahead and add two employees — “John Foo” and “John Gibble” — and their attributes to our table. Since they are each separate, individual, human beings, they get their own rows.

Let’s try to visualize this:


<figure>
<img id="database_table" src="https://i.imgur.com/62dMHlG.png" title="source: by Adeel M." style="height:150px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">Database Table</figcaption>
</figure>



Okay, great! We’re off to a good start in storing the company employees’ information. Now, we just need to add in several more entries (rows) — one for each employee.

<br>

**Now, let’s try to do the same thing with an OO language.
We will use Ruby as the OO language of choice for this example.**

Let's say we want to create a program that:

1.	Allows us to create new entries (instances/rows) for employees
2.	Add their attributes
3.	Allows us to do other cool things with the data. 
4.	We may want to create an interactive website that lets the user interact with the data through some sort of graphical user interface.

We can’t accomplish this with the same code we used to store the data in the previous example, since Structured Query Language (SQL) is good primarily for Creating, Retrieving, Updating, and Deleting data from a database (C.R.U.D.). Rather, we will need to use an OO language to create the graphical interactive portion of our app.

When using an OO language, we will think of each employee as an object with specific attributes. Each time we create a new employee entry, we want our program to create a new instance of the object “Employee,” assign it attributes, and then store it in a class variable.


Let’s use our previous employees, “John Foo” and “John Gibble,” for this example:

To begin, we will create a CLASS called Employee.
Each Employee will have attributes, which we will define as attr_accessor :id,  :first_name,  :last_name,  :age,  :height,  :hire_date


When we create entries (instances) of each new Employee, we will use:
```
Employee.new(id, first_name, last_name, age, height, hire_date)
```

<br>

Now, we can create an entry for John Foo and give him attributes using
```
john_Foo = Employee.new(1, “John Foo”, 28, “6 feet”, “June 2017”)
```

<br>

Let’s create a second entry for John Gibble using
```
john_Gibble = Employee.new(2, “John Gibble”, 25, “6 feet”, “April 2018”)
```


We have just used our object-orients (OO) program to create two instances of the object Class “Employee”: john_Foo & john_Gibble, each with their own attributes.

<br>

The program has stored john_Foo in its class variable as:
```
=> #<Employee:0x00007fee5a83e0f0 @id=1, @first_name="John", @last_name="Foo", @height="6 feet", @hire_date="June 2017"> 
```

 
It has also stored john_Gibble in its class variable as:
```
 => #<Employee:0x00007fee5a030340 @id=2, @first_name="John", @last_name="Gibble", @height="6 feet", @hire_date="April 2018">
```

<br>

The program stores each employee as a separate instance, with unique *object instance ids* (e.g.  Employee:0x00007fee5a83e0f0 and Employee:0x00007fee5a030340), along with each of the attributes we assigned them.

<br>

Let’s try to visualize these entries in the form of a table:


<figure>
<img id="database_table" src="https://i.imgur.com/JQ5F9vG.png" title="source: by Adeel M." style="height:150px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">Class Instances in Table Form</figcaption>
</figure>

<br>
<br> 
Note that we do not include the object's instance ids (e.g. Employee:0x00007fee5a83e0f0) as any of the column names. This is because the rows of the table are a visual representation of these instances.


Let’s compare this table representation of the Employee Class, its instances, and attributes with the previous database table and it columns and rows. By doing so, we can derive and establish the following relationship between the OO language constructs and database constructs:

<figure>
<img id="database_table" src="https://i.imgur.com/DTthjie.png[/img" title="source: by Adeel M." style="height:150px" />
<figcaption style="font-family: arial; font-size:14px; color:gray;">Relationship between OO language constructs & Database constructs</figcaption>
</figure>

<br>

* The **Class** gets turned into a **table** in the database.
* A Class **attribute** gets turned into a **column** in the database.
* Each **instance of a Class** object gets turned into a **new row** in the database.

We have just finished mapping each OO language construct to its equivalent relational database construct. This is pretty much what we do when we use the ORM technique: define relationships between OO language constructs and database constructs so that the OO programming language can communicate with the database in a logical way.


<br>

**Bringing it all together:**

As we discussed earlier, the data for each new instance of the object will get stored in a class variable of some sort. This is unacceptable, however, since it will disappear once we close the program. So, we will store (persist) this data in a database, instead. We do this by first connecting our program to a database, such as SQLite3. Then, thanks to the ORM technique we discussed above, we will be able use query language (e.g. SQL) within the OO language we are working with (e.g. Ruby) to interact with the database. It essentially lets us tell the database what to do with each Class, its attributes, and instances in terms of database tables, columns, and rows.

Once done, the name for each Class we create will be used to create a table in the database with the same name. Each attribute for the class Object will be used to create a column in the database. And, each time we instantiate a new object for our class, the database will treat it as a new entry — it will create and add a new row for it in the respective table.

Now the data will remain inside of a database even after we close the program. Any time we wish to run our program again and use any of the stored data, we will tell our program which data we want. The program will contact the database and tell it what it wants using ORM. The database will provide our program with the information, which will then get converted into something the OO programming language understands — its respective classes and attributes, creating a new instance for each row of the table. Once retrieved and converted, our program will then be able to manipulate that data using the constructs it knows (classes, attributes, instances, etc.).

Since creating ORMs is not always an easy task, many OO languages provide libraries with pre-built ORMs that contain pre-defined functions/methods. For Ruby and its frameworks, this  librarycomes in the form of ActiveRecord. Such libraries have their own syntax/naming conventions, but it is a small price to pay for increased ease of usability.

