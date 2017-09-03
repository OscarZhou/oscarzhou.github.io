---
title: "Hacker Rank: Data Types (Day 1)"
date: 2017-07-26
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## About Code Challenge

Honestly, I do not like this kind of stuff, but I have to improve the ability in this aspect in order to get a job. Hah. The desire to achieve some goals is always the motivation to move forward. So, today I will start a code challenge issued by Hacker Rank. It is a 30 days' challenges. In every day, there is a tutorial video which shows the topic about that day before the code challenges. And I want to note this process and show it in the same pattern, tutorial + code challenges.  

***

## Tutorial

Today's topic is data types, which is the necessary and basic knowledge for every programming languages. Although this tutorial is based on Java, it also give me some new views on C#. The presenter tutors very clearly and vividly so let's start to learn it.  



<p align="center">
<img src="/images/post/20170726001.png" alt="data type1" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

The class is the most important thing in a program. In fact, the program consists of all kinds of classes. And the class consists of properties and methods. For example, we crate a class `Car`. It can be like this.  
  

    class Car
    {
        private int maxSpeed = 100;
        private int minSpeed = 0;
        private double weight = 4079;
        private bool isTheCarOn = false;
        private char condition = 'A';

        public void printVariables()
        {
            Console.WriteLine("This is the maxSpeed " + maxSpeed);
            Console.WriteLine(minSpeed);
            Console.WriteLine(weight);
            Console.WriteLine(isTheCarOn);
            Console.WriteLine(condition);
            Console.WriteLine(nameOfCar);
           
        }
    }

<p align="center">
<img src="/images/post/20170726002.png" alt="data type2" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


<p align="center">
<img src="/images/post/20170726003.png" alt="data type3" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>


<p align="center">
<img src="/images/post/20170726004.png" alt="data type4" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

In C#, we have the same thing. `int`, `double` ,`bool` and `char` are the primitive data type.  


<p align="center">
<img src="/images/post/20170726005.png" alt="data type5" width="70%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

Of course, the class `Car` is the referenced data type. If you assign a referenced variable to another referenced variable, both of them will share the same thing. I add the method `wreckCar` into the `Car`.  

    public void wreckCar()
    {
        condition = 'C';
    }

And I will create two objects, `FamilyCar` and `AliceCar`.  

    static void Main(string[] args)
    {
        Car familyCar = new Car();
        Console.WriteLine("Family's Car:");
        familyCar.printVariables();

        Car aliceCar = familyCar;
        familyCar.wreckCar();
        Console.WriteLine("Alice's Car:");
        aliceCar.printVariables();

        Console.ReadKey();

    }

You will see the result like below:  

<p align="center">
<img src="/images/post/20170726009.png" alt="data type9" width="70%"  /><br/>
<center><h5><b>Figure 9</b></h5></center>
</p>


<p align="center">
<img src="/images/post/20170726006.png" alt="data type6" width="70%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

<p align="center">
<img src="/images/post/20170726007.png" alt="data type7" width="70%"  /><br/>
<center><h5><b>Figure 7</b></h5></center>
</p>


<p align="center">
<img src="/images/post/20170726008.png" alt="data type8" width="70%"  /><br/>
<center><h5><b>Figure 8</b></h5></center>
</p>

It's quite interesting, isn't it?  

***

## Code Challenge

The first challenge is about I/O operation. When you read some variables, you can call `Console.ReadLine()`, but the type is string. If you want the input value to be `int` or `double` type, you can use `Convert.ToInt32()` or `Convert.ToDouble()`, but we should notice that you can not use `Console.Read()` as the input value because it is used for reading the next character from the standard input stream.  

Another thing is that if you want to show the double number, you can use `xxx.ToString("0.0");`.  



