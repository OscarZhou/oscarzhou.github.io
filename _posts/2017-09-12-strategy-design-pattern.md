---
title: "Strategy Design Pattern"
date: 2017-09-12
category: Other
tags: [design pattern, strategy]
comments: true

---

## When to use

When you want to define a class that will have one behavior that is similar to other behaviors in a list.  
E.g. I want the class object to be able to choose from  
* Not Flying  
* Fly with wings  
* Fly super fast  

When you need to use one of several behaviors dynamically.  

***

## Strengths
* Often reduces long lists of conditionals
* Avoids duplicate code
* Keeps class changes from forcing other class changes
* Can hide complicated / secret code from the user

***
  
## Negatives

* Increased Number of Objects / Classes
  
*** 

## Relation diagram


<p align="center">
<img src="/images/post/20170912001.png" alt="strategy diagram" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

Define a family of algorithms, encapsulate each one, and make them interchangeable. The Strategy pattern lets the algorithm vary independently from clients that use it.  


***

## Code implementation

**Animal.cs**  
  
    public class Animal
    {
        private string name;
        private double height;
        private int weight;
        private string favFood;
        private double speed;
        private String sound;

        /*
            Instead of using an interface in a traditional way
         *  we use an instance variable that is a subclass
         *  of the Flys interface.
         *  
         *  Animal doesn't care what flyingType does, it just
         *  knows the behavior is available to its subclass
         *  
         *  This is known as Composition : Instead of inheriting
         *  an ability through inheritance the class is composed
         *  with Objects with the right ability
         *  
         *  Composition allows you to change the capabilities of 
         *  objects at run time!
         */
        public Flys flyingType;
        
        public void setName(string newName)
        {
            name = newName;
        }

        public string getName()
        {
            return name;
        }

        public void setHeight(double newHeight)
        {
            height = newHeight;
        }

        public double getHeight()
        {
            return height;
        }

        public void setWeight(int newWeight)
        {
            if (newWeight > 0)
            {
                weight = newWeight;
            }
            else
            {
                Console.WriteLine("Weight must be bigger than 0");
            }
        }

        public double getWeight()
        {
            return weight;
        }

        public void setFavFood(string newFavFood)
        {
            favFood = newFavFood;

        }

        public string getFavFood()
        {
            return favFood;
        }

        public void setSpeed(double newSpeed)
        {
            speed = newSpeed;

        }

        public double getSpeed()
        {
            return speed;
            
        }

        public void setSound(string newSound)
        {
            sound = newSound;
        }

        public string getSound()
        {
            return sound;
        }

        public string tryToFly()
        {
            return flyingType.fly();
        }

        public void setFlyingAbility(Flys newFlyType)
        {
            flyingType = newFlyType;
        }
    }

**Dog.cs**  

    public class Dog: Animal
    {
        public void digHole()
        {
            Console.WriteLine("Dug a hole");
        }
        public Dog():base()
        {
            setSound("Bark");

            /*
             * We set the Flys interface polymorphically
             * This sets the behavior as a non-flying Animal    
             */

            flyingType = new CantFly();
        }
    }

**Bird.cs**  

    public class Bird:Animal
    {
        public Bird() : base()
        {
            setSound("Tweet");

            flyingType = new ItFlys();
        }
    }
    
**Flys.cs**  

    public interface Flys
    {
        string fly();
    }

    public class ItFlys : Flys
    {
        public string fly()
        {
            return "Flying high";
        }
    }

    public class CantFly : Flys
    {
        public string fly()
        {
            return "I can't fly";
        }
    }
    
**AnimalPlay.cs**
    
    class AnimalPlay
    {
        static void Main(string[] args)
        {
            Animal sparky = new Dog();
            Animal tweety = new Bird();
            
            Console.WriteLine("Dog: "+ sparky.tryToFly());
            Console.WriteLine("Bird: "+ tweety.tryToFly());

            //This allows dynamic changes for flyingType
            sparky.setFlyingAbility(new ItFlys());
            Console.WriteLine("Dog: " + sparky.tryToFly());
    }
    
    
<p align="center">
<img src="/images/post/20170912004.png" alt="strategy diagram result" width="50%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


****

Please take some time to digest strategy design pattern.  
  

