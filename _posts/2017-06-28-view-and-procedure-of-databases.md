---
title: "The View and Procedure of The Database"
date: 2017-06-28
category: SQLServer
tags: [SQL, Database, Procedure, View]
comments: true

---

## What is stored procedure?

A **[stored procedure](https://en.wikipedia.org/wiki/Stored_procedure)** (also termed proc, storp, sproc, StoPro, StoredProc, StoreProc, sp, or SP) is a subroutine available to applications that access a relational database management system (RDBMS). Such procedures are stored in the database data dictionary.  
Uses for stored procedures include data-validation (integrated into the database) or access-control mechanisms. Furthermore, stored procedures can consolidate and centralize logic that was originally implemented in applications. To save time and memory, extensive or complex processing that requires execution of several SQL statements can be saved into stored procedures, and all applications call the procedures. One can use nested stored procedures by executing one stored procedure from within another.  

***

## Custom stored procedure without parameters
 
The syntax of defining stored procedure:  

    Create PROC[EDURE] [procedure name]
        @[parameter1] [data type] = [default value] OUTPUT,
        ....,
        @[parameterN] [data type] = [default value] OUTPUT,
    AS
        [SQL statement]
    GO


The parameters of stored procedure:
1. Optional parameters, which is same in C#
2. Input parameters and output parameters
3. Input parameters allows default value

I will use some specific question to show how to create non-parameter stored procedure. For example, there are table `Students`, `StudentClass` and `ScoreList`. All data we will operate come from these three tables.  

<p align="center">
<img src="/images/post/20170628001.png" alt="data table" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


**Question:** 
1. Querying the test results. Displaying the student number, student name, class and overall result and sorting the overall result.  
2. Counting and analyzing the overall result. Displaying class name, the average score of C#, the average score of databases and grouping the class.  
  
As the illustration above showed that the student name and student number are in the table `Students`. The class name is in the table `StudentClass`. The overall result is in the table `ScoreList`. Thus, question one is join query from 3 tables.  

The procedure can be written like  
  
    
    use [SMDBWeb]
    go
    
    if exists(select * from sysobjects where name = 'usp_queryscore')
    drop procedure usp_queryscore
    go
    
    create procedure usp_queryscore
    as
        select StudentName, st.StudentId, ClassName, ScoreSum = (CSharp + SQLServerDB)
        from Students st 
        inner join StudentClass sc on st.ClassId = sc.ClassId
        inner join ScoreList sl on st.StudentId = sl.StudentId
        order by ScoreSum desc
    
        select ClassName, C#Avg = avg(CSharp), DBAvg = avg(SQLServerDB)
        from Students st 
        inner join StudentClass sc on st.ClassId = sc.ClassId
        inner join ScoreList sl on st.StudentId = sl.StudentId
        group by ClassName
    
    go
    
    exec usp_queryscore

You can see that we create the procedure by referencing the syntax of the procedure. The result is:    
  
<p align="center">
<img src="/images/post/20170628002.png" alt="non-parameter procedure" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

***

## Custom stored procedure with parameters

There are two kinds of parameters for stored procedure, input parameters and output parameters. Input parameters is to pass the value into stored procedure. Output parameters is to get the result sets after calling stored procedure.  

**Question:** Querying the overall result by custom passing line.    

The procedure can be written in this way.  

    use [SMDBWeb]
    go
    
    if exists(select * from sysobjects where name = 'usp_queryscore')
    drop procedure usp_queryscore
    go
    
    create procedure usp_queryscore
    @C# int,
    @DB int
    as
        select StudentName, st.StudentId, ClassName, CSharp, SQLServerDB
        from Students st 
        inner join StudentClass sc on st.ClassId = sc.ClassId
        inner join ScoreList sl on st.StudentId = sl.StudentId
        where CSharp > @C# and SQLServerDB > @DB
        
    go
    
    exec usp_queryscore 60,60
     
In this procedure, the passing line for C# and SQLServerDB is 60 and 60 respectively.  

<p align="center">
<img src="/images/post/20170628003.png" alt="parameter procedure" width="50%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

You can find that all scores are above 60.  

***

## Custom stored procedure with default parameters

For the same question, the procedure can be written like  

    use [SMDBWeb]
    go
    
    if exists(select * from sysobjects where name = 'usp_queryscore')
    drop procedure usp_queryscore
    go
    
    create procedure usp_queryscore
    @C# int=60,
    @DB int=60
    as
        select StudentName, st.StudentId, ClassName, CSharp, SQLServerDB
        from Students st 
        inner join StudentClass sc on st.ClassId = sc.ClassId
        inner join ScoreList sl on st.StudentId = sl.StudentId
        where CSharp > @C# and SQLServerDB > @DB
        
    go
    
    exec usp_queryscore
    exec usp_queryscore @C#=65
    exec usp_queryscore @DB=65
    exec usp_queryscore 60,65
     
As we see, the only changes different from the previous one is to assign default value to the parameters. The result is  


<p align="center">
<img src="/images/post/20170628004.png" alt="parameter procedure" width="50%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

***

## Custom stored procedure with output parameters

**Question:** Querying the overall result by custom passing line and displaying the query list and outputing the number of people who do not participate the test and fail the tests.  
      
    use [SMDBWeb]
    go
    
    if exists(select * from sysobjects where name = 'usp_queryscore')
    drop procedure usp_queryscore
    go
    
    create procedure usp_queryscore
    @C# int=60,
    @DB int=60,
    @AbsentCount int output,
    @FailedCount int output
    as
        select StudentName, st.StudentId, ClassName, CSharp, SQLServerDB
        from Students st 
        inner join StudentClass sc on st.ClassId = sc.ClassId
        inner join ScoreList sl on st.StudentId = sl.StudentId
        where CSharp > @C# and SQLServerDB > @DB
        
        select @AbsentCount = count(*) from Students 
        where StudentId not in (select StudentId from ScoreList)
    
        select @FailedCount = count(*) from ScoreList
        where CSharp < @C# or SQLServerDB < @DB
    
    go
    
    declare @AbsentCount int, @FaildedCount int
    exec usp_queryscore 60,60,@AbsentCount output, @FaildedCount output
    select AbsentCount = @AbsentCount, FailedCount = @FaildedCount
    
The store procedure with output parameters requires to define the output parameters at the very beginning. In the part of the logical implementation, it is only to query from a single table, which is not difficult. You should notice that the part of executing procedure is different from the ones we did before. You have to declare the output parameter and use select statement to show the result of output parameters.  
  
In this case, the result is  


<p align="center">
<img src="/images/post/20170628005.png" alt="parameter procedure" width="50%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

***

So far, I have already showed the use of different custom stored procedure. It is better to keep the syntax in your mind.  