---
title: "Hacker Rank: Day 3 to Day 14"
date: 2017-08-10
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Introduction

The recent code challenges are relative simple, so I pick some questions that are easy to make mistake from them to show you.  


*** 


## Code Challenge

### Day 8: Dictionaries and Maps

**Task**   
Given *n* names and phone numbers, assemble a phone book that maps friends' names to their respective phone numbers. You will then be given an unknown number of names to query your phone book for. For each *name* queried, print the associated entry from your phone book on a new line in the form name=phoneNumber; if an entry for *name* is not found, print Not found instead.  


**Input Format**  
The first line contains an integer, *n*, denoting the number of entries in the phone book. 
Each of the *n* subsequent lines describes an entry in the form of *n* space-separated values on a single line. The first value is a friend's name, and the second value is an *n*-digit phone number.  

After the *n* lines of phone book entries, there are an unknown number of lines of queries. Each line (query) contains a *name* to look up, and you must continue reading lines until there is no more input.  

**Output Format**  

On a new line for each query, print Not found if the name has no corresponding entry in the phone book; otherwise, print the full *name* and *phoneNumber* in the format name=phoneNumber.  

**Sample Input**  

<mark>
3<br/>
sam 99912222<br/>
tom 11122222<br/>
harry 12299933<br/>
sam<br/>
edward<br/>
harry<br/>
</mark>

**Sample Output**  

<mark>
sam=99912222<br/>
Not found<br/>
harry=12299933<br/>
</mark>

**Code**  

        using System;
        using System.Collections.Generic;
        using System.IO;
        class Solution {
            static void Main(String[] args) {
                /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution */
                int n = Convert.ToInt32(Console.ReadLine());
                Dictionary<string, string> phoneBook = new Dictionary<string, string>();
                for (int i = 0; i < n; i++)
                {
                    string[] s= Console.ReadLine().Split(' ');
                    string name = s[0];
                    string phoneNumber = s[1];
                    phoneBook.Add(name, phoneNumber);
                }
                while (true)
                {
                    string nameToLookUp = Console.ReadLine();
                    if (phoneBook.ContainsKey(nameToLookUp))
                    {
                        string phoneNumber = phoneBook[nameToLookUp];
                        Console.WriteLine("{0}={1}", nameToLookUp, phoneNumber);
                    }
                    else
                    {
                        Console.WriteLine("Not found");
                    }
                }
            }
        }
    
We should note that `string[] s= Console.ReadLine().Split(' ');`. The type of the return value should be `string p[]`. Another point that should be noted is `if (phoneBook.ContainsKey(nameToLookUp))`. Before using the key, we have to determine to check if it is existed.  

***
    
### Day 9: Recursion  

<p align="center">
<img src="/images/post/20170810001.png" alt="recursion" width="50%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


**Task**  
Write a factorial function that takes a positive integer, *N* as a parameter and prints the result of *N* (*N* factorial).  

**Input Format**  

A single integer, N (the argument to pass to factorial).  
  

**Output Format**  

Print a single integer denoting *N!*.  

**Sample Input**  
<mark>
3
</mark>

**Sample Output**  

<mark>
6
</mark>

**Code**  

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {
    
        static int factorial(int n) {
            // Complete this function
            if(n>1)
            {
                return n*factorial(n-1);
            }
            else
            {
                return 1;
            }
        }
    
        static void Main(String[] args) {
            int n = Convert.ToInt32(Console.ReadLine());
            int result = factorial(n);
            Console.WriteLine(result);
        }
    }

The most important thing for recursion is that there must be end point, like `return 1`.  


*** 

   
### Day 10: Binary Numbers  


**1. Binary to Decimal Conversion**  

(210)<sub>10</sub>=(2 * 10<sup>2</sup>)+(1 * 10<sup>1</sup>)+(0 * 10<sup>0</sup>)  


**2. Decimal to Binary Conversion**  

4 / 2 = 2 remainder 0  
2 / 2 = 1 remainder 0  
1 / 2 = 0 remainder 1  


This can be expressed in pseudocode as  
    
    while(n > 0):
        remainder = n%2;
        n = n/2;
        Insert remainder to front of a list or push onto a stack
    
    Print list or stack
    
    

**Task**  
Given a base-*10* integer, , convert it to binary (base-*2*). Then find and print the base-*10* integer denoting the maximum number of consecutive *1*'s in *n*'s binary representation.

**Input Format**  

A single integer, N (the argument to pass to factorial).  
  

**Output Format**  

A single integer, *n*.  


**Sample Input**  
<mark>
13
</mark>

**Sample Output**  

<mark>
2
</mark>

**Explanation**  

The binary representation of *13* is *1101*, so the maximum number of consecutive *1*'s is *2*.  
  
**Code**  

    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {
        static void Main(String[] args) {
            int n = Convert.ToInt32(Console.ReadLine());
            int maxCount=0, consecutiveCount=0;
            int remainder=0;
            while(true)
            {
                remainder = n %2; 
                n = n/2;
                if(remainder == 1)
                {
                    consecutiveCount++;
                    if(consecutiveCount > maxCount)
                    {
                        maxCount = consecutiveCount;    
                    }
                }
                else
                {
                    consecutiveCount = 0;
                }
                if(n==0)
                {
                    break;
                }
            }
            Console.WriteLine(maxCount);
        }
    }

We should note that `remainder = n % 2;` which means that the reverse order of all remainders is the final binary number.  


*** 

### Day 12: Inheritance  
 
**Code**  

    using System;
    using System.Linq;
    
    class Person{
        protected string firstName;
        protected string lastName;
        protected int id;
        
        public Person(){}
        public Person(string firstName, string lastName, int identification){
                this.firstName = firstName;
                this.lastName = lastName;
                this.id = identification;
        }
        public void printPerson(){
            Console.WriteLine("Name: " + lastName + ", " + firstName + "\nID: " + id); 
        }
    }
    
    class Student : Person{
        private int[] testScores;  
      
        /*	
        *   Class Constructor
        *   
        *   Parameters: 
        *   firstName - A string denoting the Person's first name.
        *   lastName - A string denoting the Person's last name.
        *   id - An integer denoting the Person's ID number.
        *   scores - An array of integers denoting the Person's test scores.
        */
        // Write your constructor here
        public Student(string firstName, string lastName, int identification, int[] testScores):base(firstName, lastName, identification)
        {
            this.testScores = testScores;
        }
        /*	
        *   Method Name: Calculate
        *   Return: A character denoting the grade.
        */
        // Write your method here
        public char Calculate()
        {
            int sum = 0;
            foreach(int score in testScores)
            {
                sum += score;
            }
            int average = sum/testScores.Length;
            if(average >= 90 && average <=100)
            {
                return 'O';
            }
            else if(average >= 80 && average < 90)
            {
                return 'E';
            }
            else if(average >= 70 && average < 80)
            {
                return 'A';
            }
            else if(average >= 55 && average < 70)
            {
                return 'P';
            }
            else if(average >= 40 && average < 55)
            {
                return 'D';
            }
            else
            {
                return 'T';
            }
        }
    }

    class Solution {
        static void Main() {
            string[] inputs = Console.ReadLine().Split();
            string firstName = inputs[0];
            string lastName = inputs[1];
            int id = Convert.ToInt32(inputs[2]);
            int numScores = Convert.ToInt32(Console.ReadLine());
            inputs = Console.ReadLine().Split();
            int[] scores = new int[numScores];
            for(int i = 0; i < numScores; i++){
                scores[i]= Convert.ToInt32(inputs[i]);
            } 
            
            Student s = new Student(firstName, lastName, id, scores);
            s.printPerson();
            Console.WriteLine("Grade: " + s.Calculate() + "\n");
        }
    }
    
In C#, we can use `public Student(string firstName, string lastName, int identification, int[] testScores):base(firstName, lastName, identification)` to inherit the the constructor of the base class. 


***
    

### Day 13: Abstract Class  

**Code**  
    
    using System;
    using System.Collections.Generic;
    using System.IO;
    abstract class Book
    {
        protected String title;
        protected  String author;
        
        public Book(String t,String a){
            title=t;
            author=a;
        }
        public abstract void display();
    }
        
    class MyBook : Book
    {
        public int price;
        public MyBook(String t, String a, int p):base(t, a)
        {
            this.price = p;
        }
        
        public override void display()
        {
            Console.WriteLine("Title: {0}",  base.title);
            Console.WriteLine("Author: {0}", base.author);
            Console.WriteLine("Price: {0}", this.price);
        }
    }
    
    class Solution {
        static void Main(String[] args) {
          String title=Console.ReadLine();
          String author=Console.ReadLine();
          int price=Int32.Parse(Console.ReadLine());
          Book new_novel=new MyBook(title,author,price);
          new_novel.display();
        }
    }
    
In such situation, we can not assign the `public` in front of `class MyBook`, otherwise, we can not inherit `Book`. In addition, we can use `public override void display()` to override the abstract method in abstract class.  


***






