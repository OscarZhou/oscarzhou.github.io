---
title: "The error handler in the entire site"
date: 2017-06-14
category: MVC
tags: [MVC, .NET, Error Handler]
comments: true

---

## Introduction

As we all know, during coding, we can use `try catch` to filter some errors, but there are some limitation. 

**1.** It is only suitable for the error handling of the statements.   
**2.** It is not suitable for error handling of some problems like no page exists. For example, when user submit the request, but if the url is wrong, it definitely can not detect this error.  
  
So if we want to figure out such problems, how should we do?  

## Configure error handler in the `Web.config` file  

First of all, we need to add some new pages(.html files) as the pages of error handling.  

<p align="center">
<img src="/images/post/20170614001.png" alt="files list" width="30%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

These files are used for showing the error pages. That is, when the error of the pages occurs, instead of showing the error information pages designed by browser, we can customize the error information's pages, which improves users' interface experience.  

    <!DOCTYPE html>
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>No Found Pages</title>
        <link href="Styles/bootstrap.css" rel="stylesheet" />
        <style type="text/css">
            body {
                margin: 0 auto;
            }
        </style>
    </head>
    <body class="col-lg-6 col-lg-push-3">
        <h2>This page does not exist!</h2>
    </body>
    </html>


The typical configuration of error handling in `web.config` (under `system.web`).  
     
    <customErrors defaultRedirect="~/ErrorPage.html" mode="On">
      <error statusCode="404" redirect="~/FileNotFound.html"/>
    </customErrors>


**[Parameters Descrition](https://msdn.microsoft.com/en-us/library/h0hfz6fc(v=vs.85).aspx):**  


<table class="table">
    <tr>
        <th>Attribute</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>defaultRedirect</td>
        <td>Specifies the default URL that is redirected by a broweser when an error occurs.</td>
    </tr>
    <tr>
        <td>mode</td>
        <td>
            <li>
                <b>On:</b> Enable custom errors
            </li>
            <li>
                <b>Off:</b> Disable custom errors
            </li>
            <li>
                <b>RemoteOnly:</b> Specifies that custom errors are shown only to the remote clients, and that ASP.NET errors are shown to the local host
            </li>
        </td>
    </tr>
    <tr>
        <td>statusCode</td>
        <td>HTTP's specific error status code.
            <li>
                <b>403:</b> disabled access
            </li>
            <li>
                <b>404:</b> not found file
            </li>
            <li>
                <b>500:</b> server error
            </li>
        </td>
    </tr>
    <tr>
        <td>redirect</td>
        <td>Specifies the page that is redirected by a broweser when the current error occurs.</td>
    </tr>
</table>

So when I try to access the non-exist page, the browser will detect the 404 error. But we should note that when we debug, the mode should be set "on", but after we deploy the site, we need to modify it to "RemoteOnly". Then it will redirect the pages we just set. Like this:  

<p align="center">
<img src="/images/post/20170614002.png" alt="404 Error" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

Nowadays, almost all large sites have their own error pages, but it's really useful, isn't it?   

***

## Use `HandleError` 
  
For `web.config`, there are also some limitation:  

**1.** 这里的修改是针对整个站点的错误，配置错误显示页面，但是这些信息显示一般比较模糊  
**2.** 如果针对某种类型的控制器，甚至动作方法，显示更具体的信息，使用web.config方式不合适  
**3.** 使用`try catch` 也不适合（业务代码和错误代码混编） 
  
**`HandleError`:** a better way that the error is displayed according to the action.  
  
Let's have a look at how to implement it.  

    [HandleError(ExceptionType = typeof(System.Exception), View = "Error")]
    public ActionResult GetList(string queryClass)
    {
        List<Student> objStudents = new StudentManage().GetListByClassId(queryClass);
        ViewBag.className = queryClass;
        ViewBag.stuList = objStudents;

        return View("StudentManage");
    }

The **ExceptionType** stands for the type of catching the exception, and the **View** stands for the specific error view. And then let's finish the "Error" page. Note that we can give more detail about the errors.  

    <div>
        <h2>Sorry, there is an error when we process your request. Please contact administration!</h2>
        <h3>Error source:</h3>
        <div>
            <ul>
                <li>Controller Name:@Model.ControllerName</li>
                <li>Action Name:@Model.ActionName</li>
            </ul>
        </div>
        <h3>
            Excepton information:
        </h3>
        <div style="overflow: auto;">
            @Model.Exception.Message
        </div>
    </div>

### The principle of the implementation

When the error occurs, `HandleError` will encapsulate exception information into the object `HandleErrorInfo`, then return to "Error" view. In order to activate this error page, I add `throw new Exception();` into the previous action, and then you will see the result below:  

<p align="center">
<img src="/images/post/20170614003.png" alt="Handle Error" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

Note that after using this feature, the configuration in web.config will be invalid. In addition, if you want to check this page on your own computer, you have to modify the parameter "debug" in the web.config to false. Otherwise, the page won't be activated.  
  
`<compilation debug="false" targetFramework="4.5" />`
  
***
So much more today, hope that this article will give you some help.  





