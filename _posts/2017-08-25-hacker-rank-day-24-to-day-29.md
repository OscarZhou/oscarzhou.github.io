---
title: "Hacker Rank: Day 24 to Day 29"
date: 2017-08-25
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Introduction

We finally finish the 30 days' challenges. Cheer up!  
  
*** 


## Code Challenge

### Day 24: More Linked List

**Task**   

A Node class is provided for you in the editor. A Node object has an integer data field, *data*, and a Node instance pointer, *next*, pointing to another node (i.e.: the next node in a list).  

A removeDuplicates function is declared in your editor, which takes a pointer to the *head* node of a linked list as a parameter. Complete removeDuplicates so that it deletes any duplicate nodes from the list and returns the head of the updated list.  

  
**Input Format**  

You do not need to read any input from stdin. The following input is handled by the locked stub code and passed to the removeDuplicates function:   
The first line contains an integer, *N*, the number of nodes to be inserted.   
The *N* subsequent lines each contain an integer describing the *data* value of a node being inserted at the list's tail.    
  
**Output Format**  

Your *removeDuplicates* function should return the head of the updated linked list. The locked stub code in your editor will print the returned list to stdout.  


**Sample Input**  
<mark>
6<br/>
1<br/>
2<br/>
2<br/>
3<br/>
3<br/>
4<br/>
</mark>

**Sample Output**  

<mark>
1 2 3 4<br/>
</mark>


**Code**  
      
          
    using System;
    using System.Collections.Generic;
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
       public static Node removeDuplicates(Node head){
         //Write your code here
           if(head != null)
           {
               Node last = head;
               while(true)
               {
                   if(last.next == null)
                   {
                       break;
                   }
                   if(last.data == last.next.data)
                   {
                       last.next = last.next.next;
                   }
                   else
                   {
                       last = last.next;
                   }
               }
               
           }
           return head;
       }
        public static  Node insert(Node head,int data)
        {
            Node p=new Node(data);
            
            
            if(head==null)
                head=p;
            else if(head.next==null)
                head.next=p;
            else
            {
                Node start=head;
                while(start.next!=null)
                    start=start.next;
                start.next=p;
                
            }
            return head;
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
            head=removeDuplicates(head);
            display(head);
        }
    }
    

This problem is about modifying the original linked list to remove the duplicates in consecutive two node.  
  

***
    
### Day 25: Running Time and Complexity


**Task**  

A prime is a natural number greater than *1* that has no positive divisors other than *1* and itself. Given a number, *n*, determine and print whether it's *Prime* or *Not prime*.  

  
**Input Format**  

The first line contains an integer, *T*, the number of test cases.   
Each of the *T* subsequent lines contains an integer, *n*, to be tested for primality.  
  
  
**Output Format**  

For each test case, print whether *n* is *Prime* or *Not prime* on a new line.  



**Sample Input**  
<mark>
3<br/>
12<br/>
5<br/>
7<br/>
</mark>

**Sample Output**  

<mark>
Not prime<br/>
Prime<br/>
Prime<br/>
</mark>



**Code**  
        
    using System;
    using System.Collections.Generic;
    using System.IO;
    class Solution {
        static void Main(String[] args) {
            /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution */
            int n = Convert.ToInt32(Console.ReadLine());
            for(int i=0; i<n; i++)
            {
                int value = Convert.ToInt32(Console.ReadLine());
                if(value == 1)
                {
                    Console.WriteLine("Not prime");
                    continue;
                }
                else if(value == 2)
                {
                    Console.WriteLine("Prime");
                    continue;
                }
                int sqt = (int)Math.Sqrt(value);
                for(int j = 2; j<sqt+1; j++)
                {
                    if(value%j==0)
                    {
                        Console.WriteLine("Not prime");
                        break;
                    }
                    if(j == sqt)
                    {
                        Console.WriteLine("Prime");       
                    }
                }
            }
        }
    }

In this problem, what we have to learn is the method about how to determine if a number is a prime in a faster way.  


*** 

   
### Day 28: RegEx, Patterns, and Intro to Databases

**Task**  

Consider a database table, Emails, which has the attributes First Name and Email ID. Given *N* rows of data simulating the Emails table, print an alphabetically-ordered list of people whose email address ends in *@gmail.com*.  


**Input Format**  

The first line contains an integer, *N*, total number of rows in the table.  
Each of the *N* subsequent lines contains *2* space-separated strings denoting a person's first name and email ID, respectively.  
  
  
**Output Format**  

Print an alphabetically-ordered list of first names for every user with a gmail account. Each name must be printed on a new line.  
  
**Sample Input**  
<mark>
riya riya@gmail.com<br/>
julia julia@julia.me<br/>
julia sjulia@gmail.com<br/>
julia julia@gmail.com<br/>
samantha samantha@gmail.com<br/>
tanya tanya@gmail.com<br/>
</mark>

**Sample Output**  

<mark>
julia<br/>
julia<br/>
riya<br/>
samantha<br/>
tanya<br/>
</mark>


**Code**  

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {
    
        static void Main(String[] args) {
            int N = Convert.ToInt32(Console.ReadLine());
            List<string> list = new List<string>();
            for(int a0 = 0; a0 < N; a0++){
                string[] tokens_firstName = Console.ReadLine().Split(' ');
                string firstName = tokens_firstName[0];
                string emailID = tokens_firstName[1];
                if(emailID.EndsWith("@gmail.com"))
                {
                    list.Add(firstName);
                }
            }
            list.Sort();
            foreach(string v in list)
            {
                Console.WriteLine(v);
            }
        }
    }

Note that `EndsWith()` and `Sort` method.  

 

***



### Day 29: Bitwise AND 

**Task**  

Given set *S={1, 2, 3, ..., N}*. Find two integers, *A* and *B* (where *A < B*), from set *S* such that the value of *A & B* is the maximum possible and also less than a given integer, *K*. In this case, *&* represents the bitwise AND operator.  

**Input Format**  

The first line contains an integer, *T*, the number of test cases.   
Each of the *T* subsequent lines defines a test case as *2* space-separated integers, *N* and *K*, respectively.  

   
**Output Format**  

For each test case, print the maximum possible value of *A & B* on a new line.  

  
**Sample Input**  
<mark>
3<br/>
5 2<br/>
8 5<br/>
2 2<br/>
</mark>

**Sample Output**  

<mark>
1<br/>
4<br/>
0<br/>
</mark>


**Code**  

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {
        static void Main(String[] args) {
            int t = Convert.ToInt32(Console.ReadLine());
            for(int a0 = 0; a0 < t; a0++){
                string[] tokens_n = Console.ReadLine().Split(' ');
                int n = Convert.ToInt32(tokens_n[0]);
                int k = Convert.ToInt32(tokens_n[1]);
                
                int max = 0;
                for(int i=0; i<n-1; i++)
                {
                    for(int j=i+1; j<n; j++)
                    {
                        int crt = i & j;
                        if(crt > max && crt < k)
                        {
                            max = crt;
                        }
                    }
                }
                Console.WriteLine(max);
            }
            
        }
    }



***



