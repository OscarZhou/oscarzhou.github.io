---
title: "How to Implement File Uploading in MVC"
date: 2017-06-06
category: MVC
tags: [.Net, MVC, File uploading]
comments: true

---

## Introduction

Last article, we studied how to implement validation code in MVC, this time, I will introduce the implementation of uploading files in MVC. Uploading files is also very common function in nearly every website, such as upload the profile of your account, some verification file and image and so on. The current websites hardly live without this function.  


## Code

There are two parts consist of this function:  
**1.** Construct the form of uploading files  

First of all, we need to finish the html part. So in folder Views, we create a folder File and Index.cshtml in the File folder.  

    <div class="btn-area">
        @using (Html.BeginForm("UploadFile", "File", FormMethod.Post, new { enctype = "multipart/form-data" }))
        {
            <input class="form-control" type="file" name="file" />
            <input class="btn btn-primary" type="submit" name="upload" value="Submit" />
        }
    </div>

In `Html.BeginForm`, the first parameter is the Action, the second one is the Controller, the third one is method type and the last one is anonymous object that is default value. Just remind it.  

When the type of `<input/>` is file, the interface will show a control that allows you to open a directory dialog. Thus, the interface we get is like below:  
 

<p align="center">
<img src="/images/post/20170606001.png" alt="html upload file" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

***

**2.** Receive the data of files uploaded
  
The mapping function of Action parameter make the file directly map to the type `HttpPostedFileBase`. This type is an abstract class, which includes the members below:  

<table class="table">
    <tr>
        <th>Attribute/Method</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>int ContentLength{get;}</td>
        <td>Get the size of uploading files</td>
    </tr>
    <tr>
        <td>string ContentType{get;}</td>
        <td>Get the content type of MIME of uploading files</td>
    </tr>
    <tr>
        <td>string FileName{get;}</td>
        <td>Get fully qualified name of files in clients</td>
    </tr>
    <tr>
        <td>Stream InputStream{get;}</td>
        <td>Get the object Stream, this object direct an uploaded file</td>
    </tr>
    <tr>
        <td>void SaveAs(string filename)</td>
        <td>Save the content of files uploaded</td>
    </tr>
</table>

### What is the MIME Type?  
MIME 类型就是设定某种扩展名的文件用一种应用程序来打开的方式类型，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。  

Then let's finish the FileController.  

    [HttpPost]
    public ActionResult UploadFile(HttpPostedFileBase file)
    {
        if (file != null)
        {
            // Get the directory of the file
            string filePath = Server.MapPath("~/files/") + file.FileName;
            // Save the file to this path
            file.SaveAs(filePath);
            return Content("<script>alert('Uploading successfully!');location.href='" + Url.Content("~/Files") +
                        "'</script>");
            
        }
        return View("Index");
    }
    
Note that the type of the parameter is `HttpPostedFileBase`. In addition, owing to that the default folder is named "files", we need to create a folder called "files" under the project.     

***

So much more, the function of the uploading file works. Hope that it will help you.  
   




