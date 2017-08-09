---
title: "The use of dynamic"
date: 2017-05-30
category: C#
tags: [C#, MVC, Dynamic]
comments: true

---

## The feature of the keyword "dynamic"

In the course of compiling, the system won't check. Instead, the system will check during the running of the application. If the member exists and the parameter is correct, the application will run normally.  
    
    public ActionResult GetStuList()
    {
        string className = Request.Params["className"].ToString();
        List<Student> stuList = new StudentManager().GetStudentByClass(className);
        ViewBag.className = className;
        ViewBag.stuList = stuList;
        return View("StudentManageRazor");
    }

In MVC, it is very common to use `ViewBag`. The reason why we can assign a value like that is because of the dynamic feature. The system allows you to assign a value without fixed type. However, the system will check when the application runs.  
 
    <div class="stuList">
        <%
            if (ViewBag.stuList != null)
            {
                foreach (Models.Student stu in ViewBag.stuList)
                {
                    %>
                <div class="stuItem">
                    <%= stu.StudentId %>
                </div>
                <div class="stuItem">
                    <%= stu.StudentName %>
                </div>
                <div class="stuItem">
                    <%= stu.Gender %>
                </div>
                <div class="stuItem">
                    <a href="#">Detail</a>
                </div>
                <div class="stuItem">Modify</div>
                <div class="stuItem">Delete</div>
                    <%
                }
            }
             %>
    </div>


## The conclusion of dynamic type

**1.** Can be applied in field, method parameters, return value and generic type parameters.  
**2.** Can be assigned a value and any types and do not need to be convert the type.  

The example is shown below:  

        static void Main(string[] args)
        {
            dynamic person1 = new Student {StudentId = 10001, StudentName = "John"};
            person1.GetInfo();

            dynamic person2 = new Teacher {TeacherId = 20000, TeacherName = "Katy"};
            person2.GetInfo();
        }

        class Student
        {
            public int StudentId { get; set; }

            public string StudentName { get; set; }

            public void GetInfo()
            {
                Console.WriteLine("Name:{0} Student No:{1} ");
            }
        }

        class Teacher
        {
            public int TeacherId { get; set; }

            public string TeacherName { get; set; }

        }

As you see, class Student has the method GetInfo, and class Teacher doesn't. However, when you compile it, there won't be error. When you run it, the result is like below:  


<p align="center">
<img src="/images/post/20170530001.png" alt="dynamic" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


