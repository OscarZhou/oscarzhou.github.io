---
title: "Rich Text Editor(CKEditor)"
date: 2017-06-10
category: MVC
tags: [MVC, CKEditor, Rich Text Editor]
comments: true

---

## Introduction

An online [rich-text editor](https://en.wikipedia.org/wiki/Online_rich-text_editor) is the interface for editing rich text within web browsers, which presents the user with a "what-you-see-is-what-you-get" (WYSIWYG) editing area. The aim is to reduce the effort for users trying to express their formatting directly as valid HTML markup.  


In this article, I will introduce a rich text editor which is simple and easy to use, called CKEditor.
  
## [CKEditor](https://ckeditor.com/)

**1.** Open source  
**2.** Lightweight, no installation  


<p align="center">
<img src="/images/post/20170610001.png" alt="ckeditor" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>
  
The image above shows the result of the rich text editor.  

***

## How to make it?  

### 1.Add the resource files

<p align="center">
<img src="/images/post/20170610002.png" alt="resource file" width="30%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>
   
   
### 2.Import the script of cheditor.js

    <script type="text/javascript" src="@Url.Content("~/ThirdFiles/ckeditor/ckeditor.js")" ></script>

### 3.Replace &lt;TextArea&gt; with ckeditor

    <div class="form-group">
        @Html.TextArea("NewsContent", null, new { @class = "form-control", placeholder = "News Detail" })
        <script type="text/javascript">
            CKEDITOR.replace("NewsContent")
        </script>
    </div>`

### 4.Get Model object from Controller

    [HttpPost]
    public ActionResult PostNews(News objNews)
    {
        // Save the instance News to database

        return View("AddDynamicNews");
    }

So far, we can run the application like thw image above. But when I copy some html code in it, there will be an error shown like this:  

<p align="center">
<img src="/images/post/20170610003.png" alt="error page" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

In order to fix this error, we have to finish the fifth step.  


### 5.Add ASP.NET default security mechanism

    [ValidateInput(false)]
    [HttpPost]
    public ActionResult PostNews(News objNews)
    {
        // Save the instance News to database

        return View("AddDynamicNews");
    }
    
The addition of `[ValidateInput(false)]` will solve this error. 

## Conclusion

Overall, the rich text editor is very useful in certain situation, such as blog system, dashboard and so on. So if you need this kind of function in your website, you may think about CKEditor.  


***
So much more today, hope that this article will give you some help.  





