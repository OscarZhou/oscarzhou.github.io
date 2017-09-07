---
title: "Hacker Rank: Day 15 to Day 17"
date: 2017-08-13
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Introduction

Let's the rest of the questions. They are very interesting.  


*** 


## Code Challenge


### Day 15: Linked List



**Task**   
Complete the insert function in your editor so that it creates a new Node (pass *data* as the Node constructor argument) and inserts it at the tail of the linked list referenced by the *head* parameter. Once the new node is added, return the reference to the *head* node.


<p align="center">
<img src="/images/post/20170813001.png" alt="Linked List" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


**Input Format**  
The insert function has *2* parameters: a pointer to a Node named *head*, and an integer value, *data*. 
The constructor for Node has *1* parameter: an integer value for the *data* field.

You do not need to read anything from stdin.

**Output Format**  

Your insert function should return a reference to the *head* node of the linked list.


**Sample Input**  

<mark>
4<br/>
2<br/>
3<br/>
4<br/>
1<br/>
</mark>

**Sample Output**  

<mark>
2 3 4 1 
</mark>

**Code**  
    
    using System;
    class Node
    {
        public int data;
        public Node next;
        public Node(int d){
            data=d;
            next=null;
        }
            
    }
    class Solution {
        public static  Node insert(Node head,int data)
        {
          //Complete this method
            Node new_data = new Node(data);
            
            if(head == null)
            {
                head = new_data;
                return head;
            }
            Node last = head;
            while(true)
            {
                if(last.next == null)
                {
                    last.next = new_data;
                    return head;
                }
                else
                {
                    last = last.next;
                }
            }
        }
        public static void display(Node head)
        {
            Node start=head;
            while(start!=null)
            {
                Console.Write(start.data+" ");
                start=start.next;
            }
        }
        static void Main(String[] args) {
        
            Node head=null;
            int T=Int32.Parse(Console.ReadLine());
            while(T-->0){
                int data=Int32.Parse(Console.ReadLine());
                head=insert(head,data);
            }
            display(head);
        }
    }
        
        

Note that `head = new_data;`, because at the very beginning, I misunderstood the type of the `head` all the time. In fact, `head` is a `Node` which has `data` and `next`. Thus, we have to assign the first node to `head`.  


***
    
### Day 16: Exceptions - String to Integer


**Task**  
Read a string, *S*, and print its integer value; if *S* cannot be converted to an integer, print Bad String.  
  

**Input Format**  

A single string, *S*.  
  
  

**Output Format**  

Print the parsed integer value of *S*, or Bad String if *S* cannot be converted to an integer.  


**Sample Input1**  
<mark>
3
</mark>

**Sample Output1**  

<mark>
3
</mark>



**Sample Input2**  
<mark>
za
</mark>

**Sample Output2**  

<mark>
Bad String
</mark>

**Code**  

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {

        static void Main(String[] args) {
                 string S = Console.ReadLine();
                 try
                 {
                     Console.WriteLine(Convert.ToInt32(S));
                 }
                 catch(Exception e)
                 {
                     Console.WriteLine("Bad String");
                 }
             }
         }

This test is mainly used for instructing how to use the `try{}catch(){}` in C#, which is not very difficult.  
  
  


*** 

   
### Day 17: More Exceptions


**Task**  
Write a Calculator class with a single method: int power(int,int). The power method takes two integers, *n* and *p*, as parameters and returns the integer result *n*<sup>p</sup> of . If either *n* or *p* is negative, then the method must throw an exception with the message: n and p should be non-negative.  


**Input Format**  

Input from stdin is handled for you by the locked stub code in your editor. The first line contains an integer, *T*, the number of test cases. Each of the *T* subsequent lines describes a test case in *2* space-separated integers denoting and *p*, respectively.  

  

**Output Format**  

Output to stdout is handled for you by the locked stub code in your editor. There are *T* lines of output, where each line contains the result of *n*<sup>p</sup> as calculated by your Calculator class' power method.


**Sample Input**  
<mark>
4<br/>
3 5<br/>
2 4<br/>
-1 -2<br/>
-1 3<br/>
</mark>

**Sample Output**  

<mark>
243<br/>
16<br/>
n and p should be non-negative<br/>
n and p should be non-negative<br/>
</mark>


**Code**  

    using System;
    //Write your code here\
    class Calculator
    {
        public int power(int n, int p)
        {
            if(n<0 || p<0)
            {
                throw new Exception("n and p should be non-negative");
            }
            else
            {
                if(p > 1)
                {
                    return (n * power(n, p-1));
                }
                else if(p == 0)
                {
                    return 1;
                }
                else
                {
                    return n;
                }
            }
            
        }
        
    }
    class Solution{
        static void Main(String[] args){
            Calculator myCalculator=new  Calculator();
            int T=Int32.Parse(Console.ReadLine());
            while(T-->0){
                string[] num = Console.ReadLine().Split();
                int n = int.Parse(num[0]);
                int p = int.Parse(num[1]); 
                try{
                    int ans=myCalculator.power(n,p);
                    Console.WriteLine(ans);
                }
                catch(Exception e){
                   Console.WriteLine(e.Message);
    
                }
            }
            
            
            
        }
    }
    
Note that `throw new Exception("n and p should be non-negative");`. In this case, what we learnt is about how to throw an exception in C#. Another point we should note is that when we create the class, we can not add the `public` modifier.  

 

***




