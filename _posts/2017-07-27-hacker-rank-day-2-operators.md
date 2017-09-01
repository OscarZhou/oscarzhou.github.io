---
title: "Hacker Rank: Variables and Arithmetic(Day 2)"
date: 2017-07-27
category: Code Challenge
tags: [C#, Tutorial, Code Challenge]
comments: true

---

## Tutorial

The operators is another necessary knowledge to be learned in programming language. In this lecture, we also introduce some relevant knowledge points. If you are an experienced programmer, just skip it. Anyway, let's have a briefly review.  
  
We will continue to build up the class `Car` to explain this part.  


<p align="center">
<img src="/images/post/20170727001.png" alt="variables" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


We can add 4 properties more and some methods, like,  

    private double maxFuel = 16;
    private double currentFuel = 8;
    private double mpg = 26.4; // miles per gallon
    private int numberOfPeopleInCar = 1;


    public void getIn()
    {
        numberOfPeopleInCar = numberOfPeopleInCar + 1;
    }

    public void getOut()
    {
        numberOfPeopleInCar--;
    }

    public double howManyMilesTillOutOfGas()
    {
        return currentFuel * mpg;
    }

    public double maxMilesPerFillUp()
    {
        return maxFuel * mpg;
    }
    

<p align="center">
<img src="/images/post/20170727002.png" alt="constructor" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

You know, we do not always use the default values, so it is where the methods come into the picture. We usually modify the value of properties via the methods. And there is one special method called Constructor, which is used for initializing some properties.  
 
    public Car(int customMaxSpeed, double customWeight, bool customIsTheCarOn)
    {
        maxSpeed = customMaxSpeed;
        weight = customWeight;
        isTheCarOn = customIsTheCarOn;
    }


**Parameter:** A variable in a function that refers to input data.  
**Argument:** A piece of data passed into a function whose value becomes the value of the parameter.  

So when you call the those functions like,  

    static void Main(string[] args)
    {
        Car birthdayPresent = new Car(500, 5000.546, true);
        Console.WriteLine("Birthday Car v1");
        birthdayPresent.printVariables();
        birthdayPresent.getIn();
        birthdayPresent.getIn();
        birthdayPresent.getIn();
        Console.WriteLine("Miles Left:" + birthdayPresent.howManyMilesTillOutOfGas());
        Console.WriteLine("Max Miles" + birthdayPresent.maxMilesPerFillUp());

        Console.WriteLine("Birthday Car v2");
        birthdayPresent.printVariables();
        birthdayPresent.getOut();

        Console.WriteLine("Birthday Car v3");
        birthdayPresent.printVariables();
    }

You will see the result,  

<p align="center">
<img src="/images/post/20170727003.png" alt="result" width="50%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

*** 


## Code Challenge

**Task**  
Given the meal price (base cost of a meal), tip percent (the percentage of the meal price being added as tip), and tax percent (the percentage of the meal price being added as tax) for a meal, find and print the meal's total cost.  

**Note:** Be sure to use precise values for your calculations, or you may end up with an incorrectly rounded result!

**Input Format**  

There are  lines of numeric input: 
The first line has a double,  *meanCost*(the cost of the meal before tax and tip).   
The second line has an integer,  *tipPercent*(the percentage of *mealCost* being added as tip).   
The third line has an integer,  *taxPercent*(the percentage of *mealCost* being added as tax).  

**Output Format**  

Print The total meal cost is totalCost dollars., where *totalCost* is the rounded integer result of the entire bill (mealCost with added tax and tip).  

**Solution**  
    
    using System;
    using System.Collections.Generic;
    using System.IO;
    class Solution {
        static void Main(String[] args) {
            /* Enter your code here. Read input from STDIN. Print output to STDOUT. Your class should be named Solution */
        
            double meanCost;
            int tipPercent;
            int taxPercent;
            
            meanCost = Convert.ToDouble(Console.ReadLine());
            tipPercent = Convert.ToInt32(Console.ReadLine());
            taxPercent = Convert.ToInt32(Console.ReadLine());
            
            double totalCost = meanCost + (meanCost * tipPercent / 100.0) + (meanCost * taxPercent / 100.0);
            Console.WriteLine("The total meal cost is {0} dollars.", Math.Round(totalCost) );
        }
        
    }
    
***

This is quite easy. Let's move Day 3.  



