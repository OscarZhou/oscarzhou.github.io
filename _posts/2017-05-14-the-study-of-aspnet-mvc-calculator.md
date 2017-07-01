---
title: "The study of ASP.NET MVC (The sample of calculator)"
date: 2017-05-14

---

So now, let's start the study of MVC via a small project calculator. The requirement is that the calculator is used for calculating the average score of the students. The total score is known. What we should do is clicking a button and get the average. It is quite simple question.  

At the beginning of any projects, we should build up our start up page. In this project. I will set the `/Home/Index` as 
our start up page.  

### **/Home/Index**

        <%@ Page Language="C#" Inherits="System.Web.Mvc.ViewPage<dynamic>" %>
        <!DOCTYPE html>
        <html>
        <head runat="server">
            <meta name="viewport" content="width=device-width" />
            <title>Index</title>
        </head>
        <body>
            <div>
                <h2>This is my first ASP.NET program!</h2>
                <%
                    int[] scores = {99, 80, 87, 43};
                    int sum = 0;
                    foreach (int score in scores)
                    {
                        sum += score;
                    }                            
                %>
                <br/>
                The total score is 
                <%= sum %>
                <br/>
                <a href="/Calculator">Go to my calculator</a>
            </div>
        </body>
        </html>

We should note that the code in the `<% %>` symbol is C# code. This is one of the marvelous features of MVC, which allows to mix compile both C# and html code. And you will see the `<a>` label which helps us redirect to another page. The attribute of `<a>` is `/Calculator`. I will introduce it later.  
 
***

Okay, In fact, the first step should be finishing the class. So we create the MyCalculator class here.  
###  **/Models/Calculator**
        
        namespace MVCStudy.Models
        {
            public class MyCalculator
            {
                public int GetAverageScore(int sumScore, int sumSubjects)
                {
                    return sumScore / sumSubjects;
                }
            }
        }

It is quite easy. We finish the method of getting average score.  

***
Next, we need to build up the page of the calculator, because you have to have a page to display when the link "Go to my calculator" was clicked.  

### **/Calculator/MyCal**

        <%@ Page Language="C#" Inherits="System.Web.Mvc.ViewPage<dynamic>" %>
        <!DOCTYPE html>
        <html>
        <head runat="server">
            <meta name="viewport" content="width=device-width" />
            <title>MyCal</title>
        </head>
        <body>
            <div>
                <form method="post" action="/Calculator/GetAvgScore">
                    Sum Score:<input type="text" name="sumScore" /><br />
                    Sum Subject:<input type="text" name="sumSubject" /><br/>
                    <input type="submit" value="calculate"/>
                </form>
                <br/>
                <%= ViewData["avgScore"] %>
            </div>
        </body>
        </html>
        
So in this page, we have two input controls which receive the information of the sum score and the sum subject. And then we click the "submit" button, we will get the result on the page.  Don't worry about the `<%= ViewData["avgScore"] %>`, it's just used for displaying the data as we saved the result data into a kind of data structure called ViewData.  
  
***
### **/Controllers/CalculatorController**
        
        using MVCStudy.Models;
        using System;
        using System.Web.Mvc;
        namespace MVCStudy.Controllers
        {
            public class CalculatorController : Controller
            {
                // GET: /Calculator/
                public ActionResult Index()
                {
                    return View("MyCal");
                }
                public ActionResult GetAvgScore()
                {
                    //1 receive the submitted data
                    int sumScore = Convert.ToInt16(Request.Params["sumScore"]);
                    int sumSubject = Convert.ToInt16(Request.Params["sumSubject"]);
                    //2 call the model to process the data
                    int result = new MyCalculator().GetAverageScore(sumScore, sumSubject);
                    //3 return view (If it is necessary to return the data before returning the view, the data must be preserved)
                    ViewData["avgScore"] = "the average score is " + result;
                    return View("MyCal");
                }
            }
        }
        
In this part, we followed the steps we talked in last article to implement this function and saved the result into ViewData. Noted that we return to "MyCal", because there is not Index.aspx file in the Calculator folder. The same to Index() method. That's the reason why we use '/Calculator' in the link in the home page. Normally, the page will redirect to the Index() mehtod, but we set the return view as "MyCal", so we don't need to worry about the redirection. We will see the "MyCal" page for instead.  

 


  
***

If you want to read the relevant article, please go to [The study of ASP.NET MVC](http://oscarzhou.co.nz/blog/2017/05/11/the-study-of-aspnet-mvc)








