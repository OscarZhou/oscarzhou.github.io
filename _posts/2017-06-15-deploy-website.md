---
title: "Deploy Website on IIS"
date: 2017-06-15
category: Other
tags: [MVC, .NET, IIS, Deployment]
comments: true

---

## Introduction

After finishing the website, we need to deploy it on server so that it can be accessed on Internet.  

## Website Configuration Files

### About ASP.NET configuration files
**1.** web.config is usually applied in the configuration files in application level. The modification won't influence other sites but it can be used for the sub-directories.  
**2.** it is based on XML, the node of configuration are case sensitive.  
**3.** it is readable and writable, which is more convenient than binary files.  
**4.** after finishing the modification, ASP.NET will detect the changes automatically and is not necessary to restart server or IIS.  

### The hierarchical structure of configuration files

<p align="center">
<img src="/images/post/20170615001.png" alt="architecture of configuration" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


## The Configuration of Web.config File

**1.** `sessionState`

    <sessionState mode="InProc" cookieless="false" timeout="25">
    </sessionState>
    

<table class="table">
    <tr>
        <th>Attributes</th>
        <th>Option</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>mode</td>
        <td></td>
        <td>Specifies where to store session state values.</td>
    </tr>
    <tr>
        <td>&nsbp;</td>
        <td>Off</td>
        <td>Session state is disabled.</td>
    </tr>
    <tr>
        <td>&nsbp;</td>
        <td>InProc</td>
        <td>Session state is in process with an ASP.NET worker process.</td>
    </tr>
    <tr>
        <td>&nsbp;</td>
        <td>StateServer</td>
        <td>Session state is using the out-of-process ASP.NET state service to store state information.</td>
    </tr>
    <tr>
        <td>&nsbp;</td>
        <td>SQLServer</td>
        <td>Session state is using an out-of-process SQL Server database to store state information.</td>
    </tr>
</table>

If you want to know more parameters, you can visit [here](https://msdn.microsoft.com/en-us/library/h6bb9cz9(v=vs.85).aspx).  
  
**2.** `compilation`

    <compilation debug="false" targetFramework="4.5" />
    
*true* enable the debug, *false* disable the debug. When we release this project, we can disable it.  
  

**3.** `httpRuntime`

    <httpRuntime enable="true" executionTimeout="90" maxRequestLength="4096" />
    
`enable="true"`: If false, the application is effectively turned off.   
`executionTimeout`: Specifies the maximum number of seconds that a request is allowed to execute.  
`maxRequestLength`: Specifies the limit for the input stream buffering threshold, in KB.  

If you want to know more parameters, you can visit [here](https://msdn.microsoft.com/en-us/library/e1f13641(v=vs.100).aspx).  


**4.** `globalization`

    <globalization requestEncoding="utf-8" responseEncoding="utf-8" />

This option is used for configuring the globalization settings of an application.  
    
Read more information, please visit [here](https://msdn.microsoft.com/en-us/library/hy4kkhe0(v=vs.71).aspx).  


***

## Publishing the ASP.NET MVC Project

ASP.NET MVC release includes the following files:  

**1.** Program collection files and views.  
**2.** configuration files, like web.config.  
**3.** resource files: images, css, javascript  

### The steps of publishing project

**1.Profile**  

Right click the project and click the Publish

<p align="center">
<img src="/images/post/20170615002.png" alt="publish profile" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

After opening the window of publishing web, select the "New Profile". After inputting the name, click next.    

**2.Connection**

Choose the "File System" as the publish method and set the target location. Then click next.  

<p align="center">
<img src="/images/post/20170615003.png" alt="connection" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>


**3.Settings**

Choose the release. Then next.  

<p align="center">
<img src="/images/post/20170615004.png" alt="setting" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>


**4.Preview**

<p align="center">
<img src="/images/post/20170615005.png" alt="preview" width="70%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

Click Publish.

***

## Deploy Website 

### Install IIS

First of all, we need to install the IIS (Internet Information Service).  

[Control Panel] -> [Program and Function] -> [Open or Close Windows Function]  

<p align="center">
<img src="/images/post/20170615006.png" alt="install program" width="50%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

First installation needs some time, so be patient to wait for a minute.  

### Install ASP.NET 4.0

The ASP.NET module does not directly establish any connection with IIS in the default situation, thus, the connection should be creataed.  
Input the command `%WINDIR%\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe -iru -enable` in CMD. 

<p align="center">
<img src="/images/post/20170615007.png" alt="register asp.net 4.0 in cmd" width="40%"  /><br/>
<center><h5><b>Figure 7</b></h5></center>
</p>

Or you can visit the directory below:  

<p align="center">
<img src="/images/post/20170615008.png" alt="register asp.net 4.0" width="70%"  /><br/>
<center><h5><b>Figure 8</b></h5></center>
</p>

### Configure the IIS

**1.Add Application Pool**    

<p align="center">
<img src="/images/post/20170615009.png" alt="Add application pool" width="70%"  /><br/>
<center><h5><b>Figure 9</b></h5></center>
</p>

We need to choose v4.0 version.  

**2.Add Website**   

<p align="center">
<img src="/images/post/20170615010.png" alt="Add website" width="70%"  /><br/>
<center><h5><b>Figure 10</b></h5></center>
</p>

You can input any website names as you want and then we select the application pool we just created. The physical path is to choose the folder that we publish in the Visual Studio. If we have our own domain, we can input its IP address in it. And then we can input our domain name to the host name.  
 

**3.Add Group/UserName**  

Right click the website we just created and then choose the "Edit Permission".  

<p align="center">
<img src="/images/post/20170615011.png" alt="Add Group/UserName1" width="70%"  /><br/>
<center><h5><b>Figure 11</b></h5></center>
</p>

Select the "Users" and click the Edit.  

<p align="center">
<img src="/images/post/20170615012.png" alt="Add Group/UserName1" width="70%"  /><br/>
<center><h5><b>Figure 12</b></h5></center>
</p>

Select the "Users" and click the Add.  

<p align="center">
<img src="/images/post/20170615013.png" alt="Add Group/UserName2" width="70%"  /><br/>
<center><h5><b>Figure 13</b></h5></center>
</p>

Click the "Advanced".  

<p align="center">
<img src="/images/post/20170615014.png" alt="Add Group/UserName3" width="70%"  /><br/>
<center><h5><b>Figure 14</b></h5></center>
</p>

In the opening window, click the look up. we need to add two roles: "NETWORK SERVICE" and "IIS_IUSRS".  

<p align="center">
<img src="/images/post/20170615015.png" alt="Add Group/UserName4" width="70%"  /><br/>
<center><h5><b>Figure 15</b></h5></center>
</p>


**4.Run the website**   

<p align="center">
<img src="/images/post/20170615017.png" alt="browse" width="70%"  /><br/>
<center><h5><b>Figure 16</b></h5></center>
</p>

Click the browsing, then we will see the website.

<p align="center">
<img src="/images/post/20170615016.png" alt="startup" width="70%"  /><br/>
<center><h5><b>Figure 17</b></h5></center>
</p>

See, here it is. It is successful. We are so lucky.

***
So much for today, hope that this article will give you some help.  





