---
title: "Data Import(Make Progress Bar Changed)"
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

<br/>
<p align="center">
<img src="/images/postproject/omt20170716003.png" alt="data import interface" width="70%"  /><br/>
<center><h3><b>Figure 3</b></h3></center>
</p>
<br/>

First of all, I placed a progress bar on the interface, but in winform, you can't update the progress bar with the change of the numbers of importing data. So if you want the progress bar to indicate the progress in real time, you can use another control called BackgroundWorker and do data operation in it so that you can  

<br/>
<p align="center">
<img src="/images/postproject/omt20170716004.png" alt="BackgroundWorker" width="70%"  /><br/>
<center><h3><b>Figure 4</b></h3></center>
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
<img src="/images/postproject/omt20170716005.png" alt="dowork" width="60%"  /><br/>
<center><h3><b>Figure 5</b></h3></center>
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


In this part, you will note that `bkgWorker.ReportProgress(counter++);` is called once time in every loop, which means that the progress will be changed in every loop.  



<br/>
<p align="center">
<img src="/images/postproject/omt20170716006.png" alt="progress report" width="60%"  /><br/>
<center><h3><b>Figure 6</b></h3></center>
</p>
<br/>

    private void bkgWorkForImporting_ProgressChanged(object sender, ProgressChangedEventArgs e)
        {
            prbImport.PerformStep(); // Change the state of the progress bar
            var objOrder = objOrders[e.ProgressPercentage];
            lbProcessing.Text = string.Format("     Current Processing: Loading {0}{1}.txt",
                objOrder.OrderNo, objOrder.Purchaser);
            var counter = e.ProgressPercentage;
            var total = objOrders.Count();
            var progressIndicate = string.Format("{0:P1}", counter * 1.0 / total);
            lbProgress.Text = progressIndicate;
            if (counter + 1 == total)
                lbProgress.Text = "100%";
        }

`prbImport.PerformStep();` is the key to update the progress bar. So just remember these two functions when you want to implement the progress bar changed.  
1. `bkgWorkForImporting_DoWork`  
2. `bkgWorkForImporting_ProgressChanged`  

***

There are still other problems that are solved by other articles. 

