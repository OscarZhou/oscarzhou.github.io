---
title: "Class and Object"
date: 2017-05-08

---


In this article, I will give the answers to some questions the beginners of programming may ask. The mode is a little like Q&A.

#### 1. **How do you determine how many class you will write?**  
This question belongs to the problem of the software requirement and design. Usually we extract the user cases from the functionality analysis and the requriment document and then determine what kind of class do we need.

#### 2. **What is the kind of content are there in a class?**  
This question belongs to the problem of basic structure of the class. Obviously, a class includes attributes and methods.


#### 3. **Is there any specific methods or techniques to create a class?**  
This question belongs to the problem of basic principle of object-oriented development. You need to spend time experiencing, accumulating and summary.  

The principle you must master in object-oriented development:  
    1. One object is only responsible for one thing. It must concentrate. Too many responsibilities are easy to cause uncertainty so that the program is not stable.  
    2. When the requirement changes, the modification of the class should be as little as possible. The best stragegy is to meet the needs by adding the extension class.  
    3. It is necessary to program based on interface. The module in the high layer calls the interface and the module in the low layer implements the interface just in case that the low layer influences the high layer.  
    4. The specific interfaces are encouraged to use as much as possible, but this kind of interface should be simple rather than complex.  
    5. In the inherited architecture, the derived class can replace the basic class, the virtual machine can find the specific derived class according to the variable of the basic class so that the polymorphism is implemented.
    
#### 4. **How do you determine the relation between classes?**  
This question is a problem about the cooperation between the software components. Different software has different design mode.



#### 5. **When the program runs, how the class changes?**  
This is a problem about creation and use of objects. Class is a template. Only objects do work when the program runs.


