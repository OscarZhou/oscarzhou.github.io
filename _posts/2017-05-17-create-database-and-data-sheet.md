---
title: "The Study of SQLServer: Create Database and Data Sheet"
date: 2017-05-17
category: SQLServer
tags: [SQLServer, Database, DML]

---


We have introduced some concepts about database and database management system in the last article. Today we will use sql to create database and data sheet.     
  
If you want to go through other relevant articles, please look at here: 

***

1. [Introduction](http://oscarzhou.co.nz/blog/sqlserver/2017/05/16/the-study-of-sql-server-introduction)
2. [Create Database and Data Sheet](http://oscarzhou.co.nz/blog/sqlserver/2017/05/17/create-database-and-data-sheet)
3. 

***  

## Create Database 

Open the SQLServer, click the [New Query] button and input the below order.
                
        -- specify the database we plan to use
        use master
        go
        
        
        -- create database
        create database StudentManageDB
        on primary
        (
            name='StudentManageDB_data', -- the logic name of the database file
            filename='C:\DB\StudentManageDB_data.mdf', -- the physical name of the database file, absolute path
            size=10MB, -- the initial size of the database file
            filegrowth=5MB --the incremental size of the database file
        )
        -- create log file
        log on
        (
            name='StudentManageDB_log',
            filename='C:\DB\StudentManageDB_log.ldf',
            size=5MB,
            filegrowth=2MB
        )
        go

The order above can be used for creating database but we should note that the database is user database. In fact, the database can be grouped into system database and user database. Fig3.  


<p align="center">
  <img src="/images/post/20170517001.png" alt="System Types" /><br/>
  <center><h4><b>Figure 1</b></h4></center>
</p>

We can see that there are four databases in the system database, which is 'master', 'model', 'msdb' and 'tempdb' respectively.  

* **master:** Store all database information including system login, configuration setting and connected server, etc..  
* **model:** The template database used for creating new user database.  
* **msdb:** Save the backups of the database, SQL Agent information, DTS program package and SQLSERVER tasks, etc..  
* **tempdb:** Store the temporary objects, for example, the temporary table and procedure.  

In addition, we should know the composition of the user database file. A database file contains data file and log file. The data file is the file with the extension ".mdf" that is the main data file and ".ndf" that is the minor data file. The log file is the file with the entension ".ldf". <mark>One database must contain only one .mdf file but is allowed to contain the .ndf file and .ldf file.</mark>  

 
***

## Delete Database

        -- determine if the current database exists
        if exists(select * from sysdatabases where name='StudentManageDB')
        drop database StudentManageDB
        go

Exists() is used for checking if the database StudentManageDB exists. If ok, delete it.  
<mark> You need to be careful to use drop, because the database can't be restored with drop.</mark>  

***

## Detach and Attach Database

Normally, when the database service is running, direct moving and copying database file are not allowed. The so-called detaching database is to unbind the restrictions of service of the database file being used.  

        exec sp_detach_db @dbname='StudentManageDB' --database name
        
Attaching database is adding the database file in the specific path into database service and running it. Only when the database is attached are users able to manipulate the data via DBMS.  
    
        exec sp_attach_db @dbname='StudentManageDB' --database name
        @filename1='C:\Work&Study\DB\StudentManageDB_data.mdf',--the physical path of the main database file
        @filename2='C:\Work&Study\DB\StudentManageDB_log.ldf' --the physical path of the database log file

***

Hope that it can help you guys.  



