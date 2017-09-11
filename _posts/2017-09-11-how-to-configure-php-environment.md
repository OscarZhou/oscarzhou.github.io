---
title: "How to Configure the PHP Environment"
date: 2017-09-11
category: Other
tags: [PHP, Configuration]
comments: true

---

## Introduction

I think it's time to study PHP. Before that, we have to set up the php environment firstly. This article will show how to configure the environment step by step.  

  
*** 

## Configuring PHP Development Environment

First of all, we need to download, install, and configure the web server, the php engine, and the MySQL server. There is an easy way to download all the components, **AMP package** (**A**pache, **M**ySQL, **P**HP).  
[XAMPP](https://www.apachefriends.org/download.html) for Windows.  
[MAMP](https://www.mamp.info/en/) for macOS.  

So far, we have already download the installation package. And before we install it, we need to close the antivirus software.  



<p align="center">
<img src="/images/post/20170911001.png" alt="installation interface" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

If you have already got here, just keep clicking the next button.  
  

<p align="center">
<img src="/images/post/20170911002.png" alt="installation interface" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

Install in process.  
  
<p align="center">
<img src="/images/post/20170911003.png" alt="successful interface" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

Install successfully.  
  
 

***


## Cases

Let's create two files, *phptest.html* and *learnphp.php*.  

**phptest.html**  

    <html>
    <body>
    <form action="learnphp.php" method="post">
    <table border="0">
    
    <tr>
    <td>Name</td>
    <td align="center"><input type="text" name="username" size="30" /></td>
    </tr>
    
    
    <tr>
    <td>Address</td>
    <td align="center"><input type="text" name="streetaddress" size="30" /></td>
    </tr>
    
    
    <tr>
    <td>City</td>
    <td align="center"><input type="text" name="cityaddress" size="30" /></td>
    </tr>
    
    
    <tr>
    <td colspan="2" align="center"><input type="submit" value="Submit" /></td>
    </tr>
    
    
    </table>
    </form>
    </body>
    </html>

We can get a page like below.  

<p align="center">
<img src="/images/post/20170911004.png" alt="html page" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

**learnphp.php**  

    
    <html>
    <head>
        <title>Information Gathered</title>
    </head>
    
    <body>
        <?php
            
        echo "<p>Data processed</>";
        
        ?>
    </body>
    </html>

When we click the submit button, the page will be redirected to another page like below.  


<p align="center">
<img src="/images/post/20170911005.png" alt="php page" width="40%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>


Well done. I finished my first php program.  

***

