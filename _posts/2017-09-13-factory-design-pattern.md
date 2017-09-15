---
title: "Factory Design Pattern"
date: 2017-09-13
category: Other
tags: [design pattern, factory]
comments: true

---

## What is factory pattern

When a method returns one of several possible classes that share a common super class.  
* Create a new enemy in a game
* Random number generator picks a number assigned to a specific enemy
* The factory returns the enemy associated with that number

The class is chosen at run time.  

## When to use

* When you don't know ahead of time what class object you need
* When all of the potential classes are in the same subclass hierarchy
* To centralize class selection code
* When you don't want the user to have to know every subclass
* To encapsulate object creation


***

## Relation diagram


<p align="center">
<img src="/images/post/20170913001.png" alt="factory diagram" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

The **Factory Pattern** allows you to create objects without specifying the exact class of object that will be created.  


***

## Code implementation

**EnemyShip.cs**  

    public abstract class EnemyShip
    {
        private string name;
        private double amtDamage;

        public string getName()
        {
            return name;
        }

        public void setName(string newName)
        {
            name = newName;
        }

        public double getDamage()
        {
            return amtDamage;
        }

        public void setDamage(double newDamage)
        {
            amtDamage = newDamage;
        }

        public void followHeroShip()
        {
            Console.WriteLine(getName()+" is following the hero");
        }

        public void displayEnemyShip()
        {
            Console.WriteLine(getName()+" is on the screen");
        }

        public void enemyShipShoots()
        {
            Console.WriteLine(getName()+" attacks and does "+ getDamage());
        }
    }


**UFOEnemyShip.cs**  

    public class UFOEnemyShip : EnemyShip
    {
        public UFOEnemyShip()
        {
            setName("UFO Enemy Ship");
            setDamage(20.0);
        }
    }

**RocketEnemyShip.cs**  

    public class RocketEnemyShip: EnemyShip
    {
        public RocketEnemyShip()
        {
            setName("Rocket Enemy Ship");
            setDamage(10.0);
        }
    }
    
**BigUFOEnemyShip.cs**  

    public class BigUFOEnemyShip:UFOEnemyShip
    {
        public BigUFOEnemyShip()
        {
            setName("Big UFO enemy ship");
            setDamage(40.0);
        }
    }
    
**EnemyShipFactory.cs**
    
    public class EnemyShipFactory
    {
        public EnemyShip makeEnemyShip(string newShipType)
        {
            EnemyShip newShip = null;
            if (newShipType.Equals("U"))
            {
                return new UFOEnemyShip();
            }
            else if (newShipType.Equals("R"))
            {
                return new RocketEnemyShip();
            }
            else if (newShipType.Equals("B"))
            {
                return new BigUFOEnemyShip();
            }
            else
            {
                return null;
            }
        }
    }
    
**EnemyShipTesting.cs**    

    public class EnemyShipTesting
    {
        public static void Main(string[] args)
        {
            EnemyShipFactory shipFactory = new EnemyShipFactory();

            EnemyShip theEnemy = null;

            Console.WriteLine("What type of ship? (U /R /B)");
            string typeOfShip = Console.ReadLine();
            theEnemy = shipFactory.makeEnemyShip(typeOfShip);
            if (theEnemy != null)
            {
                doStuffEnemy(theEnemy);    
            }
            else
            {
                Console.WriteLine("Please enter U, R or B next time");
            }
        }

        public static void doStuffEnemy(EnemyShip anEnemyShip)
        {
            anEnemyShip.displayEnemyShip();
            anEnemyShip.followHeroShip();
            anEnemyShip.enemyShipShoots();

            Console.ReadKey();
        }
    }
    
And you will see the result is like below:  


<p align="center">
<img src="/images/post/20170913002.png" alt="factory diagram" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

****

I found that I am falling love with the design patterns.  

  

