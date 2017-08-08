---
title: "Three Layers Architecture in MVC"
date: 2017-06-01
category: .Net
tags: [.Net, MVC, Three Layers]
comments: true

---

## Introduction

I think three layers architecture is very useful to make my logic of programming clearer. I have to say I fairly rely on it. Today I will introduce this easy and useful architecture.  

Three layers includes three class libraries: BLL(Business Logic Layer), DAL(Data Access Layer) and Models.  


<p align="center">
<img src="/images/post/20170601001.png" alt="three layers" width="40%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


Before coding, I need to add the reference to make sure that all goes smoothly.    

***


## About Reference

Currently, these three layers are only separated parts. Before starting coding, I have to make some connection among them by adding **Reference**.  
In the DAL layer, I only add the reference of Models:  

<p align="center">
<img src="/images/post/20170601006.png" alt="reference about DAL" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

In the BLL layer, the reference of Models and DAL should be added:  

<p align="center">
<img src="/images/post/20170601007.png" alt="reference about BLL" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

At last, I add the reference of BLL and Models into main project:  

<p align="center">
<img src="/images/post/20170601008.png" alt="reference about project" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

After completing the addition of reference, I start to introduce these three class libraries one by one.  


***


## Models

**Models** usually contains classes which are corresponding to the tables of the database. For example, A student management system has the table of the students, the table of the student classes and the table of the system administration. 

<p align="center">
<img src="/images/post/20170601002.png" alt="Models" width="40%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

Then you can create the class like below:

    
    using System;
    namespace Model
    {
        public class Student
        {
            public int StudentId { get; set; }
    
            public string StudentName { get; set; }
    
            public string Gender { get; set; }
    
            public DateTime Birthday { get; set; }
    
            public string StudentIdNo { get; set; }
    
            public string CardNo { get; set; }
    
            public string PhoneNumber { get; set; }
    
            public string StudentAddress { get; set; }
    
            public int ClassId { get; set; }
        }
    }

The corresponding table in database:  

<p align="center">
<img src="/images/post/20170601003.png" alt="data table" width="40%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

This part is easiest one among the three layers.
  
***

## DAL(Data Access Layer)

**Data Access Layer** is used for the operation that needs to connect the database. Firstly, I usually create a **SQLHelper** file to encapsulate some ADO.NET syntax, which will make the subsequent calling easier.  

<p align="center">
<img src="/images/post/20170601004.png" alt="Models" width="40%"  /><br/>
<center><h5><b>Figure 7</b></h5></center>
</p>

The **SQLHelper** is the most important part in three layers. You can code like below:  

    public class SQLHelper
    {
        private static string connString = ConfigurationManager.ConnectionStrings["connString"].ToString();

        public static int Update(string sql)
        {
            SqlConnection conn = new SqlConnection(connString);
            SqlCommand cmd = new SqlCommand(sql, conn);
            try
            {
                conn.Open();
                return cmd.ExecuteNonQuery();
            }
            catch (SqlException e)
            {
                Console.WriteLine(e);
                throw;
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
            finally
            {
                conn.Close();
            }
        }
        public static object GetSigleObject(string sql)
        {
            SqlConnection conn = new SqlConnection(connString);
            SqlCommand cmd = new SqlCommand(sql, conn);
            try
            {
                conn.Open();
                return cmd.ExecuteScalar();
            }
            catch (SqlException e)
            {
                Console.WriteLine(e);
                throw;
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
            finally
            {
                conn.Close();
            }
        }
        public static SqlDataReader GetDataReader(string sql)
        {
            SqlConnection conn = new SqlConnection(connString);
            SqlCommand cmd = new SqlCommand(sql, conn);
            try
            {
                conn.Open();
                return cmd.ExecuteReader(CommandBehavior.CloseConnection);
            }
            catch (SqlException e)
            {
                Console.WriteLine(e);
                throw;
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
        }
    }

There are several points that you need to take notice of the codes above.  
 
**1.** All the methods are static methods.  
**2.** In the *Update()* and *GetSingleObject()* methods, you can close the connection of the database in `finally{}`. However, in the *GetDataReader()* method, you need to pass the enumerate variables `CommandBehavior.CloseConnection` which allows to close connection of database outside of the method. I will show it in later part.  
**3.** Adding the `try{}catch{}` is necessary.  

***

Secondly, I will start to add some files corresponding to the classes in **Models**. The naming rule is usually **"class name+Service"**, like StudentService, ClassService and SysAdminService. You can see it in the Fig.7.  

In the service files, we need to add some methods that implement specific database operation. So it does make sense that the specific sql syntax will be written in them. I will give an example about it instead of presenting all of them.  

    public List<Student> GetListByClassId(string className)
    {
         StringBuilder sqlBuilder = new StringBuilder();
         sqlBuilder.Append("select * from [dbo].[Students] ");
         sqlBuilder.Append(
             "inner join [dbo].[StudentClass] on [dbo].[Students].ClassId = [dbo].[StudentClass].ClassId ");
         sqlBuilder.Append("where [dbo].[StudentClass].ClassName like '%{0}%'");
 
         string sql = string.Format(sqlBuilder.ToString(), className);
         try
         {
             List<Student> objStudents = new List<Student>();
             SqlDataReader objReaders = SQLHelper.GetDataReader(sql);
             while (objReaders.Read())
             {
                 objStudents.Add(new Student()
                 {
                     StudentId = Convert.ToInt32(objReaders["StudentId"]),
                     StudentName = objReaders["StudentName"].ToString(),
                     Gender = objReaders["Gender"].ToString(),
                     Birthday = Convert.ToDateTime(objReaders["Birthday"]),
                     StudentIdNo = objReaders["StudentIdNo"].ToString(),
                     CardNo = objReaders["CardNo"].ToString(),
                     PhoneNumber = objReaders["PhoneNumber"].ToString(),
                     StudentAddress = objReaders["StudentAddress"].ToString(),
                     ClassId = Convert.ToInt32(objReaders["ClassId"])
                 });
             }
             objReaders.Close();  
             return objStudents;
         }
         catch (Exception e)
         {
             Console.WriteLine(e);
             throw;
         }
     }
     
     public int InsertStudent(Student objStudent)
     {
         StringBuilder sqlBuilder = new StringBuilder();
         sqlBuilder.Append(
             "insert into [dbo].[Students] (StudentName, Gender, Birthday, StudentIdNo, CardNo, PhoneNumber, StudentAddress, ClassId) ");
         sqlBuilder.Append("values('{0}', '{1}', '{2}', {3}, '{4}', '{5}', '{6}', {7})");
         string sql = string.Format(sqlBuilder.ToString(), objStudent.StudentName, objStudent.Gender,
             objStudent.Birthday, objStudent.StudentIdNo, objStudent.CardNo, objStudent.PhoneNumber,
             objStudent.StudentAddress, objStudent.ClassId);
 
         try
         {
             return SQLHelper.Update(sql);
         }
         catch (Exception e)
         {
             Console.WriteLine(e);
             throw;
         }
 
     }
    
  
In the *GetListByClassId()*, you will see `objReaders.Close(); ` before the `return`, which is the point that I mentioned before about `SqlDataReader.Close`. You had better take some time to understand this part.  

*** 

## BLL(Business Logic Layer)

**Business Logic Layer** is the easiest part in these three layers. The naming rule is usually **"class name+Manage"**, like StudentManage, ClassManage and SysAdminManage. The codes are like this:  

    public class StudentManage
    {
        private StudentService objStudentService = new StudentService();
        public List<Student> GetListByClassId(string className)
        {
            if (className != null)
            {
                //将搜索条件保存到session
                HttpContext.Current.Session["query"] = className;
            }
            return objStudentService.GetListByClassId(className);
        }

        public int InsertStudent(Student objStudent)
        {
            return objStudentService.InsertStudent(objStudent);
        }

        public Student GetStudent(string studentId)
        {
            return objStudentService.GetStudent(studentId);
        }

        public int EditStudent(Student objStudent)
        {
            return objStudentService.EditStudent(objStudent);
        }

        public int DeleteStudent(string stuId)
        {
            return objStudentService.DeleteStudent(stuId);
        }
    }

You can find out that most of time, I called the methods corresponding to the functions in the service files. Yes, it is what this layer usually does, but sometimes you can add other codes to assist to implement the function you want to have. That's the reason why it is called **Business Logic Layer**. It makes sense, doesn't it?      


***


Hope that this article will give you some help.  


