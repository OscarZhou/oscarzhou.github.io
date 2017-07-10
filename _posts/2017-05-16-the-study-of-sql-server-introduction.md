---
title: "The Study of SQLServer: Introduction"
date: 2017-05-16
category: SQLServer
tags: [SQLServer, MVC, Database]

---

Nowadays, any software or websites will be meaningless without the operation of the data, which indirectly proves the importance of the database.Currently, there are many kinds of database that are used by corporations and companies, such as Oracle, mysql, MangoDB and SQLServer. Even though they have different name, their purpose are the same and all of them use the basic sql syntax.  
 
I will show you how to create and code a database according to requirement document in SQLServer, as it is a very easy database to study and a Microsoft product which can perfectly match with the .Net framework.  

Due to that I don't want to make an article too long to read, so the special topic will be divided up into many parts in different articles. In this part, I will give a general idea of SQLServer.  

***

1. [Introduction](http://oscarzhou.co.nz/blog/sqlserver/2017/05/16/the-study-of-sql-server-introduction)
2. [Create Database and Data Sheet](http://oscarzhou.co.nz/blog/sqlserver/2017/05/17/create-database-and-data-sheet)
3.   

***
As we see, the DBMS provides the data definiation language(DDL) so that users can use DDL to create the database easyly. And the DBMS also provides the data manipulation language(DML) to insert, update, delete and query data.  

SQLServer is one of the Database Management System. In SQLServer, you can use simple SQL syntax to manipulate the data. (Fig.2)  

<p align="center">
  <img src="/images/post/20170516002.png" alt="Simple SQL Syntax" /><br/>
  <center><h4><b>Figure 2</b></h4></center>
</p>

The snippet of code is very easy for programmers, but you can't expect people who don't have the major background to code like that in order to operating the database. Thus, the application appears.

***


## Application

What the application do is actually to help users operate data information more intuitive and convient. People can use the application to achieve their goal rather than use DBMS.  

**Function:** Send request to database and display the result of the response.  
**Requirment:** Be able to finish data proecssing according to the business logic.
  
***

## How to study the Database

* Study the standard SQL language  
    1. SQL (Structed Query Language)
    2. SQL can be used to finish all the operation to the database
    3. The application communicate with the database by SQL  
* Study how to manage database according to the specific DBMS  
    1. Data import and export  
    2. Data backup and restore  
    3. The performance of the Database improvement  
* Create the application by embeding the DML into senior programming language
    1. Base on desktop application of C/S
    2. Base on Web application of B/S  

***


What I talked above might be a little bit abstract, but they are basic concept about the database, which is very important. So let's do some coding in next article.  




