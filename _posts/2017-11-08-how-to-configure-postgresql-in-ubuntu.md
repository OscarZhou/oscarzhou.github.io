---
title: "How to Configure PostgreSQL in Ubuntu"
date: 2017-11-08
category: Other
tags: [Installation, Configuration, PostgreSQL, Ubuntu]
comments: true

---


## Introduction

This is my first time to configure a database on the virtual machine with totally command lines as my server. I met some obstacles during the process, fortunately I solved them. I will use the simplest and most direct way to explain this part.  

***

## Commands

First of all, rookies need to know when the command `sudo` can be typed. It only works on the system user, that is, the switch of the users do not happen. Let's get started to configure PostgreSQL on the virtual machine.     

**1.Installation**

`sudo apt-get update` Before installing the PostgreSQL, we had better do the updating.  
`sudo apt-get install postgresql postgresql-contrib` Download and install the PostgreSQL.  

**2.Basic Operation**

`sudo -i -u postgres` Switch off the user to postgres user. After doing this, we will see that the directory showed on the screen has already change to <span style="color:white;background-color:black">postgres@ubuntu:</span>  
 
`postgres@ubuntu: psql` Access to the PostgreSQL default database *postgres* as postgres user. After doing this, the directory changed again, like <span style="color:white;background-color:black">postgres=#:</span>     

In the PostgreSQL, there are several common commands to help us check some information about the database:    
 
`postgres=#: \l` View all established databases.  
`postgres=#: \dt` View all data tables.  
`postgres=#: \du` View all users.  
`postgres=#: \conninfo` View current connected information.  
`postgres=#: \password postgres` Set user postgres's password.  
`postgres=#: \q` Quit the database.  

For example, under the current substance, `postgres=#: \conninfo` can return:  

<span style="color:white;background-color:black">You are connected to database "postgres" as user "postgres" via socket in "/var/run/postgredql" at port "5432"</span>   

Note that the user in above result points the system user, instead of the database user.  

**3.Create User and Database**

The following operation is executed under the database user. Taking the example of `postgres`.  

`postgres@ubuntu: createuser --interactive`  Define a new PostgreSQL account.   
`postgres@ubuntu: createdb testdb` Define a new database named *testdb*  
`postgres@ubuntu: psql -d testdb` Switch off to different database.    

`sudo adduser tester` Create a new user named *tester* in system level.  
`sudo -i -u tester` Switch off to user tester.  

At this moment, the directory will become <span style="color:white;background-color:black">testdb=#:</span>, and the `testdb=#: \conninfo` will return:  
 
<span style="color:white;background-color:black">You are connected to database "testdb" as user "tester" via socket in "/var/run/postgredql" at port "5432"</span>   

**4.Finish the Configuration**

If our sever wants to sucessfully connect to the database, we need to finish the following configuration:  

Modify the file `/etc/postgresql/9.*/main/pg_hba.conf`, then locate the line with *host all all x.x.x.x/x md5*, change it to your Ubuntu IP address.  
Modify the file `/etc/postgresql/9.*/main/postgresql.conf`, then locate the line with *listen_address=''*, change it to *listen_address='\*'*.  
Restart the PostgreSQL  

**5.Other**

I want to share two commands to help finish the 4th step.  

`sudo netstat -plunt |grep postgres` View the IP and Port of the PostgreSQL  
`sudo service postgresql restart` Restart the service of PostgreSQL.  

***

Hope that this article will helped you.  
