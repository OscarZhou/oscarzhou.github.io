---
title: "How to Implement Globalization and Localization in WinForm"
date: 2017-12-23
category: C#
tags: [Globalization, Localization, C#, Winform]
code: omt
comments: true

---

<br/>


## Introduction

The displaying language on current UI is English. However, recently I need to change it into Chinese so that our customer service in China can use this tool conveniently. In C# Winform, the program and UI of corresponding language version can be implemented through loading the resource files of different language. That is, the globalization can be implemented by resource files. Let's finish the function step by step.    

***
  
## 1. Add Chinese and English resource files

In WinForm, when a form is created, there will be a default .resx file to be generated automatically. Find out the default .resx file under specific directory and add .zh-CHS.resx file and .en.resx file based on this file name. For example:  



  
<br/>
<p align="center">
<img src="/images/postproject/omt20171223001.png" alt="file format" width="50%"  /><br/>
<center><h3><b>Figure 1</b></h3></center>
</p>
<br/>

If the default form name is `OrderManagementTool.resx`, you need to name another two file: `OrderManagementTool.zh-CHS.resx` and `OrderManagementTool.en-US.resx` respectively.  

***

## 2. Edit the resource files

Choose the form that you want to add the another language and open the properties of the UI. Change the attribute Localization to true. For example:  



  
<br/>
<p align="center">
<img src="/images/postproject/omt20171223002.png" alt="file format" width="50%"  /><br/>
<center><h3><b>Figure 2</b></h3></center>
</p>
<br/>

Then go back to the default .resx file and double click it. You will see the interface like below:  

  
<br/>
<p align="center">
<img src="/images/postproject/omt20171223003.png" alt="file format" width="70%"  /><br/>
<center><h3><b>Figure 3</b></h3></center>
</p>
<br/>

Note that the option on the left top conner, you have to change it into `Strings`, then you can see this interface. When we get the correct page, we need to find out the corresponding name of the control that we want to make it display different languages. Then we set it as the `Name` in separated .zh-CHS.resx file and .en-US.resx file and fill the corresponding language in the `Value` as the figures shown below.  
 
<br/>
<p align="center">
<img src="/images/postproject/omt20171223004.png" alt="file format" width="70%"  /><br/>
<center><h3><b>Figure 4</b></h3></center>
</p>
<br/>

<br/>
<p align="center">
<img src="/images/postproject/omt20171223005.png" alt="file format" width="70%"  /><br/>
<center><h3><b>Figure 5</b></h3></center>
</p>
<br/>

***

## 3. Load the resources files with correct language  

Open the Program.cs which is the entry point of the program and add `CultureInfo` 
 
        /// <summary>
        /// The main entry point for the application.
        /// </summary>
        [STAThread]
        static void Main()
        {
            Application.EnableVisualStyles();
            Application.SetCompatibleTextRenderingDefault(false);
            Thread.CurrentThread.CurrentUICulture = new System.Globalization.CultureInfo("zh-CHS");  //  This line is lately added
            Application.Run(new OrderManagementTool());
        }

After this, we finish it.  


*** 

## Result 

<br/>
<p align="center">
<img src="/images/postproject/omt20171223006.png" alt="file format" width="70%"  /><br/>
<center><h3><b>Figure 6</b></h3></center>
</p>
<br/>


Hope it is helpful.  
