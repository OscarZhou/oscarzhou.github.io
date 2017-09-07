---
title: "Hacker Rank: Day 21 to Day 23"
date: 2017-08-22
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Introduction

We nearly finish the 30 days' challenges. Cheer up!  
  
*** 


## Code Challenge

### Day 21: Generics

**Task**   

Write a single generic function named *printArray*; this function must take an array of generic elements as a parameter (the exception to this is C++, which takes a *vector*). The locked *Solution* class in your editor tests your function.  
  
**Input Format**  

The locked *Solution* class in your editor will pass different types of arrays to your *printArray* function.  
  
**Output Format**  

Your *printArray* function should print each element of its generic array parameter on a new line.  
**Code**  
      
    using System;
    class Printer
    {
        /**
        *    Name: PrintArray
        *    Print each element of the generic array on a new line. Do not return anything.
        *    @param A generic array
        **/
        // Write your code here
        static void PrintArray<T>(T[] arr)
        {
            foreach(var value in arr)
            {
                Console.WriteLine(value);
            }
        }
        static void Main(string[] args)
        {
            int n = Convert.ToInt32(Console.ReadLine());
            int[] intArray = new int[n];
            for (int i = 0; i < n; i++)
            {
                intArray[i] = Convert.ToInt32(Console.ReadLine());
            }
            
            n = Convert.ToInt32(Console.ReadLine());
            string[] stringArray = new string[n];
            for (int i = 0; i < n; i++)
            {
                stringArray[i] = Console.ReadLine();
            }
            
            PrintArray<Int32>(intArray);
            PrintArray<String>(stringArray);
        }
    }

To be honest, I always think the generic is one of most awesome features in C#, which makes the things easier.  In this problem, we should note that the declaration format of the methods that include the generics. In this case, please don't forget add `static`, otherwise, you will receive an error. Remember that `static void PrintArray<T>(T[] arr)` is how you declare a method.  


***
    
### Day 22: Binary Search Trees


**Task**  
The height of a binary search tree is the number of edges between the tree's root and its furthest leaf. You are given a pointer, *root*, pointing to the root of a binary search tree. Complete the *getHeight* function provided in your editor so that it returns the height of the binary search tree.  
  
**Input Format**  

The locked stub code in your editor reads the following inputs and assembles them into a binary search tree:   
The first line contains an integer, *n*, denoting the number of nodes in the tree.   
Each of the *n* subsequent lines contains an integer, *data*, denoting the value of an element that must be added to the BST.  
  
**Output Format**  

The locked stub code in your editor will print the integer returned by your *getHeight* function denoting the height of the BST.  


**Sample Input**  
<mark>
7<br/>
3<br/>
5<br/>
2<br/>
1<br/>
4<br/>
6<br/>
7<br/>
</mark>

**Sample Output**  

<mark>
3<br/>
</mark>

**Explanation**

The input forms the following BST:  


<p align="center">
<img src="/images/post/20170822001.png" alt="BST" width="50%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

The longest root-to-leaf path is shown below:  


<p align="center">
<img src="/images/post/20170822002.png" alt="BST longest path" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


**Code**  
    
    using System;
    class Node{
        public Node left,right;
        public int data;
        public Node(int data){
            this.data=data;
            left=right=null;
        }
    }
    class Solution{
        static int getHeight(Node root){
          //Write your code here
            if(root == null)
            {
                return -1;
            }
            
            int left_length += getHeight(root.left);
            int right_length += getHeight(root.right);
            return (left_length>right_length?left_length:right_length)+1;   
        }
        static Node insert(Node root, int data){
              if(root==null){
                  return new Node(data);
              }
              else{
                  Node cur;
                  if(data<=root.data){
                      cur=insert(root.left,data);
                      root.left=cur;
                  }
                  else{
                      cur=insert(root.right,data);
                      root.right=cur;
                  }
                  return root;
              }
          }
          static void Main(String[] args){
              Node root=null;
              int T=Int32.Parse(Console.ReadLine());
              while(T-->0){
                  int data=Int32.Parse(Console.ReadLine());
                  root=insert(root,data);            
              }
              int height=getHeight(root);
              Console.WriteLine(height);
              
          }
      }
      
I was struggled with this problem. In fact, I struggled with most recursion problem. However, for this question, I highly recommend you to follow this idea by tracking the tree picture, which help you understand it.  


*** 

   
### Day 23: BST Level-Order Traversal

**Task**  

A level-order traversal, also known as a breadth-first search, visits each level of a tree's nodes from left to right, top to bottom. You are given a pointer, *root*, pointing to the root of a binary search tree. Complete the *levelOrder* function provided in your editor so that it prints the level-order traversal of the binary search tree.  

**Input Format**  

The locked stub code in your editor reads the following inputs and assembles them into a BST:   
The first line contains an integer, *T*(the number of test cases).  
The *T* subsequent lines each contain an integer, *data*, denoting the value of an element that must be added to the BST.  
  
**Output Format**  

Print the *data* value of each node in the tree's level-order traversal as a single line of *N* space-separated integers.  


**Sample Input**  
<mark>
6<br/>
3<br/>
5<br/>
4<br/>
7<br/>
2<br/>
1<br/>
</mark>

**Sample Output**  

<mark>
3 2 5 1 4 7<br/>
</mark>

**Explanation**

The input forms the following binary search tree:  
 

<p align="center">
<img src="/images/post/20170822003.png" alt="Binary search tree" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

We traverse each level of the tree from the root downward, and we process the nodes at each level from left to right. The resulting level-order traversal is *3->2->5->1->4->7*, and we print these data values as a single line of space-separated integers.  


**Code**  

    using System;
    using System.Collections;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Node{
        public Node left,right;
        public int data;
        public Node(int data){
            this.data=data;
            left=right=null;
        }
    }
    class Solution{
        static void levelOrder(Node root){
            //Write your code here
            Queue<Node> q = new Queue<Node>();
            if(root != null)
            {
                q.Enqueue(root);
                while(q.Count != 0)
                {
                    Node node = q.Dequeue();
                    Console.Write(node.data + " ");
                    if(node.left != null)
                    {
                        q.Enqueue(node.left);
                    }
                    if(node.right != null)
                    {
                        q.Enqueue(node.right);
                    }
                }
            }
        }
        static Node insert(Node root, int data){
            if(root==null){
                return new Node(data);
            }
            else{
                Node cur;
                if(data<=root.data){
                    cur=insert(root.left,data);
                    root.left=cur;
                }
                else{
                    cur=insert(root.right,data);
                    root.right=cur;
                }
                return root;
            }
        }
        static void Main(String[] args){
            Node root=null;
            int T=Int32.Parse(Console.ReadLine());
            while(T-->0){
                int data=Int32.Parse(Console.ReadLine());
                root=insert(root,data);            
            }
            levelOrder(root);
            
        }
    }

I love this question, it shows the combination of data structure *Queue* and *generics* and it makes use of the feature of `Queue` to solve this problem that show the node level by level, from left to right, top to bottom. It's really a good idea.  
 

***




