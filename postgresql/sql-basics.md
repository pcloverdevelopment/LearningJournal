---
description: >-
  Quick overview of what SQL is, it's relation to other DB models, and some
  terminology.
---

# SQL Basics

### Programming Paradigm

> SQL is a declarative language, so it focuses on the **WHAT WILL HAPPEN** and not **HOW IT WILL HAPPEN**

All this means is that we don't have to worry about how or where the data is actually stored. We just know the logical model of the database and write what we want in SQL. This abstracts the complexity of the database physical storage, rules, and constraints from us. 

### What Makes a Database a ... database?

Let's do a brief history:

1. Way back in the day, file processing systems were used to store files via software... this was a step forward but there was no relation between the files... and the software itself was in various languages and had to know about the operating systems, hardware, etc.... \(So using python to do data operations via file IO rather than the common language of SQL...\)
   * File processing system had no structure or relationship between files
   * There was a bunch of custom software built in various languages talking directly to hardware
   * These different systems didn't have a good way to talk to each other, and ended up storing redundant data in each system from other systems, which is very error prone.
2. We changed to a database oriented approach eventually, where a DBMS was used, and the data had to follow a certain model... the structured model helped to consolidate the fragmented file systems and helped form relationships between data. This logical approach helped with relieving redundant data, a common language for working with data \(SQL Standard\)... we no longer had to care about the hardware or operating system, etc... 
   * Caused people to start using a general approach rather then custom software
   * Provided an abstraction over handling the physical storage of data
3. We started off with various models that provided a logical structure to data that eventually led to the relational model..
   * First we had Hierarchical which was a parent child one to many relationship \(70s and 80s\) tree like structure.. looked like the HTML/XML DOM
   * Then we had the networking model come along which started to provide a many to many relationship between our database entities.. \(70s and 80s\)...
   * Then came relational! It provides rules \(Boyce-Codd 12 rules of a RDBMS\) of how a relational system works with it's constraints and rules. [https://www.w3resource.com/sql/sql-basic/codd-12-rule-relation.php](https://www.w3resource.com/sql/sql-basic/codd-12-rule-relation.php)
   * Some other database models include enitity-relationship, object oriented, flat, semi-structured, etc..



