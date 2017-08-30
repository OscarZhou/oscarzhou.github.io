---
title: "How to Implement Importing Data"
date: 2017-07-16
category: C#
tags: [.NET, C#, Winform]
code: omt
comments: true

---

<br/>


## Introduction


My first thing in this project is to import existed orders' records. In the past, I used two ways to manage my business. One of them is to use a bunch of .txt files to record customers' order detail, which includes customers' contact information and what kind of things do they buy. The another way is to manage the transaction record by one .xls file. I'll show them below:  

  
<br/>
<p align="center">
<img src="/images/postproject/omt20170716001.png" alt="file format" width="30%"  /><br/>
<center><h3><b>Figure 1</b></h3></center>
</p>
<br/>


  
<br/>
<p align="center">
<img src="/images/postproject/omt20170716002.png" alt="txt file" width="70%"  /><br/>
<center><h3><b>Figure 2</b></h3></center>
</p>
<br/>

***

## Difficulties in Code Implementation

When I tried to implement this function, I met several problems below:  
1. [How to make progress bar update on interface and data update in database simultaneously](#1)
2. [How to parse the file name and order contents of text file](#2)
3. [How to store the previous directory](#3)
4. [How to parse the excel file](#4)

I will introduce the solutions to the problems in next several articles. You can track them by the links above.

***


<h3 id='1'><b>How to make progress bar update on interface and data update in database simultaneously </b></h3>

First of all, I placed a progress bar on the interface, but in winform, you can't update the progress bar with the change of the numbers of importing data. So if you want the progress bar to indicate the progress in real time, you can use another control called BackgroundWorker and do data operation in it so that you can  

<br/>
<p align="center">
<img src="/images/postproject/omt20170716003.png" alt="txt file" width="70%"  /><br/>
<center><h3><b>Figure 3</b></h3></center>
</p>
<br/>

You can see that the control marked by the red square is BackgroundWorker. 

    private readonly List<Order> objOrders = new List<Order>();
    
    private void btnImport_Click(object sender, EventArgs e)
        {
            var fileSelector = new FolderBrowserDialog();
            if (fileSelector.ShowDialog() == DialogResult.OK)
            {
                lbFolder.Text = string.Format("     Selected Directory: {0}", fileSelector.SelectedPath);
                var files = Directory.GetFiles(fileSelector.SelectedPath).Where(name => name.EndsWith(".txt")); // Filter the suffix of the file
                prbImport.Maximum = files.ToList().Count;  // Set the maximum value of the progress bar
                prbImport.Step = 1; 
                prbImport.Value = 0;

                foreach (var file in files.ToList())
                {
                    var objOrder = FormatParsing.ParsePathToFileName(file); // Parse the file name. This part will be shown in next article 
                    objOrder = FormatParsing.ParseContentIntoOrder(file, objOrder); // Parse the file content. This part will be shown in next article
                    objOrders.Add(objOrder);
                }
                objOrders.Sort(); //  Sort the orders according to the order No

                // Start another thread to update the progress bar in the background
                // This is the key part to implement this function
                if (bkgWorkForImporting.IsBusy != true)
                    bkgWorkForImporting.RunWorkerAsync();
                btnImportRecords.Enabled = false;
            }
        }

To be honest, this is not typically running in real-time, but it will give you a good visualization. You should note two points. One is the `objOrders.Sort()`. For the sorting of the objects, we have to override the `CompareTo()` by ourselves. If you want to sort according to order No. You can write like this:  

    public int CompareTo(Order other)
    {
        return this.OrderNo.CompareTo(other.OrderNo);
    }

This snippet of code solve the sorting problem. Another point is that `bkgWorkForImporting.RunWorkerAsync();`. It is this piece of code that make the progress bar updated possible. It is kind of like creating another thread that is used for processing the interface business. So you should create the event of BackgroundWorker like this:  


<br/>
<p align="center">
<img src="/images/postproject/omt20170716004.png" alt="txt file" width="40%"  /><br/>
<center><h3><b>Figure 4</b></h3></center>
</p>
<br/>

And then you can implement the code like below:  

    private void bkgWorkForImporting_DoWork(object sender, DoWorkEventArgs e)
        {
            var bkgWorker = sender as BackgroundWorker; // It is the necessary part

            var counter = 0;
            foreach (var objOrder in objOrders)
            {
                var objUserInfo = objOrder.User;
                new UserInfoManage().InsertUser(objUserInfo); //fill the userinfo table
                objOrder.User.UserNo = new UserInfoManage().GetLatestUserNo(); // Get the UserNo inserted lately
                new OrderManage().InsertOrder(objOrder); // Fill the order table
                var lstItems = objOrder.LstItems;
                foreach (var objItem in lstItems)
                    new ItemManage().InsertItem(objItem); // Fill the itemlist table

                // Pass the progress to the progress bar method to update the bar
                try
                {
                    bkgWorker.ReportProgress(counter++);
                }
                catch (NullReferenceException exception)
                {
                    Console.WriteLine(exception);
                    throw;
                }
            }
        }


In this part, you will note that 


***




***




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
 
 
 
 
 








  
  




