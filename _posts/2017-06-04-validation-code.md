---
title: "How to Implement Validation Code in MVC"
date: 2017-06-04
category: MVC
tags: [.Net, MVC, Validation Code]
comments: true

---

## Introduction

Validation code has become one of the essential functions of today's website. This article will show you how to implement a simple validation code function in .Net MVC website.  

## Code

**1.** In Models, Create the model of generating validate code. Two methods will be added in this model.  

    public class CreateValidateCode
    {
        /// <summary>
        /// Randomly generate validation code string
        /// </summary>
        /// <param name="codeCount"></param>
        /// <returns></returns>
        public string CreateRandomCode(int codeCount)
        {
            string allChar = "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z";
            string[] allCharArray = allChar.Split(',');
            string randomCode = "";
            int temp = -1;
            Random rand = new Random();
            for (int i = 0; i < codeCount; i++)
            {
                if (temp!=-1)
                {
                    rand = new Random(i * temp * ((int) DateTime.Now.Ticks));
                }
                int t = rand.Next(35);
                if (temp == t)
                {
                    return CreateRandomCode(codeCount);
                }
                temp = t;
                randomCode += allCharArray[t];
            }
            return randomCode;
        }
        /// <summary>
        /// Generate validation code image
        /// </summary>
        /// <param name="validateCode"></param>
        /// <returns></returns>
        public byte[] CreateValidateGraphic(string validateCode)
        {
            Bitmap image = new Bitmap((int) Math.Ceiling(validateCode.Length * 16.0), 27);
            Graphics g = Graphics.FromImage(image);
            try
            {
                Random random = new Random(); // Create random generator
                g.Clear(Color.White); // Clear the background color of the image
                for (int i = 0; i < 25; i++) // Draw the interference line
                {
                    int x1 = random.Next(image.Width);
                    int x2 = random.Next(image.Width);
                    int y1 = random.Next(image.Height);
                    int y2 = random.Next(image.Height);
                    g.DrawLine(new Pen(Color.Silver), x1, y1, x2, y2);
                }
                Font font = new Font("Arial", 13, (FontStyle.Bold | FontStyle.Italic));
                LinearGradientBrush brush = new LinearGradientBrush(new Rectangle(0, 0, image.Width, image.Height),
                    Color.Blue, Color.DarkRed, 1.2f, true);
                g.DrawString(validateCode, font, brush, 3, 2);
                for (int i = 0; i < 100; i++) // Draw the foreground interference point of the image
                {
                    int x = random.Next(image.Width);
                    int y = random.Next(image.Height);
                    image.SetPixel(x, y, Color.FromArgb(random.Next()));
                }

                g.DrawRectangle(new Pen(Color.Silver), 0, 0, image.Width - 1, image.Height - 1); // Draw margin line of the image
                // Save the image data
                MemoryStream stream = new MemoryStream();
                image.Save(stream, ImageFormat.Jpeg);
                return stream.ToArray();  // Output the image stream
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
                throw;
            }
            finally
            {
                g.Dispose();
                image.Dispose();
            }
        }
    }



**2.** In BLL CustomerManage, create a `Register(Customer objCustomer). In this part, no specific function will be implemented but simulated for instead.  

    public bool Register(Customer objCustomer)
        {

            return true;
        }



**3.** In Register.cshtml, add html code.  

    <div class="form-group">
        <label class="control-label" for="validationCode">Validation Code</label>
        @*<input class="form-control" type="text" id="Email" name="Email" value="@(Model != null ? Model.Email : "")" placeholder="Email">*@
        @Html.TextBox("validateCode")
        <img alt="验证码图片" id="ImgValidateCode" title ="看不清?点击换一个" src="@Url.Action("ValidateCode")" onclick="this.src=this.src+'?'"/>
        @Html.ValidationMessage("ValidateCode") 
    </div>


**4.** In CustomerController, create a new action  

    /// <summary>
    /// Add action method, call the method of generating validation code and save the validation code
    /// </summary>
    /// <returns></returns>
    public ActionResult ValidateCode()
    {
        //[1] Generate the string of random number with 5 digits validation code
        CreateValidateCode objCreateCode = new CreateValidateCode();
        // Generate validation code
        string code = objCreateCode.CreateRandomCode(5);
        //[2] Use TempData to save data( Session mechanism)
        TempData["ValidateCode"] = code; 
        //[3] Return validation code image
        return File(objCreateCode.CreateValidateGraphic(code), "image/Jpeg");
    }

When we refresh the page, the result is like below:  

<p align="center">
<img src="/images/post/20170604001.png" alt="validation code" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


When submitting the register form, we need add the validation code proofreading.  

    [HttpPost]
    public ActionResult Register(Customer objCustomer, string validateCode)
    {
        if (ModelState.IsValid)
        {
            // Validate the validation code of user's input
            if (string.Compare(TempData["ValidateCode"].ToString(), validateCode, true) != 0)
            {
                ModelState.AddModelError("ValidateCode", "验证码不正确，请重新输入");
                return View("Register");
            }


            CustomerManage objCustomerManage = new CustomerManage();
            if (!objCustomerManage.Register(objCustomer))
            {
                ModelState.AddModelError("doubleUser", "当前用户名已被使用，请重新输入!");
                return View("Register", objCustomer);
            }
            else
            {
                return Content("<script>alert('注册成功,请继续购物!');window.location='" + Url.Content("~/") + "'</script>");
            }


        }
        else
        {
            return View("Register", objCustomer);    
        }
        
    }


<p align="center">
<img src="/images/post/20170604002.png" alt="validation code" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>


***

You can use the code to implement the validation code in a real project. Hope that it will help you.  




