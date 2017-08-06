---
title: "Optional Parameters and Named Parameters"
date: 2017-05-28
category: C#
tags: [C#, MVC, Optional Parameter, Named Parameter]
comments: true

---

## Why is the optional parameters necessary?  

If there are too many parameters in a method, it would be very annoying when you try to call it. However, optional parameters are those that you don't need to pass all of them every time. So it's more convenient. The optional parameters are also called default parameters.  

## The grammer requirement
`[modifier] [return value] [function name](parameter 1, parameter 2,... parameter n, optional parameter 1, opritional parameter 2, ... optional parameter n)`

The big difference between the required parameter and the optional parameter is that even if we didn't pass the optional parameter, there will be a default parameter as the input value. Thus, the required parameters are not necessary, but the optional parameters must put the back of the required parameters.  

The code example is like below:

    class OptionalParameter
    {
        public void QueryStudent(string stuName, string className = "Computer A", int age = 20)
        {
            Console.WriteLine("Your query condition is student name={0}, class name={1}, age={2}", stuName, className, age);
        }
    }

The main function is: 

    static void Main(string[] args)
    {
        OptionalParameter.OptionalParameter op = new OptionalParameter.OptionalParameter();
        op.QueryStudent("John");
        op.QueryStudent("John", "Design B");
        op.QueryStudent("John", "Design C", 22);
    
        Console.ReadKey();
    }


The result is:  

<p align="center">
<img src="/images/post/20170528001.png" alt="optional parameter" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

As you see, in the parts where there isn't passing value, the default value will be replaced.   


***
## Why use the named parameters?  

**Merit:** Allow to ignore the sequence of the parameters  
**Feature:** When calling the method, you can use the name of the parameter to assign a value to this parameter. The advantage of this feature is that you can clearly see the corresponding value of each parameter, which enhanced the readability of the code.  
  
## The grammer requirement
`[method name](parameter name 1: value 1,... parameter name n:value n, optional parameter name 1: value1, ... optional parameter name n: value n)`

Please noted that the parameter names and the parameter values should be separated by colon, and the parameter 1 isn't necessarily the first parameter in the method. Please look at the example below:

    static void Main(string[] args)
    {
        OptionalParameter.OptionalParameter op = new OptionalParameter.OptionalParameter();
        op.QueryStudent("John");
        op.QueryStudent("John", "Design B");
        op.QueryStudent(age:22, className: "Design C", stuName:"John" );
    
        Console.ReadKey();
    }

The result is:  

<p align="center">
<img src="/images/post/20170528001.png" alt="optional parameter" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

Noted the last calling, you will find that the sequence is not fixed. Sometimes this kind of function is very useful. And when you check the route file in MVC, you can find that they also use this feature.  
                                                                      
<p align="center">
<img src="/images/post/20170528002.png" alt="route feature" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

***

Hope this article is helpful to you.  
