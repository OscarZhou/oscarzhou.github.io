---
title: "Delegate Application Example"
date: 2017-05-26
category: C#
tags: [C#, Delegate, Counter]

---


## Introduction

Today I will use the delegate to implement the message delivery between clients. When you run this application, three windows will pop up on the screen. One is server window, which is only used for receiving the message and displaying the chatting records, and the another two are clients windows, which allow to send message to the server. Okay, it's very easy. Let's start it now.    
   
If you want to know other articles about the delegate. You can read:  

**1** [Basic Delegate](http://oscarzhou.co.nz/blog/c%23/2017/05/12/delegate)  
**2** [Advanced Delegate](http://oscarzhou.co.nz/blog/c%23/2017/05/25/advanced-delegate)
   
***

## Logic and Code

I don't want to make this program look complicated, we only do the core part, delivering messages. The interface is like this:  
 
<p align="center">
<img src="/images/post/20170526001.png" alt="Project Window" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

As you see, it works. Despite it looks a little weird, because the chatting software today always shows the chatting records on the client's own interface. We also can implement it like that, but here I want to show you how to use the delegate in a clearly logic.  

First of all, we must clearly know which interface we want to update, because it is where we will add our `Receiver()` function. So in this case, the server window is the place. Okay, we know everything now, so let's code!  

On the client part:

    public delegate void DlgSendMsg(string content);//step 1 

On the server part:
  
    public void Receiver(string content)
    {
        tbServer.AppendText(content);//step 2
        
    }
    
Go back to client part:

    public DlgSendMsg EvtSendMsg; //step 3
    
    FrmClient1 frmClient1 = new FrmClient1();
    frmClient1.EvtSendMsg += this.Receiver;//step 4
    frmClient1.Show();
    
    private void btnSend_Click(object sender, EventArgs e)
    {
        EvtSendMsg("Client1: " + tbClient.Text.Trim() + "\r\n");//step 5
    }

That's it, so easy. You just need to move the client code to all the client parts. Why is it so easy. Like I said before, just remind the five steps when you want to use delegate to solve some problems.  

*** 

**1. Declare the delegate (define the prototype of a method or a function: return value + the type and numbers of parameters)**  
**2. Define the specific method according to the declaration of the delegate**  
**3. Create the object of the delegate**  
**4. Associate the delegate with the specific method**  
**5. Call the method via the delegate rather than directly calling the method**  


