---
title: "Observer Design Pattern"
date: 2017-09-12
category: Other
tags: [design pattern, observer]
comments: true

---

## When to use

When you need many other objects to receive an update when another object changes.  
* Stock market with thousands of stocks needs to send updates to objects representing individual stocks.  
* The Subject (publisher) sends many stocks to the Observers  
* The Observers (subscribers) takes the ones they want and use them  

***

## Strengths
* Loose coupling is a benefit  
    * The Subject (publisher) doesn't need to know anything about the Observers (subscribers)

***
## Negatives

* The Subject (publisher) may send updates that don't matter to the Observer (subscriber)

  
*** 

## Relation diagram


<p align="center">
<img src="/images/post/20170912002.png" alt="observer diagram" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

The **Observer pattern** is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.  

***

## Code implementation

**Subject.cs**  
  
    /*
     * This interface handles adding, deleting and updating
     * all observers
     */
    public interface Subject
    {
        void Register(Observer o);

        void Unregister(Observer o);

        void NotifyObserver();
    }

**Observer.cs**  

    /*
     * The Observers update method is called when the Subject changes
     */
    public interface Observer
    {
        new void Update(double ibmPrice, double aaplPrice, double googPrice);
    }


**StockGrabber.cs**  

    // Uses the Subject interface to update all Observers
    public class StockGrabber: Subject
    {
        private List<Observer> observers;

        private double ibmPrice;
        private double aaplPrice;
        private double googPrice;

        public StockGrabber()
        {
            // Creates an List to hold all observers
            observers = new List<Observer>();
        }
        public void Register(Observer newObserver)
        {
            observers.Add(newObserver);
        }

        public void Unregister(Observer deleteObserver)
        {
            // Get the index of the observer to delete
            int observerIndex = observers.IndexOf(deleteObserver);

            // Print out message (Have to increment index to match)
            Console.WriteLine("Observer " + (observerIndex + 1) + " deleted");

            // Removes observer from the List
            observers.RemoveAt(observerIndex);
        }

        public void NotifyObserver()
        {
            // cycle through all observers and notifies them of price changes

            foreach (Observer observer in observers)
            {
                observer.Update(ibmPrice, aaplPrice, googPrice);
            }
        }

        public void setIBMPrice(double newIBMPrice)
        {
            this.ibmPrice = newIBMPrice;
            NotifyObserver();
        }

        public void setAPPLPrice(double newAPPLPrice)
        {
            this.aaplPrice = newAPPLPrice;
            NotifyObserver();
        }

        public void setGOOGPrice(double newGOOGPrice)
        {
            this.googPrice = newGOOGPrice;
            NotifyObserver();
        }
    }
        
**StockObserver.cs**  

    // Represents each Observer that is monitoring changes in the subject
    public class StockObserver:Observer
    {
        private double ibmPrice;
        private double applPrice;
        private double googPrice;

        // Static used as a counter
        private static int observerIDTracker = 0;

        // Used to track the observers
        private int observerID;

        // Will hold reference to the StockGrabber object
        private Subject stockGrabber;

        public StockObserver(Subject stockGrabber)
        {
            /*
             * Store the reference to the stockGrabber object so
             * I can make calls to its methods
             */
            this.stockGrabber = stockGrabber;

            // Assign an observer ID and increment the static counter
            this.observerID = ++observerIDTracker;

            // Message notifies user of new observer
            Console.WriteLine("New Observer " + this.observerID);

            // Add the observer to the Subjects List
            stockGrabber.Register(this);
        }

        public void Update(double ibmPrice, double applPrice, double googPrice)
        {
            this.ibmPrice = ibmPrice;
            this.applPrice = applPrice;
            this.googPrice = googPrice;

            printThePrices();
        }

        public void printThePrices()
        {
            Console.WriteLine(observerID + "\nIBM: " + ibmPrice +
                              "\nAAPL: " + applPrice + "\nGOOG: " + googPrice + "\n");
        }
    }
    
**GrabStocks.cs**
    
    public class GrabStocks
    {
        public static void Main(string[] args)
        {
            StockGrabber stockGrabber = new StockGrabber();

            StockObserver observer1 = new StockObserver(stockGrabber);

            stockGrabber.setIBMPrice(197.00);
            stockGrabber.setAPPLPrice(677.60);
            stockGrabber.setGOOGPrice(676.40);
            
            StockObserver observer2 = new StockObserver(stockGrabber);

            stockGrabber.setIBMPrice(197.00);
            stockGrabber.setAPPLPrice(677.60);
            stockGrabber.setGOOGPrice(676.40);

            stockGrabber.Unregister(observer1);

            stockGrabber.setIBMPrice(197.00);
            stockGrabber.setAPPLPrice(677.60);
            stockGrabber.setGOOGPrice(676.40);

            Console.ReadKey();
        }
    }
    
And then we will see the result like below:  

<p align="center">
<img src="/images/post/20170912003.png" alt="observer diagram result" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

****

Please take some time to digest observer design pattern. I found that design patterns are really awesome things which are created by those genius in this field.  
  
  

