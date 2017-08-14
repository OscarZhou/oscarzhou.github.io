---
title: "Identity Authentication and Authorization in MVC"
date: 2017-06-12
category: MVC
tags: [MVC, Identity Authentication, Authorization]
comments: true

---

## Introduction

In .NET MVC, the mechanism of identity authentication and authorization is one of the most important functions. In the past, I only use Session to maintain the current user. However, I do not know other easier and purer approaches. I told myself that I'd better know some specific mechanism of .NET.  

***
 
## Identity Authentication and Session

Save user state and information based on Session, like user login information equivalent to authorization. When browsing the specific pages, if system detects that no users login, it will ban some users' operation.  

**Weakness:**   
**1.** The Session has the life cycle, when time is out, the user must re-login.  
**2.** The Session probably lose, such as the restart of the server, Ram recycle etc, which influence users' experience.  

***

## The Ways of Identity Authentication in ASP.NET

<table class="table">
    <tr>
        <th>Authentication Ways</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Windows</td>
        <td>Authenticate with Windows Operating System and NTFS files system(suit for company inner site and not good for commercial site)</td>
    </tr>
    <tr>
        <td>Forms</td>
        <td>Send proof to the Client with web pages, the Client then submit proof to application for identity authentification(most common)</td>
    </tr>
    <tr>
        <td>Passport</td>
        <td>A single sign-on standard(offered by Microsoft)</td>
    </tr>
    <tr>
        <td>Federated</td>
        <td>A single sign-on standard(offered by Google)</td>
    </tr>
</table>


**Forms Authentication**
1. The use in real project is the most common.    
2. Developed by Amazon intially, use the Cookie to maintain the state among pages.  
3. Offer `FormsAuthentication` that is specifically used for idnetity authentication in ASP.NET MVC.  
4. One of `FormsAuthentication`'s functions is to write into a Cookie which marks user's identity.  
  
***

## The Class of Authenticating Forms

**[FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication(v=vs.110).aspx):**  Namespace: `System.Web.Security`


<table class="table">
    <tr>
        <th>Attributes or Methods</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>string LoginUrl</td>
        <td>Gets the URL for the login page that the FormsAuthentication class will redirect to.</td>
    </tr>
    <tr>
        <td>TimeSpan Timeout</td>
        <td>Gets the amount of time before an authentication ticket expires.</td>
    </tr>
    <tr>
        <td>void SetAuthCookie(string userName, bool createPersistentCookie)</td>
        <td>Creates an authentication ticket for the supplied user name and adds it to the cookies collection of the response, using the supplied cookie path, or using the URL if you are using cookieless authentication.</td>
    </tr>
    <tr>
        <td>void SignOut()</td>
        <td>Removes the forms-authentication ticket from the browser.</td>
    </tr>
    <tr>
        <td>string Encrypt(FormsAuthenticationTicket ticket)</td>
        <td>Creates a string containing an encrypted forms-authentication ticket suitable for use in an HTTP cookie.</td>
    </tr>
    <tr>
        <td>FormsAuthenticationTicket Decrypt(string encryptedTicket)</td>
        <td>Creates a FormsAuthenticationTicket object based on the encrypted forms-authentication ticket passed to the method.</td>
    </tr>
</table>


***

## Code 

So far, we have already known some basic ideas about identity authentication and authorization in ASP.NET. Next, we will start the implementation of this mechanism.  
 
### Login
First of all, we modify the codes of the login part. In SysAdmin Controller,  

    [HttpPost]
    public ActionResult Login(SysAdmin objSysAdmin)
    {
        //Call the business logic
        objSysAdmin = new SysAdminManage().Login(objSysAdmin);
        if (objSysAdmin.AdminName != null)
        {
            //为当前用户名提供一个身份验证票证，并将该票证添加到Cookie
            FormsAuthentication.SetAuthCookie(objSysAdmin.AdminName, false);
            ViewData["info"] = "welcome: " + objSysAdmin.AdminName;
            return RedirectToAction("Index", "Student");
        }
        else
        {
            ViewData["info"] = "user name or password error!";
            return View("AdminLogin");
        }
    }

***

### Page detection

After signing up successfully, we have to check the user state in every pages. In ASP.NET MVC, we use the object User to check if the user is authenticated.  
1. The User object encapsulates the proof of users' identity.  
2. Used for controlling the code permission.  

Take StudentManagement as an example. In Student Controller,  

    public ActionResult Index()
    {
        if (this.User.Identity.IsAuthenticated)
        {
            string adminName = this.User.Identity.Name;
            ViewBag.adminName = adminName;
            return View("StudentManage");
        }
        else
        {
            return RedirectToAction("Index", "SysAdmin");
        }
    }


As the matter stands, we still note two things.  

**1.** For `FormsAuthentication.SetAuthCookie(objSysAdmin.AdminName, false);`, the second parameter determines if create a persistent code. If it is false, we have to modify the web.config file.  
 
    <authentication mode="Forms">
      <forms loginUrl="~/SysAdmin/Index" timeout="5"></forms>
    </authentication>
    
loginUrl and timeout are two readonly attributes offered by `FormsAuthentication`.  
**mode:** Authentification Ways(Forms, None, Password, Windows)  
**loginUrl:** If the authentification does not pass when users access the pages, the page will redirect to this url( it is usually login page)  
**timeout:** The valid period of Cookies. The value stands for minutes.  

**2.** In real project, the first page always the content page rather than annoying login page. Thus, we need to modify the default value of route.   

    public static void RegisterRoutes(RouteCollection routes)
    {
        routes.IgnoreRoute("{resource}.axd/{*pathInfo}");
        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "SysAdmin", action = "Index", id = UrlParameter.Optional }
        );
    }

*** 

### Log Out

In Views,  

    <p class="bg-primary">
        Current User:  @ViewBag.adminName
        <a href="@Url.Action("adminExit", "SysAdmin")">Log out</a>
    </p>

In Controller,  

    public ActionResult AdminExit(SysAdmin objSysAdmin)
    {
        FormsAuthentication.SignOut();//Delete the proof of identity authentication from browser. 
        Session["CurrentAdmin"] = null;// Clear the Session 
        return View("AdminLogin");// Return login pages
    }

***

### User the feature `Authrize` to implement authorization
 
The feature `Authrize` has the same function with `this.User.Identity.IsAuthenticated`, which means that if we add the `Authrize` on the action, we can leave out the codes below:  

    if (this.User.Identity.IsAuthenticated) // Authentication
    {
        string adminName = this.User.Identity.Name;
        ViewBag.adminName = adminName;
        return View("StudentManage");
    }
    else
    {
        return RedirectToAction("Index", "SysAdmin");
    }
 
You can use this feature on a controller or an action. For example,  


    [Authorize]
    public ActionResult AdminExit(SysAdmin objSysAdmin)
    {
        FormsAuthentication.SignOut();
        Session["CurrentAdmin"] = null;
        return View("AdminLogin");
    }

***


### `Authrize` Implement the Advanced Authorization

We can specify to authorize an username like below:  

    [Authorize(Users = "John")]
    public ActionResult Index()
    {
        if (this.User.Identity.IsAuthenticated)
        {
            string adminName = this.User.Identity.Name;
            ViewBag.adminName = adminName;
            return View("StudentManage");
        }
        else
        {
            return RedirectToAction("Index", "SysAdmin");
        }
    }

 
 
This snippet of code can achieve the same result with User authorization.  


*** 


## Demonstration  


<p align="center">
<img src="/images/post/20170612001.png" alt="login page" width="40%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

<br/>
<br/>

<p align="center">
<img src="/images/post/20170612002.png" alt="user state" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>
<br/>
<br/>


<p align="center">
<img src="/images/post/20170612003.png" alt="log out" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>
  
  
***
So much more today, hope that this article will give you some help.  





