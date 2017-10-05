---
title: "The Explanation of ActionResult"
date: 2017-05-15
category: MVC
tags: [mvc, ActionResult]
comments: true

---

## Introduction

We all know that `ActionResult` is the return type in the controller and we usually use `return View()` to return the result. So we need to think about one thing, which is that what the relation between `ActionResult` and `View`.  

<p align="center">
<img src="/images/post/20170515001.png" alt="view" width="80%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

From the illustration above, we can see that the return type of `View` is actually `ViewResult`, which is not `ActionResult`. When I search `ViewResult` in **Assembly Explorer** in Visual Studio. I found that `ViewResult` inherits `ViewResultBase`, and `ViewResultBase` inherits `ActionResult`. So it makes sense that `ActionResult` is able to be the return type of `View`, as the base class can be the return type of any children class. This is also one feature of the polymorphism.  

***

## Other return methods of ActionResult

As I said above, `View` is one of child classes of the `ActionResult`. Obviously, there must be other child classes of the `ActionResult`. Thus, there are also other ways to return result. The difference is that what they return are not a view. I will list different output types and methods to show how powerful the MVC is.  


<table class='table'>
    <tr>
        <th>Output type</th>
        <th>Description</th>
        <th>Controller ouput method</th>
    </tr>
    <tr>
        <td>EmptyResult</td>
        <td>No return result, the action is not necessary to ouput</td>
        <td>None</td>
    </tr>
    <tr>
        <td>ContentResult</td>
        <td>Output text</td>
        <td>Content(string content) </td>
    </tr>
    <tr>
        <td>JsonResult</td>
        <td>Output Json string</td>
        <td>Json(object data) </td>
    </tr>
    <tr>
        <td>JavaScriptResult</td>
        <td>Output Javascript files</td>
        <td>JavaScript(string script) </td>
    </tr>
    <tr>
        <td>RedirectResult</td>
        <td>Specify the URL to redirect</td>
        <td>Redirect(string url) </td>
    </tr>
    <tr>
        <td>RedirectToRouteResult</td>
        <td>Specify the route value to redirect</td>
        <td>RedirectToAction(string actionName) </td>
    </tr>
    <tr>
        <td>FilePathResult</td>
        <td>Specify the file path to output the file. Inherits class FileResult</td>
        <td>File(string fileName, string contentType) </td>
    </tr>
    <tr>
        <td>FileContentResult</td>
        <td>Specify the number of bytes to output file. Inherits class FileResult</td>
        <td>File(byte[] fileContents, string contentType) </td>
    </tr>
    <tr>
        <td>FileStreamResult</td>
        <td>Specify the stream to output the file. Inherits class FileResult</td>
        <td>File(Stream fileStream, string contentType) </td>
    </tr>
    <tr>
        <td>ViewResult</td>
        <td>Call the view file to ouput view</td>
        <td>View(string viewName) </td>
    </tr>
</table>


***

## Examples

Next, I will show the specific use of these methods.  

**1. ContentResult**  

Html:  
    
    <p>
        @Html.RouteLink("Output text", new {controller = "ActionResult", action="ContestTest"})
    </p>

C#:  

    public ActionResult ContestTest()
    {
        string content = "<h1>Welcome to ASP.NET MVC</h1>";
        return Content(content);
    }

The result:  

<p align="center">
<img src="/images/post/20170515002.png" alt="ContentResult" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


**2. JsonResult**  


Html:  
    
    <p>
        @Html.RouteLink("Output Json string", new {controller="ActionResult", action="JsonTest"})
    </p>

C#:  
    
    public ActionResult JsonTest()
    {
        var book = new
        {
            bookId = 1,
            bookName = "ASP.NET MVC4",
            author = "John",
            publishData = DateTime.Now
        };
        return Json(book, JsonRequestBehavior.AllowGet);
    }

The result:  

<p align="center">
<img src="/images/post/20170515003.png" alt="JsonResult" width="50%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

**3. JavaScriptResult**  

Html:  
    
    <p>
        <script src="@Url.RouteUrl(new{action="JavascriptTest"})" type="text/javascript"></script>
    </p>

When the program runs, the reference of this javascript file will be executed.

C#:  

    public ActionResult JavascriptTest()
    {
        string js = "alert('Welcome to ASP.Net MVC')";
        return JavaScript(js);
    }

The result:  

<p align="center">
<img src="/images/post/20170515004.png" alt="JavaScriptResult" width="50%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

**4. RedirectResult**  

Html:  
    
    <p>
        <script src="@Url.RouteUrl(new{action="JavascriptTest"})" type="text/javascript"></script>
    </p>

Detail html:  

    <div>
        @foreach (KeyValuePair<string, object> data in this.ViewContext.RouteData.Values)
        {
            <p>@data.Key:@data.Value</p>
        }
    </div>
    
C#:  

    public ActionResult RedirectTest()
    {
        return Redirect("/SysAdmin/Index");
    }

    public ActionResult Detail()
    {
        return View("Detail");
    }
    
The result:  

<p align="center">
<img src="/images/post/20170515005.png" alt="RedirectResult" width="50%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>


**5. RedirectToRouteResult**  

Html:  
    
    <p>
        @Html.RouteLink("Redirect to the action", new { controller = "ActionResult", action = "RedirectToRouteResultTest" })
    </p>

C#:  
    
    public ActionResult RedirectToRouteResultTest()
    {
        return RedirectToAction("Detail", new {id = 1});
    }
    

The result:  

<p align="center">
<img src="/images/post/20170515006.png" alt="RedirectToRouteResult" width="50%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

</br>    

**6. FilePathResult**  
Html:  
    
    <p>
        @Html.RouteLink("Play mp3", new { controller = "ActionResult", action = "FilePathTest" })
    </p>

C#:  
    
    public ActionResult FilePathTest()
    {
        return File("~/Content/abc.mp3", "audio/mp3");
    }
    
</br>    
    
**7. FilePathResult**  

Html:  
    
    <p>
        @Html.RouteLink("View text file", new { controller = "ActionResult", action = "FileContentTest" })
    </p>

C#:  
    
    public ActionResult FileContentTest()
    {
        string content = "Welcome";
        byte[] contents = System.Text.Encoding.UTF8.GetBytes(content);
        return File(contents, "text/plain");
    }
  
</br>    
    
**8. FileStreamResult**  
Html:  
    
    <p>
        @Html.RouteLink("View text file", new { controller = "ActionResult", action = "FileContentTest" })
    </p>

C#:  
    
    public ActionResult FileStreamTest()
    {
        string content = "Welcome";
        byte[] contents = System.Text.Encoding.UTF8.GetBytes(content);

        FileStream fs= new FileStream(Server.MapPath("~/Content/abc.pdf"), FileMode.Open);
        return File(fs, "application/pdf");
    }
  
***

This is only a general idea, when you need to use some specific methods, you will have all the output file, like mp3, pdf and so on, then you will get it.  



