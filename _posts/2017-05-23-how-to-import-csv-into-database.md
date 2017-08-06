---
title: "How to Import the Data in CSV File into Database?"
date: 2017-05-23
category: SQLServer
tags: [SQLServer, Database, CSV]
comments: true

---


## Introduction

Recently I was engaged in a project that needs to use some geographical information collected by others. But the problem is that there is not ready-made database, only .csv file was provided. Even though the extra operation was needed, I am not afraid because it is easy to implement using C#, you know, read the data row by row, parse them into an object and then insert the data of the object into database. It is probably time-consuming, but we can implement it right? In fact, it is not a good idea to do that because SQL Server has a function that supports importing large volumes of data into SQL Server tables or unpartitioned views.  

In fact, there are about 4 methods supported by SQL Server to import the large volumes of data, but today I only show you how to use the `bulk insert + SQL`, the easiest way I think, to achieve our goal.  
   
***
 
## What is the CSV File?

<mark>CSV is a simple file format used to store tabular data, such as a spreadsheet or database. Files in the CSV format can be imported to and exported from programs that store data in tables, such as Microsoft Excel or OpenOffice Calc.</mark><br>
<mark>CSV stands for "comma-separated values". Its data fields are most often separated, or delimited, by a comma. For example, let's say you had a spreadsheet containing the following data.</mark>[Source:How To Create A CSV File](https://www.computerhope.com/issues/ch001356.htm)  

As the explanation above, the file that is composed of data fields separated by a comma is the CSV file. So in our case, we use *D:\DB\userinfo.txt* as our source file. It is like below:  
 
<p align="center">
<img src="/images/post/20170523001.png" alt="csv file" /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>
 
***

## SQL Server Order

Now let's look at the part of the SQL Server. We will use the `bulk insert` order to finish this part. More detailed explanation of this order can be found [here](https://docs.microsoft.com/zh-cn/sql/t-sql/statements/bulk-insert-transact-sql)  

First of all, we need to create a data table which is used for storing the relevant information.  

        if exists(select * from sysobjects where name='userinfo')
        drop table userinfo
        go
        
        create table userinfo
        (
            name nvarchar(10),
            email nvarchar(20),
            area nvarchar(10)
        )
        go
        
We had taught how to create the data table in previous articles. You can find it [here](http://oscarzhou.co.nz/blog/sqlserver/2017/05/17/create-database-and-data-table). Then we execute the following orders.  

        
        bulk insert Ip2Location.dbo.userinfo
        from 'D:\DB\userinfo.txt'
        with
        (
            fieldterminator=','
        )
        go
        select * from userinfo 
        go
        
You can the result like this:  
  
               
<p align="center">
<img src="/images/post/20170523002.png" alt="bulk insert result"/><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>
         

So far, we have implemented a transmit of data from csv file to sql server database. It is very easy, right?  

***

## Questions

1.**What if some data fields have double quotes in the csv file, but others don't. How to figure it out?**

<p align="center">
<img src="/images/post/20170523003.png" alt="csv with double quotes"/><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>
       
If you still use the same code to execute, the result will be different from the previous one, like this:  

<p align="center">
<img src="/images/post/20170523004.png" alt="bulk insert result"/><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

As we can see, some columns contains the double quotes, which is not the result we want. The solution of this problem is to use the temporary table. We import csv file into a temporary table firstly, then when we move the data from the temporary table to the final table, we remove the double quotes.  

2.**What if all data fields contains a double quotes. How do we solve it?**  

<p align="center">
<img src="/images/post/20170523005.png" alt="csv with double quotes"/><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

The solution of this problem has one more step than the last one. Except using the temporary table, we have to modify the SQL code.  

        bulk insert Ip2Location.dbo.userinfo
        from 'D:\DB\userinfo.txt'
        with
        (
            fieldterminator='","'    -- Here is the different place.
        )
        go
        select * from userinfo 
        go
        
        
3.**What if the numbers of columns in csv is more than ones in data table. How do we figure it out?**  


<p align="center">
<img src="/images/post/20170523006.png" alt="csv with more columns"/><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

If you execute the previous code, you will get this:  

<p align="center">
<img src="/images/post/20170523007.png" alt="bulk insert result error"/><br/>
<center><h5><b>Figure 7</b></h5></center>
</p>

The solution is that we create the columns for every data field in the temporary table. When we import it into the final table, we ignore those useless data columns.  
 
 
So much for today. Hope that it can help you guys.
