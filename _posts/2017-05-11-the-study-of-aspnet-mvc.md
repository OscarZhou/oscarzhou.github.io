---
title: "The study of ASP.NET MVC"
date: 2017-05-11
category: .Net
tags: [MVC, .Net, theory, introduction]

---

MVC is the one of the most popular frameworks for web development in ASP.NET. In this article, I will introduce the nature of the MVC by two figures and then explain how to use it by a simple case.  

### **What is the MVC?**  

<p align="center">
  <img src="/images/post/20170511001.jpg" alt=".NET Framework class library" /><br/>
  <center><h4><b>Figure 1</b></h4></center>
</p>

This figure indicates that MVC is one of development tools in .NET platform, which is similar to other framework based on some libraries. So if you want to learn MVC, you had better first understand how to call those support library.

<p align="center">
  <img src="/images/post/20170511002.jpg" alt="Workflow of MVC framework" width="890" height="446" /><br/>
  <center><h4><b>Figure 2</b></h4></center>
</p>

This figure above shows the workflow of MVC framework. M stands for Models, V stands for View and C stands for Controller. I will explain it specifically in the following code.  

***

Before the display of the code, we need to have a look at the file structures of MVC. It is quite important for us to understand the MVC stuff. The figure below is a empty project of MVC 4.

<p align="center">
  <img src="/images/post/20170511003.jpg" alt="The File Structure of MVC framework" /><br/>
  <center><h4><b>Figure 3</b></h4></center>
</p>

***

* **App_Data**: It is used for storing the data files. We don't need to care about it now. 
* **App_Start**: It contains the files that start up the ASP.NET MVC system. Note that the "RouteConfig.cs" file, it is the one of the most important files in MVC framework. It defines the rules of route, which supports the operation of the whole system. I will explain it in a depth later.
* **Controllers**: It is used for storing the code files of all controllers in the whole project.  
* **Models**: It is used for storing the code files of models in the whole project. In fact, they are C# class files.
* **Views**: It is used for storing the code files of views in the whole project.
   
*** 

Note that the "Controllers","Models" and "Views" are the core folders storing code. There are still some other important files, I will give a briefly introduction. 

* **Views/Web.config**: As its name implies, it is the config files working on views.  
* **Global.asax**: It is a global file and usually works with classes in App_Start folder.
* **Packages.config**: It is used for managing the version of the program collection. 
* **Root Folder/Web.config**: It is the config file of the whole project.

Currently, don't feel panic for these files. We will understand them in a gradual way by writing some codes.

***

Next, I will build up a MVC project by a simple web calculator with the purpose of giving you a general view about MVC. The  Let's start.  
<p align="center">
  <img src="/images/post/20170511004.jpg" alt="The File Structure of MVC framework" /><br/>
  <center><h4><b>Figure 4</b></h4></center>
</p>


First of all, we need add a controller called HomeController under the Controller folder. It is should be noted that the name of the controller must be end up with "Controller", like "HomeController". See Fig.4.  

There are three steps guiding you to finish the code in the controller. 

        using System.Web.Mvc;        
        namespace MVCStudy.Controllers
        {
            public class HomeController : Controller
            {
                // GET: /Home/
                public ActionResult Index()
                {
                    //[1] Get the data: receive the data submitted 
        
                    //[2] Process business: call the model to process the data
        
                    //[3] Redirect: return view (If it is necessary to return the data before returning the view, the data must be preserved)
                    return View();
                }
            }
        }

Next, we need add a view. there is another rule. It is that the view folder of the same name with the controller must be established firstly, like "Home". See Fig.4. 
         
         <%@ Page Language="C#" Inherits="System.Web.Mvc.ViewPage<dynamic>" %>
         <!DOCTYPE html>
         <html>
         <head runat="server">
             <meta name="viewport" content="width=device-width" />
             <title>Index</title>
         </head>
         <body>
             <div>
                 <h2>This is my first ASP.NET program!</h2>
             </div>
         </body>
         </html>

Then we create an Index.aspx file. We add a h2 label and run this program. We can see the Fig.5:  
 
<p align="center">
<img src="/images/post/20170511005.jpg" alt="The first MVC program" /><br/>
<center><h4><b>Figure 5</b></h4></center>
</p>

***

You may notice that we only implement V and C. Why the program can be run without M? If you want to know the answer, you must learn the principle of the MVC.  
     
Firstly, let's have a look at the "Global.asax" file and I had commented for the main methods.  

        using System.Web.Http;
        using System.Web.Mvc;
        using System.Web.Routing;
        
        namespace MVCStudy
        {
            // Note: For instructions on enabling IIS6 or IIS7 classic mode, 
            // visit http://go.microsoft.com/?LinkId=9394801
            public class MvcApplication : System.Web.HttpApplication
            {
                // Called when the system starts up
                protected void Application_Start()
                {
                    AreaRegistration.RegisterAllAreas(); // Register all areas in ASP.NET MVC project
        
                    WebApiConfig.Register(GlobalConfiguration.Configuration);  // Register the route of web Api (It is not necessary)
                    FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);  // Global filters
                    RouteConfig.RegisterRoutes(RouteTable.Routes); // Register the route (The current most important file)
                }
            }
        }

If you are careful you will find that these three classes are the files in the App_Start folder. Now what we must master is the class RouteConfig.  

        public class RouteConfig
        {
            // The static method to register the route
            public static void RegisterRoutes(RouteCollection routes)
            {
                routes.IgnoreRoute("{resource}.axd/{*pathInfo}"); // Ignore the specific requests that don't need to be processed
                //This is an anoynmous class, which defines the default controller and action. The parameter Id is optional.
                routes.MapRoute(
                    name: "Default",
                    url: "{controller}/{action}/{id}",    //  Controller name / mehtod( post or get) / id
                    defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional } // default setting, it is like "set this page as a start up page"
                );
            }
        }

The default setting is kind of like "set this page as a start up page in web form application". Thus, that's the reason why the home page will be started up. In fact, the url can be written as "localhost:64733/Home/Index". We can get same result.  We also can define our own route rules.  

### **Summary**  

#### **View visiting and addressing rules** :
  * In Controller, use View() function to call the view and return the view of the same name with action function.
  * Addressing rule: View() will be default to seek the folders that have the same name with controller under Views folders. For example, HomeController will look for "Views/Home" folder.  
  
#### **The convention in MVC** :  
  * Controller: must be end up with "Controller"
  * View: must be under the Views folder and must be created in the sub-folder of the same name with controller.
  
  
#### **The convention should be considered first rather than the configure does**  
  * It is agreed in advance.  
  * The configure file is not needed.  
  * It will be failure if you don't follow the rules.
  
  
***

I will introduce the implementation of the calculator next time. Please go to [The study of ASP.NET MVC (The sample of the calculator)](http://oscarzhou.co.nz/blog/2017/05/14/the-study-of-aspnet-mvc-calculator)








