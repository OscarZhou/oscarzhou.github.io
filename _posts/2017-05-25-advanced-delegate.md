---
title: "Advanced Delegate in C#"
date: 2017-05-25
category: C#
tags: [C#, Delegate, Counter]
comments: true

---


## Introduction

I introduced the basic delegate in C# last time ([Basic Delegate](http://oscarzhou.co.nz/blog/c%23/2017/05/12/delegate)) but I think it is not enough for the real project. So in this article I will introduce a more practical use about delegate.  
 
In fact, we usually meet a situation that we need to pass some values from a window to another window. In web development, we can pass these values by URL or save it into object Session. However, there is not such kind of things in Winform development. Here is where the delegate does work. 
 
I always advocate that the best way to learn new things is to learn it in a practical way. So I will introduce the advanced use of the delegate by a small program. We call it Counter for temporary.  
   
   
***
 
## About project  

This is a Winform program with two windows, the main window and the sub window respectively. There are two buttons "+1" and "Reset" in the main window and the sub window has a label of the number. When the button "+1" is clicked, the label of the number will increase 1 automatically. When the button "Reset" is clicked, the label of the number will change into 0. So it is the program that we will implement using delegate. Fig.1 shows the main window and the sub window.    

<p align="center">
<img src="/images/post/20170525002.png" alt="Project Window" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

***

## Five Key Steps 

The Fig.2 shows the mechanism of the delegate in this program.  

<p align="center">
<img src="/images/post/20170525001.png" alt="Five Step for Delegate" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


You can see that there are five steps in the Fig.2. If you can master this five steps, I promise that you can apply delegate to deal with any problems that can be solved by the delegate.    

**1** Declare the delegate variable  
In Main Form:  

    public delegate void DlgCount(string msg); //1. Declare the delegate variable  

**2** Define the method according to the delegate (Receive the information that the delegate passes)
In Sub Form:  

    // 2. Define the method according to the delegate
    public void Receiver(string msg)
    {
        lbCounter.Text = msg;
    }

**3** Create the object of the delegate variable (The delegate object can connect the delegate with the specific method)  
In Main Form:  
  
    public DlgCount EvtCount; // 3. Create the object of the delegate variable

**4** Associate the delegate with the specific method  
In Main Form:  

    public FrmMain()
    {
        InitializeComponent();
        FrmSub frmSub = new FrmSub();
        EvtCount += frmSub.Receiver; // 4. Associate the delegate with the specific method
        frmSub.Show();
    }
        
**5** Call the delegate variable to pass the message   
In Main Form:  
        
    private int counter = 0;
    private void btnPlusOne_Click(object sender, EventArgs e)
    {
        counter++;
        EvtCount(counter.ToString()); // 5. Call the delegate object to pass the message
    }

    private void btnReset_Click(object sender, EventArgs e)
    {
        counter = 0;
        EvtCount(counter.ToString()); // 5. Call the delegate object to pass the message
    }


***

It is easy to pass the value between window, isn't it? Like I mentioned above, just keep the these five steps in mind. You can say that you have already the usage of the delegate. I hope that it can help you guys.  

