---
title: "How to Implement DropDownList in MVC"
date: 2017-06-08
category: MVC
tags: [.Net, MVC, DropDownList, Static, Dynamic]
comments: true

---

## Introduction

Today I will show how to implement the static and dynamic DropDownList in MVC. Certainly, we will use the HtmlHelper to implement rather than using the html label, the conventional way.  
  
**1.** [Static DropDownList](#1)  

**2.** [Dynamic DropDownList](#2)  

***

<h2 id="1">Static DropDownList</h2>

In ASP.NET MVC, the achieve idea of the static DropDownList can be divided up into two parts:  

### 1. Construct Data Source

The data source of DropDownList is `IEnumerable<SelectListItem>`. This is used for directly fix contents in the view, which fits the static DropDownList.  

<p align="center">
<img src="/images/post/20170608001.png" alt="SelectListItem" width="20%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

The source code is:  

    /// <summary>Represents the selected item in an instance of the <see cref="T:System.Web.Mvc.SelectList" /> class.</summary>
    public class SelectListItem
    {
    /// <summary>Gets or sets a value that indicates whether this <see cref="T:System.Web.Mvc.SelectListItem" /> is selected.</summary>
    /// <returns>true if the item is selected; otherwise, false.</returns>
    public bool Selected { get; set; }
    
    /// <summary>Gets or sets the text of the selected item.</summary>
    /// <returns>The text.</returns>
    public string Text { get; set; }
    
    /// <summary>Gets or sets the value of the selected item.</summary>
    /// <returns>The value.</returns>
    public string Value { get; set; }
}

*** 

### 2. Using View Helper: `Html.DropDownList()`

#### In Views:  

    @using (Html.BeginForm("AddStaticNews", "File", FormMethod.Post))
    {
        var items=new List<SelectListItem>();
        items.Add(new SelectListItem(){ Text = "Education", Value = "1"});
        items.Add(new SelectListItem(){ Text = "Media", Value = "2"});
        items.Add(new SelectListItem(){ Text = "Vehicle", Value = "3"});
        items.Add(new SelectListItem(){ Text = "Military", Value = "4"});
        <table class="table">
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="title">Title</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @Html.TextBox("NewsTitle", null, new {@class="form-control", placeholder="Title"})
                    </div>
                </td>
            </tr>
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="category">Category</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @Html.DropDownList("NewsCategory", items, "Please Select", new {@class="form-control"})
                    </div>
                </td>
            </tr>
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="detail">Detail</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @Html.TextArea("NewsContent", null, new { @class = "form-control", placeholder = "News Title" })
                    </div>
                </td>
            </tr>
        </table>   
    <div class="btn-area">
        <button type="submit" class="btn btn-primary">Post News</button>
        <a href="@Url.Action("Index", "File")"><input type="button" class="btn btn-primary" value="Back" /></a>    
    </div>
    }

The effect is  

<p align="center">
<img src="/images/post/20170608002.png" alt="static dropdownlist" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


#### In Controller:
      
    public ActionResult AddStaticNews()
    {
        // Submit Form Operation
        
        return View("AddStaticNews");
    }

Since I just show how to implement the static DropDownList, so submitting form parts will be ignored.   

***
<h2 id="2">Dynamic DropDownList</h2>

We will use `SelectList` as the data source of the dynamic DropDownList. This data source is used for dynamically display list from database or other collection.  
    
The construct method is  
`public SelectList(IEnumerable items, string dataValueField, string dataTextField, object selectedValue)`  
This first parameter is data source, the second one is the field of the data value, the third one is the field of data text and the last one is selected value.  

### The implementation Process

#### In Models:  

    public class NewsCategory
    {
        public int CategoryId { get; set; }

        public string CategoryName { get; set; }

        public NewsCategory(int id, string name)
        {
            this.CategoryId = id;
            this.CategoryName = name;
        }
    }

#### In DAL:  
    
    public class NewsService
    {
        public List<NewsCategory> GetNewsCategory()
        {
            List<NewsCategory> list = new List<NewsCategory>();
            list.Add(new NewsCategory(1,"Education"));
            list.Add(new NewsCategory(2,"Media"));
            list.Add(new NewsCategory(3,"Vehicle"));
            list.Add(new NewsCategory(4,"Military"));

            return list;
        }
    }
    
Generate the data source.  

#### In BLL:  

    public class NewsManage
    {
        public List<NewsCategory> GetNewsCategory()
        {
            return new NewsService().GetNewsCategory();
        }
    }

#### In Controller:  

    public ActionResult AddDynamicNews()
    {
        List<NewsCategory> objCategories = new NewsManage().GetNewsCategory();
        SelectList list = new SelectList(objCategories, "CategoryId", "CategoryName", objCategories[0].CategoryId);
        ViewBag.Category = list;
        return View("AddDynamicNews");
    }
    
What I did in this action is firstly to get the information of all news category and then save it into `ViewBag.Category`, because we need to display the data in Views.       

#### In Views:  

    @using (Html.BeginForm("AddDynamicNews", "File", FormMethod.Post))
    {
        <table class="table">
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="title">Title</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @Html.TextBox("NewsTitle", null, new { @class = "form-control", placeholder = "News Title" })
                    </div>
                </td>
            </tr>
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="category">Category</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @* the second parameter is *@
                        @Html.DropDownList("NewsCategory", (SelectList)ViewBag.Category, "Please Select", new { @class = "form-control" }) 
                    </div>
                </td>
            </tr>
            <tr>
                <td>
                    <div class="form-group">
                        <label class="control-label" for="detail">Detail</label>
                    </div>
                </td>
                <td>
                    <div class="form-group">
                        @Html.TextArea("NewsContent", null, new { @class = "form-control", placeholder = "News Detail" })
                    </div>
                </td>
            </tr>
        </table>
        <div class="btn-area">
            <button type="submit" class="btn btn-primary">Post News</button>
            <a href="@Url.Action("Index", "File")"><input type="button" class="btn btn-primary" value="Back" /></a>    
        </div>
    }

Note that I convert the second parameter of `@Html.DropDownList` to `SelectList`. And you can see the result.  

<p align="center">
<img src="/images/post/20170608003.png" alt="dynamic dropdownlist" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

***

## The Glimpse of The `HtmlHelper`
  
  
<table class="table">
    <tr>
        <th>Method</th>
        <th>Corresponding Html</th>
    </tr>
    <tr>
        <td>BeginForm(string actionName, string controllerName)</td>
        <td>&lt;form/&gt;</td>
    </tr>
    <tr>
        <td>TextBox(string name)</td>
        <td>&lt;input type="text" /&gt;</td>
    </tr>
    <tr>
        <td>DropDownList(string name)</td>
        <td>&lt;select/&gt;</td>
    </tr>
    <tr>
        <td>TextArea(string name)</td>
        <td>&lt;textarea/&gt;</td>
    </tr>
</table>


***
So much more today, hope that this article will give you some help.  





