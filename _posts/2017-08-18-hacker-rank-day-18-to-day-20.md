---
title: "Hacker Rank: Day 18 to Day 20"
date: 2017-08-18
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Introduction

Let's the rest of the questions. They are very interesting.  


*** 


## Code Challenge

### Day 18: Queues and Stacks

**Task**   
A **palindrome** is a word, phrase, number, or other sequence of characters which reads the same backwards and forwards. Can you determine if a given string, *s*, is a palindrome?  

To solve this challenge, we must first take each character in *s*, enqueue it in a queue, and also push that same character onto a stack. Once that's done, we must dequeue the first character from the queue and pop the top character off the stack, then compare the two characters to see if they are the same; as long as the characters match, we continue dequeueing, popping, and comparing each character until our containers are empty (a non-match means *s* isn't a palindrome).  

Write the following declarations and implementations:  

1. Two instance variables: one for your *stack*, and one for your *queue*.  
2. A void pushCharacter(char ch) method that pushes a character onto a stack.  
3. A void enqueueCharacter(char ch) method that enqueues a character in the  instance variable.  
4. A char popCharacter() method that pops and returns the character at the top of the  instance variable.  
5. A char dequeueCharacter() method that dequeues and returns the first character in the  instance variable.  
  
**Input Format**  
You do not need to read anything from stdin. The locked stub code in your editor reads a single line containing string *s*. It then calls the methods specified above to pass each character to your instance variables.  
  


**Output Format**  

You are not responsible for printing any output to stdout.   
If your code is correctly written and *s* is a palindrome, the locked stub code will print *The word, s, is a palindrome.*; otherwise, it will print *The word, s, is not a palindrome.*  
  
 
**Sample Input**  

<mark>
racecar
</mark>

**Sample Output**  

<mark>
The word, racecar, is a palindrome.
</mark>

**Code**  
  
    using System;
    using System.Collections.Generic;
    using System.IO;

    class Solution {
        //Write your code here
        private Stack<char> s = new Stack<char>();
        private Queue<char> q = new Queue<char>();
                
        public void pushCharacter(char ch)
        {
            s.Push(ch);
        }
        
        public void enqueueCharacter(char ch)
        {
            q.Enqueue(ch);
        }
        
        public char popCharacter()
        {
            return s.Pop();
        }
        
        public char dequeueCharacter()
        {
            return q.Dequeue();
        }
        static void Main(String[] args) {
                // read the string s.
                string s = Console.ReadLine();
                
                // create the Solution class object p.
                Solution obj = new Solution();
                
                // push/enqueue all the characters of string s to stack.
                foreach (char c in s) {
                    obj.pushCharacter(c);
                    obj.enqueueCharacter(c);
                }
                
                bool isPalindrome = true;
                
                // pop the top character from stack.
                // dequeue the first character from queue.
                // compare both the characters.
                for (int i = 0; i < s.Length / 2; i++) {
                    if (obj.popCharacter() != obj.dequeueCharacter()) {
                        isPalindrome = false;
                        
                        break;
                    }
                }
                
                // finally print whether string s is palindrome or not.
                if (isPalindrome) {
                    Console.Write("The word, {0}, is a palindrome.", s);
                } else {
                    Console.Write("The word, {0}, is not a palindrome.", s);
                }
            }
        }

For this problem, understanding the feature of `Stack` and `Queue` is the big deal.  
1. Stack is Last-In-First-Out(LIFO).   
2. Queue is First-In-First-Out(FIFO).  


***
    
### Day 19: Interface


**Task**  
The *AdvancedArithmetic* interface and the method declaration for the abstract int *divisorSum(int n)* method are provided for you in the editor below. Write the Calculator class, which implements the *AdvancedArithmetic* interface. The implementation for the *divisorSum* method must be public and take an integer parameter, *n*, and return the sum of all its divisors.  
  
**Input Format**  

A single line containing an integer, *n*.  

**Output Format**  

You are not responsible for printing anything to stdout. The locked *Solution* class in the editor below will call your code and print the necessary output.  


**Sample Input**  
<mark>
6
</mark>

**Sample Output**  

<mark>
I implemented: AdvancedArithmetic<br/>
12<br/>
</mark>


**Code**  

    using System;
    public interface AdvancedArithmetic{
        int divisorSum(int n);
    }  
    //Write your code here
    class Calculator:AdvancedArithmetic
    {
      public int divisorSum(int n)
      {
          int sum =0;
          for(int i =1; i<=n; i++)
          {
              if(n%i==0)
              {
                  sum+= i;
              }
          }
          return sum;
      }
    }

    class Solution{
        static void Main(string[] args){
            int n = Int32.Parse(Console.ReadLine());
            AdvancedArithmetic myCalculator = new Calculator();
            int sum = myCalculator.divisorSum(n);
            Console.WriteLine("I implemented: AdvancedArithmetic\n" + sum); 
        }
    }

`interface` is kind of like `abstract`, there is only the declaration of method. You can complete specific contents in the class that inherits interface.  
 

*** 

   
### Day 20: Sorting

**Task**  

Given an array, *a*, of size *n* distinct elements, sort the array in ascending order using the Bubble Sort algorithm above. Once sorted, print the following *3* lines:  

1. Array is sorted in numSwaps swaps.   
where *numSwaps* is the number of swaps that took place.  

2. First Element: firstElement   
where *firstElement* is the first element in the sorted array.  

3. Last Element: lastElement   
where *lastElement* is the last element in the sorted array.  




**Input Format**  

The first line contains an integer, *n*, denoting the number of elements in array *a*. 
The second line contains *n* space-separated integers describing the respective values of *a*<sub>0</sub>,a<sub>0</sub>,...,a<sub>n-1</sub>.  
  

**Output Format**  

Print the following three lines of output:  

1. Array is sorted in numSwaps swaps.   
where *numSwaps* is the number of swaps that took place.  
2. First Element: firstElement   
where *firstElement* is the first element in the sorted array.  
3. Last Element: lastElement   
where *lastElement* is the last element in the sorted array.  

**Sample Input**  
<mark>
3<br/>
1 2 3<br/>
</mark>

**Sample Output**  

<mark>
Array is sorted in 0 swaps<br/>
First Element: 1<br/>
Last Element: 3<br/>
</mark>


**Code**  
    
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    class Solution {
    
        static void Main(String[] args) {
            int n = Convert.ToInt32(Console.ReadLine());
            string[] a_temp = Console.ReadLine().Split(' ');
            int[] a = Array.ConvertAll(a_temp,Int32.Parse);
            // Write Your Code Here
            int counter = 0;
            for(int i = 0; i<n-1; i++)
            {
                for(int j=i+1; j<n; j++)
                {
                    int tmp=0;
                    if(a[i]>a[j])
                    {
                        tmp = a[i];
                        a[i] = a[j];
                        a[j] = tmp;
                        counter++;
                    }
                }
            }
            Console.WriteLine("Array is sorted in {0} swaps.\nFirst Element: {1}\nLast Element: {2}", counter, a[0], a[n-1]);
        }
    }

In this problem, you need clearly to know what *Bubble sort* is. That will help you to implement this problem.  


***




