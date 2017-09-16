---
title: "XAMPP Configuration"
date: 2017-09-11
category: Other
tags: [PHP, XAMPP Configuration, software]
comments: true

---

## Introduction

The article gives a common solution to problem that the XAMPP probably met.  

*** 

## Problem: Why can't startup the Apache Module?


<p align="center">
<img src="/images/post/20170911007.png" alt="Can't startup apache" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

**The reason of the problem**  
As the image shows above, <mark>This may be due to a blocked port, missing dependencies</mark>. In fact, when we install the XAMPP, the default port is 80, which to a great extent has a conflict with others. So my solution is to change the Port.  

***

## Solution: Why can't startup the Apache Module?

In this case, we need to change some variables in two files manually, instead of changing the setting in *Config* button in the top right conner.    


<p align="center">
<img src="/images/post/20170911008.png" alt="Can't startup apache" width="40%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

When we click the *Config* button in the Apache row, some text files show up. What we need to change is in the first file, called **httpd.conf**. Open this file, there are two lines which are <mark>Listen 80</mark> and <mark>ServerName localhost:80</mark>. We need to modify the 80. I replace it with 81 and save it.  

When we try to start Apache again, we will see  


<p align="center">
<img src="/images/post/20170911009.png" alt="Can't startup apache" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

It works. 
***

## How to test if the PHP environment works

There is one vital rule to make it happen. We must put our php files into a directory called **htdocs**.  
 

<p align="center">
<img src="/images/post/20170911010.png" alt="Can't startup apache" width="40%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>
 
When you put your php code in this directory, the url should be **localhost:81/**. For example, I have a php file in the *C:\xampp\htdocs\Test*. The code is very simple like below:  

    <?php
        echo "Hello PHP World!!"
    ?>

So the result should be  

<p align="center">
<img src="/images/post/20170911011.png" alt="Can't startup apache" width="50%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

***

Good job!!