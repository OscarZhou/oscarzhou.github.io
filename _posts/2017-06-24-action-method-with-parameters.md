---
title: "Action Method with Parameters"
date: 2017-06-24
category: MVC
tags: [MVC, .NET]
comments: true

---

## Introduction

Action method with parameters is one of amazing features in MVC. The operation in the following illustration is very common in our Webform codes.  

<p align="center">
<img src="/images/post/20170624002.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


Sometimes, this kind of code is so time-consuming, especially when your class has too many properties. So what difference can the MVC do? Let's look at this feature step by step.  

***

## Get URL and form data

There are some ways to get the form data, such as `Request.Params["xxx"]` or `Request.QueryString["xxx]`. The illustration below shows that:  


<p align="center">
<img src="/images/post/20170624001.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


So we may want to ask if there is other better way to get the parameters? Here is where the parameter mapping comes into the picture. For example, in the `GetStuList()` method, we use `string className = Request.Params["className"].ToString();` to get the form data `className`. The corresponding html code is like below:  

    <form method="post" action="/Student/GetStuList">
        Class:<input type="text" name="className" id="className" value="<%= ViewBag.className==null?"":ViewBag.className %>"/><br/>
        <input type="submit" value="Query"/>
    </form>

You should note that we add attribute `name` into the `<input>` label. It is necessary attribute when you want to use parameter mapping. Then in server part, we should change our code like below:  

    public ActionResult GetStuList(string className)
     {
         List<Student> stuList = new StudentManager().GetStudentByClass(className);
         ViewBag.className = className;
         ViewBag.stuList = stuList;
         return View("StudentManageRazor");
     }

We add the `string className` into the method parameter list and the parameter name must be as same as the one in `name` attribute in `<input>` label. When we run this program, we will see  

<p align="center">
<img src="/images/post/20170624003.png" alt="object parameters" width="40%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>


<p align="center">
<img src="/images/post/20170624004.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

In another example, the method `GetStuDetail` use `string studentId = Request.QueryString["studId"].ToString();` to get the url parameters. The corresponding html code is like this:  

    <div class="stuItem">
        <a href="/Student/GetStuDetail/?studId=@stu.StudentId">Detail</a>
    </div>


If we want to use parameter mapping method, we need to add input parameter and the name of this parameter should be `studId`, which is the same as the one in the url.  

    public ActionResult GetStuDetail(string studId)
    {
        Student objStudent = new StudentManager().GetStudentById(studId);
        ViewData["objStudent"] = objStudent;
        return View("StudentDetail");
    }
    
It is the same thing actually. Only in this way can the feature work.      

<p align="center">
<img src="/images/post/20170624005.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

    
See, we also can get the same result.  

**The strength of the parameter mapping:** You do not need to get the parameters and convert the type manually. The advantage is more obvious in many parameters.
  
  
*** 


## The rule of parameter mapping

In previous two changes, we can see that we get the data by using two different methods. Thus, here is a question that is there priority in the parameter mapping? The answer is yes.  



<table class="table">
    <tr>
        <th>Source</th>
        <th>Priority</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Form data</td>
        <td>High</td>
        <td>The data comes from Request.Form</td>
    </tr>
    <tr>
        <td>Route data</td>
        <td>Middle</td>
        <td>The data comes from RouteData.Values</td>
    </tr>
    <tr>
        <td>URL data</td>
        <td>Low</td>
        <td>The data comes from Request.QueryString</td>
    </tr>
</table>


**The requirement of action method's parameters** 
1. Making sure that the name of input parameters should be as same as the parameter name of the objective data, but it is not case sensitive.    
2. Ensuring that the data type should be the same with the objective data type of the source data.  

## Mapping Model

This part is the coolest part in this article. Like the first illustration showed that sometimes we will create a model object and assign the values obtained from the form. But when the parameters on the form is too many, this task will make a lot of trouble. I put the Fig.1 again to help us have a good view.  
 
<p align="center">
<img src="/images/post/20170624002.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

But in our case, we want to use model mapping, so we need to change the code like this:  

    public ActionResult Edit(Student objStudent)
    {
        int result = new StudentManager().ModifyStudent(objStudent);
        return Content("<script>alert('Modify sucessfully');document.location='" + Url.Action("Index", "Student") +
                       "'</script>");
    }

If we use the input parameters like this, the miracle will happen. The miracle is necessary rather than accident because there are some changes behind it. The corresponding html code should absolutely be changed.  
 
    <form method="post" action="/Student/Edit">
        <ul>
            <li>Id:<input type="text" readonly="readonly" name="StudentId" value="@Model.StudentId"/></li>
            <li>Name:<input type="text" name="StudentName" value="@Model.StudentName"/></li>
            <li>Gender:<input type="radio" name="Gender" checked="@Model.Gender.Equals("男")" value="男"/>男
                <input type="radio" name="Gender" checked="@Model.Gender.Equals("女")" value="女" />女</li>
            <li>Birthday:<input type="text" name="Birthday" value="@Model.Birthday"/></li>
            <li>Identify No:<input type="text" name="StudentIdNo" value="@Model.StudentIdNo"/></li>
            <li>Phone Number:<input type="text" name="PhoneNumber" value="@Model.PhoneNumber"/></li>
            <li>Address:<input type="text" name="StudentAddress" value="@Model.StudentAddress"/></li>
            <li><input type="submit" value="Submit"/></li>
        </ul>
    </form>
    
We should make sure that the value of the `name` attribute should be consistent with the declaration of the properties in the Model. For example, the class `Student` is created like this:  
    
    public class Student
    {
        public int StudentId { get; set; }
    
        public string StudentName { get; set; }
    
        public string Gender { get; set; }
    
        public DateTime Birthday { get; set; }
    
        public string StudentIdNo { get; set; }
    
        public int Age { get; set; }
    
        public string PhoneNumber { get; set; }
    
        public string StudentAddress { get; set; }
    
        public string CardNo { get; set; }
    
        public int ClassId { get; set; }
    }


When I edit the detail of one of students, I changed gender and phone number.  

 
<p align="center">
<img src="/images/post/20170624006.png" alt="object parameters" width="50%"  /><br/>
<center><h5><b>Figure 7</b></h5></center>
</p>


And we track of the breakpoint, we will see that the `objStudent` get the form data, which means mapping successfully.  
 
  
<p align="center">
<img src="/images/post/20170624007.png" alt="object parameters" width="70%"  /><br/>
<center><h5><b>Figure 8</b></h5></center>
</p>

Using this feature, we can save a lot of work. This will be one of reasons that I use MVC as my development framework because it makes the work simple and easy.  



***