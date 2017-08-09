---
title: "从控制器到视图的数据传递方法汇总"
date: 2017-06-02
category: .Net
tags: [.Net, MVC, Data Transmission]
comments: true

---

## 背景介绍

在.Net MVC中有多种方法允许数据从控制器传递到视图。这篇文章将分别介绍这几方法，并且做出使用总结。  

1. [ViewData 对象](#1)  
2. [ViewBag 动态对象](#2)  
3. [TempData 跨请求数据传递](#3)  


***

<h2 id="1">ViewData对象</h2>

ViewData 是一种字典集合数据，是“视图基类”和“控制器基类”的属性。也就是说，ViewData 同时继承了视图基类和控制器基类。  
**常见的使用方法：** 控制器中写入数据，在视图中读取数据。类似下面的例子：  

控制器中：  
<p align="center">
<img src="/images/post/20170602001.png" alt="viewdata controller" width="60%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

视图中：  
<p align="center">
<img src="/images/post/20170602002.png" alt="viewdata view" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

ViewData的Value可以存放任意数据类型的数据，因此使用时需要强制转换。  


***

<h2 id="2">动态对象ViewBag</h2>

ViewBag是dynamic类型的对象，同样也是“视图基类”和“控制器基类”的属性。  
**好处：** 使用更灵活方便。  
**特点：** ViewBag其实是对ViewData数据的包装，使用ViewData保存的数据可使用ViewBag读取，反之亦然。  

控制器中：  
<p align="center">
<img src="/images/post/20170602003.png" alt="viewbag controller" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

视图中：  
<p align="center">
<img src="/images/post/20170602004.png" alt="viewbag view" width="50%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

实际开发中最好选择其中的一种使用，建议使用ViewBag。  

***

<h2 id="3">跨请求数据传递TempData</h2>


TempData 是一种字典对象，也能用于从“控制器到视图”的数据传递，和ViewData类似。  
**特性：** TempData 还能实现“不同请求之间”的数据传递。  
现在我们来看看实现，我以一个登录的功能作为例子，按照MVC的规则，是需要在登录的View去提交表单到登录的Controller，但是这里为了验证TempData，我们改一下提交表单到其他控制器的随便一个方法：  

AdminLogin.cshtml 视图中：  
<p align="center">
<img src="/images/post/20170602005.png" alt="tempdata view" width="70%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

在这里是把表单提交到Student控制器中的StudentLogin动作。  

Student 控制器中：  

    public ActionResult StudentLogin()
        {
            // 获取用户输入数据
            SysAdmin objSysAdmin = new SysAdmin()
            {
                LoginId = Convert.ToInt32(Request.Params["loginId"]),
                LoginPwd = Request.Params["loginPwd"]
            };
            // 保存到TempData 中
            TempData["objSysAdmin"] = objSysAdmin;
            // 跳转到其他控制器（重定向到另一个action）
            return RedirectToAction("Login", "SysAdmin");
        }

这个StudentLogin 是新添加的动作方法，注意最后是从StudentController中跳转到SysAdminController中。  

Admin 控制器中：  

    public ActionResult Login()
        {
            //调用业务逻辑
            SysAdmin objSysAdmin = (SysAdmin) TempData["objSysAdmin"];
            //保存数据 
            if (objSysAdmin != null)
            {
                ViewData["info"] = "welcome: " + objSysAdmin.AdminName;
                return RedirectToAction("Index", "Student");
            }
            else
            {
                ViewData["info"] = "user name or password error!";
                return View("AdminLogin");
            }
        }

结果是我们同样可以正常登录：  


<p align="center">
<img src="/images/post/20170602006.png" alt="result" width="50%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

### 注意事项

TempData 数据保存机制是Session，但又不完全同Session。  
**1.** TempData保存数据后，如果被使用，就会被清除，因此后面的请求将不能再次使用。  
**2.** TempData保存数据后，如果没有被使用，则它保存的时间是Session的生存期。  

***  

## 总结

常用的传递方法:  

<table class="table">
    <tr>
        <th>传递方式</th>
        <th>应用场合</th>
    </tr>
    <tr>
        <td>ViewData</td>
        <td>适合传递单个数据，需要类型转换</td>
    </tr>
    <tr>
        <td>ViewBag</td>
        <td>适合传递单个数据，不需要类型转换</td>
    </tr>
    <tr>
        <td>TempData</td>
        <td>主要用来跨多个动作方法传递数据</td>
    </tr>
    <tr>
        <td>View()+Mode</td>
        <td>适合传递模型数据，不需要类型转换</td>
    </tr>
</table>

***

在.NET MVC中控制器到视图的数据传递方法大致就这几种，需要多多实践，掌握起来很容易。  


