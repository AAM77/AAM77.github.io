---
layout: post
title:      "The MANY-to-MANY Relationship in ActiveRecord"
date:       2018-06-18 14:06:33 -0400
permalink:  the_many-to-many_relationship_in_activerecord
---



### { The TL; DR version }

This post serves as more of a "how-to" guide than an in-depth explanation on many-to-many relationships in ActiveRecord.

To establish a many-to-many between table_1 and table_2:

Assuming you have files for table_1 and table_2, named *table_1.rb* and *table_2.rb*, respectively, create a new file to serve as a join file/table: let's call ours *table1_table2.rb*.

Then, add the following associations to the file:

```
class Table1_Table2 < ActiveRecord::Base
     belongs_to :table_1
     belongs_to :table_2
end
```

And, add the following line to *table_1.rb*:

```
class Table_1 < ActiveRecord::Base
     has_many :table1_table2
     has_many :table_2, through: :table1_table2
end
```


And, add the following line to *table_2.rb*:

```
class Table_2 < ActiveRecord::Base
     has_many :table1_table2
     has_many :table_1, through: :table1_table2
end
```

<br>

### { The Long Version }

I had trouble understanding how to implement the many-to-many relationship in ActiveRecord, so I wrote a blog post to help myself connect the dots. Again, this blog post is not meant to be in-depth. It is more of a "how-to" for establishing the equivalent of a join table in activerecord.

To establish a many-to-many relationship between tables in a database, we would create a separate join table containing foreign-key columns for each of the tables. This would then effectively link up the tables in a many-to-many relationship and we woud be able to extract data of interest using the appropriate join syntax.

In ActiveRecord, we do something similar. Instead of a table, we create new model. For example, if we have a database with an Authors table, Books table, and Genres table, we would need to create the following files (models):

We have three models:
* (1)	Book (*book.rb*)
* (2)	Author (*author.rb*)
* (3)	Genre (*genre.rb*)

In this design, we need to define the following relationships:

A book belongs to one author ---> Belongs to only ONE author (one-to-one) <br>
A book can have many genres ---> Has Many genres (one-to-many)

An author can write many books. ---> Has Many Books (one-to-many) <br>
An author can write books in many genres ---> Has Many genres (one-to-many, through another)

A genre can include many books ---> Has Many books (one-to-many) <br>
A genre can include many authors ---> Has Many authors (one-to-many, through another)

We can establish the one-to-one relationships using 'belongs_to :table_name  .

We can establish the one-to-many and many-to-one relationships using 'has_many' and 'has_many, through'

After adding these associations, our code in each file should look as follows:
<br>
<br>
**book.rb**
```
class Book
     belongs_to :author
     # needs a has_many association to genres
end

# The books table serves as the intermediary (join table) between genres and authors since it has foreign key columns for author_id and genre_id.
```
<br>
**author.rb**
```
class Author
     has_many :books
     has_many :genres, through books
end

# think of the has_many, through relationship as:
# they can only have a genre once they write a book.
# No book, No genre
```
<br>
**genre.rb**
```
class Genre
     has_many :authors, through books
     # needs a has_many association to books
end

# think of the has_many, through relationship as:
# you can’t say an author has written in a particular genre of books if he/she has never written a book in that genre.
# No book, No author
```

<br>
<br>
**Establishing the Many-to-Many relationship:**

The relationship we are interested in now is the many-to-many relationship between books and genres.

1 book -> Many genres <br>
1 genre -> Many books

This is effectively a Many-to-Many relationship, which means that many books can have many genres (and vice versa).

To establish this association in ActiveRecord, we need to create a new model file that serves as a “join table." Let's name this file *book_genre.rb*

Doing so will allows us to:

(1)	link each book to many genres, through the *bookgenre* file/table

(2)	link each genre to many books, through the *bookgenre* file/table


Within the book_genre class, we establish a one-to-one relationship to a genre and a one-to-one relationship to a book.

So, the code would look something like the following:<br>
```
class BookGenre
     belongs_to :book
     belongs_to :genre
end
```

Now, we go to the *book.rb* file and add the following associations: <br>
Add to **book.rb**:

```
has_many :book_genres
has_many :genres, through: :book_genres
```

<br>

Now, we go to the *genre.rb* file and add the following association: <br>
Add to **genre.rb**: 

```
has_many :book_genres
has_many :books, through: :book_genres
```


And, that’s basically it. This will create all of the correct associations for the relationships we are looking to establish.

<br>
The finished code for associating each of our models/files will look something like the following: <br>

**book.rb**

```
class Book
     belongs_to :author
     has_many :book_genres
     has_many :genres, through: :book_genres
end
```

<br>
**author.rb**
```
class Author
     has_many :books
     has_many :genres, through books  
end
```

<br>
**genre.rb**

```
class Genre
     has_many :book_genres
     has_many :authors, through books
     has_many :books, through: :book_genres
end
```


Note: <br>
*ActiveRecord does have a has_and_belongs_to_many that allows for intransitive many-to-many associations, but I have been informed that it is deprecated and not recommended. It is better to create a join table, as described above.*

