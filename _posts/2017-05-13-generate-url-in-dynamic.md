---
title: "Generate the URL by Route"
date: 2017-05-13
category: MVC
tags: [mvc, url]
comments: true

---

## Introduction

URL is an indispensable part in web development. There are two different links, one is **static link**, which directly points to the link address and is not necessary to change. Another one is **dynamic link**, in order to ensure that the route is correct when the web project are translated.  
 

In MVC, we usually write static link by using `@Url.Content("***")`, like `<img src="@Url.Content(~/UploadFile/1001.png)"/>` or `<a href="@Url.Content(~/UploadFile/1001)"`.  
  
The shortage of the static link is when the route rule changes, I have to modify all the route manually. It is really terrible. So dynamic link comes up to solve this kind of problem. When the route rule changes, URL can apply the new route rule automatically.  


*** 
 

## The methods of generating dynamic link

There are four kinds of methods to generate link dynamic. I wil show you one by one.  
 
**1.Url.Action()**

    /// <summary>Generates a fully qualified URL to an action method by using the specified action name, controller name, and route values.</summary>
    /// <returns>The fully qualified URL to an action method.</returns>
    /// <param name="actionName">The name of the action method.</param>
    /// <param name="controllerName">The name of the controller.</param>
    /// <param name="routeValues">An object that contains the parameters for a route. The parameters are retrieved through reflection by examining the properties of the object. The object is typically created by using object initializer syntax.</param>
    public string Action(string actionName, string controllerName, object routeValues)
 
You should note that the `routeValues` is an anonymous object. In fact, there are other override methods, like  

    public string Action(string actionName)
    
    public string Action(string actionName, string controllerName)


**Example**  
The route rule of url is `HotelAdmin/{controller}/{action}/{id}`. When I run `<a href=@Url.Action("ModifyDish", "Dishes", new {DishId = item.DishId}) id="dish-modify" >修改</a>`, the result is  

<p align="center">
<img src="/images/post/20170513001.png" alt="url.action" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

Not bad, yeah? Let's move to next one.  

**2.Html.ActionLink()**  
    
    /// <summary>Returns an anchor element (a element) that contains the virtual path of the specified action.</summary>
    /// <returns>An anchor element (a element).</returns>
    /// <param name="htmlHelper">The HTML helper instance that this method extends.</param>
    /// <param name="linkText">The inner text of the anchor element.</param>
    /// <param name="actionName">The name of the action.</param>
    /// <param name="routeValues">An object that contains the parameters for a route. The parameters are retrieved through reflection by examining the properties of the object. The object is typically created by using object initializer syntax.</param>
    /// <param name="htmlAttributes">An object that contains the HTML attributes for the element. The attributes are retrieved through reflection by examining the properties of the object. The object is typically created by using object initializer syntax.</param>
    /// <exception cref="T:System.ArgumentException">The <paramref name="linkText" /> parameter is null or empty.</exception>
    public static MvcHtmlString ActionLink(this HtmlHelper htmlHelper, string linkText, string actionName, object routeValues, object htmlAttributes)
    {
      return LinkExtensions.ActionLink(htmlHelper, linkText, actionName, (string) null, new RouteValueDictionary(routeValues), (IDictionary<string, object>) HtmlHelper.AnonymousObjectToHtmlAttributes(htmlAttributes));
    }
    
I can use this method in this way `@Html.ActionLink("删除", "DeleteDish", "Dishes", new {@class="dish-attr"})`

<p align="center">
<img src="/images/post/20170513002.png" alt="html.action" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


It's easy, isn't it?  

**3.Html.RouteLink()**
    
    /// <summary>Returns an anchor element (a element) that contains the virtual path of the specified action.</summary>
    /// <returns>An anchor element (a element).</returns>
    /// <param name="htmlHelper">The HTML helper instance that this method extends.</param>
    /// <param name="linkText">The inner text of the anchor element.</param>
    /// <param name="routeValues">An object that contains the parameters for a route. The parameters are retrieved through reflection by examining the properties of the object. The object is typically created by using object initializer syntax.</param>
    /// <exception cref="T:System.ArgumentException">The <paramref name="linkText" /> parameter is null or empty.</exception>
    public static MvcHtmlString RouteLink(this HtmlHelper htmlHelper, string linkText, object routeValues)
    {
      return LinkExtensions.RouteLink(htmlHelper, linkText, new RouteValueDictionary(routeValues));
    }
    
Actually it is okay to replace `Html.ActionLink()` with `Html.RouteLink()`.  
`@Html.RouteLink("删除", new { controller= "Dishes", action="DeleteDish", DishId=item.DishId}, new {@clas="dish-route"})`  

<p align="center">
<img src="/images/post/20170513003.png" alt="html.action" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>


**4.Html.Action()**  

    /// <summary>Invokes the specified child action method using the specified parameters and controller name and returns the result as an HTML string.</summary>
    /// <returns>The child action result as an HTML string.</returns>
    /// <param name="htmlHelper">The HTML helper instance that this method extends.</param>
    /// <param name="actionName">The name of the action method to invoke.</param>
    /// <param name="controllerName">The name of the controller that contains the action method.</param>
    /// <param name="routeValues">An object that contains the parameters for a route. You can use <paramref name="routeValues" /> to provide the parameters that are bound to the action method parameters. The <paramref name="routeValues" /> parameter is merged with the original route values and overrides them.</param>
    /// <exception cref="T:System.ArgumentNullException">The <paramref name="htmlHelper" /> parameter is null.</exception>
    /// <exception cref="T:System.ArgumentException">The <paramref name="actionName" /> parameter is null or empty.</exception>
    /// <exception cref="T:System.InvalidOperationException">The required virtual path data cannot be found.</exception>
    public static MvcHtmlString Action(this HtmlHelper htmlHelper, string actionName, string controllerName, object routeValues)
    {
      return ChildActionExtensions.Action(htmlHelper, actionName, controllerName, new RouteValueDictionary(routeValues));
    }
    
    
We can use the method in this way `@Html.Action("DeleteDish", "Dishes", new { DishId = "111" } )`. One thing we should note is that when the program starts, the program will go to the default action first and then go to this generated link before the view is established. Thus, before `Return View()`, you should make sure that you have already prepared all the variables for the view.  

***

In my opinion, I prefer to `Html.RouteLink()` in real project, which is flexible and clear.  


