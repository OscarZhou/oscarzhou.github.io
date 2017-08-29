---
title: "The Introduction of The Order Management Tool"
date: 2017-07-15
category: C#
tags: [.NET, C#, Winform]
code: project

---

<br/>


## Introduction

The project aims to manage the orders more effectively, conveniently and automatically.  

Since last September, I started to purchase some milk powder and health products for friends in China in my spare time. Though it looks like not very hard, this job is time-consuming. I have to repeatly calculate the price of the products every time when people ask me the price, because the price on the website of the stores is changing all the time.  

Another annoying thing is that I have to maintain the records that users purchases the products, as next time if they want to place an order, I can immediately show them their address, which increase their customer sticky.  

I know that all what I talked above is very easy to implement it. In fact, there are many e-commercial open sources code on the Internet. But the truth is that the e-commercial website is what I am currenting doing. I just can't stop to bear such a heavy operation before the website releases.  

So that is the initial intention of this project.

## Technology   

It will be written by WinForm in .Net framework. And the database will use SQL Server.  


## V1.0

First releasing.  

**1.** Place the order  
**2.** Browse the transaction  
**3.** Import original .txt and .xlsx file  
**4.** Finish undone order    

## V1.2

**1.** View and delete the order  
**2.** Name search and transaction list sorting  
**3.** Export transaction  
**4.** Solve some bugs  

## V1.3
**1.** Edit the order  
**2.** Add CreateTime into Excel  
**3.** Create new order based on previous user information  
**4.** Edit the item list  
**5.** Solve some bugs  


## V1.5
**1.** Add Price Calculation Kit (Make ordering operation more convenient)  
&nbsp;&nbsp;**1.1** Accelerate the price's calculation  
&nbsp;&nbsp;**1.2** Move product and price's information direct to the order  
&nbsp;&nbsp;**1.3** Add browsing history (latest 20 products)  
**2.** Fix some bugs  
&nbsp;&nbsp;**2.1** Add Reader.Close() in order to avoiding the collapase of the system  
&nbsp;&nbsp;**2.2** Adjust the width of the columns in each DataGridView   
&nbsp;&nbsp;**2.3** Convert the Date column in Excel with the additional column(Date) to DateTime type  
&nbsp;&nbsp;**2.4** Add Key Code to associate with window operation  
 
 
 
 
 








  
  




