---
title: "关于用Golang实现OAuth的认证的经验总结"
date: 2017-10-31
category: GoLang
tags: [Go language, OAuth]
comments: true

---

## 背景介绍

很长一段时间没有写东西了，最近在学习Golang，在这个过程中随着知识的一点点积累，在喜悦的同时，也觉得以前自己的web开发学的太粗糙了，只是会写些代码，但是并不知道为什么要那么写，就算知道下一步要怎么做，但是也不了解深层含意。比如说http协议到底是怎么运作的，这也导致了在用Golang实现小demo的过程中，自己多次走进误区。这两天我遇到两个大问题（困扰我挺长时间的问题，但是其实很简单，只是自己学艺不精），很开心，现在都已经解决了，也弄清楚是怎么回事了。所以写这篇文章总结回顾一下。  

*** 


## 问题： 实现OAuth认证

为了实现 OAuth认证，其实中间遇到些其他的问题，我就按照时间顺序一一道来。  

**思路** 当我想实现这个功能的时候，我会想想它在真实操作时会是什么样子，比如点击什么按钮进入什么样的页面等等。如下图所示，我点击*Sign in with google*之后，路由会将我的请求转到对应的处理方法。在处理方法中，我通过一个`oauth2.Config`的结构体（这个结构体是包含在`golang.org/x/oauth2`这个包中，后面大部分的实现也都是靠它），去把数据填充，然后组织好请求授权的url，这个url具体是什么我一会再讲。组好url后，为其做一个重定向，会将浏览器页面导向google的登录页面，至此，主要的第一步就完成了。这个过程也是我今天想讲的。后面的还没开始写，但是有了这一步实现的经验，后面应该没问题的。  

<p align="center">
<img src="/images/post/20171031001.png" alt="login" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


下面是我在这么一个小小的简单实现中遇到的几个问题：  
1. 我选择用ajax去做登录请求（从一开始我就把问题复杂化了，所以后面大部分的问题都是因为这个决定产生）  
2. 用ajax请求后，设置了返回的数据类型为json，即`"dataType":"json"`  
3. 遇到跨域请求问题，思考去用实现了跨域请求的中间件去解决这个问题  
4. 无法正确解析json格式的文件  
5. 无法解析json文件中的子结构  

下面我一一说解决方法：  
1. 因为最近在学Restful API，所以无论什么请求，都会去联想要用哪种http方法，要组header之类的。但是我却忘记了最根本的东西，那就是`<a>`标签存在的意义，就是为了让我们能实现页面跳转，那如何实现页面跳转呢？当然是要向服务器请求该url，然后服务器给你返回个页面。像我上面所说的，我做的事情就是点击*Sign in with google*，然后路由将我的请求转到对应的处理方法。这么简单的一个操作，既然没有数据的传输，根本不需要用ajax去实现服务器的请求。就是简单的将对应的url写在`<a>`标签当中就好。  
    
        <a id="SignInWithGoogle" href="http://localhost:9090/login?authServer=google">
            <img src="../static/img/oauth/btn_google_signin_dark_normal_web.png">
        </a>

    在成功进入后台的处理方法，收集完相关OAuth认证需要的信息后，我利用了重定向，这个时候，给人的感觉就是，当点击*Sign in with google*，直接跳到的是google的登录表单。但是实际上，是先通过我们的服务器，是我们重定向到google的。然而这个用户并察觉不到。  

        redirect := conf.AuthCodeURL(state)
        c.Redirect(http.StatusFound, redirect)
    
2. 如果我选择用了ajax，也不是任何情况都会遇到跨域请求问题的。当你设置`"content-type":"application/json"`，会被视为复杂类型，它才会有一个预检（preflight),会先向对应的url发送一个`OPTION`请求，如果这个时候你没有做相应的处理，那么就不会再进行下一步你真正想要做的`POST`或者`GET`请求了。这块也反映出我没有把整个OAuth认证的过程理解好。想也不想的就写了json上去。然后整出个跨域请求问题，还得想办法写`OPTION`对应的路由方法。真是蠢！这里如果改成`"dataType":"text"`,并且在golang中，用`c.string(code, string)`方法去回应，而不是用`c.JSON(code, json)`，ajax处就会从success方法返回。（c是gin插件的一个上下文）  

3. 话说这个问题我最终其实没有按我理想的用中间件的方法实现，代替的是我写了`OPTION`对应的处理方法，在其中加上了

        c.Writer.Header().Set("Access-Control-Allow-Origin", "*")
        c.Writer.Header().Set("Access-Control-Allow-Headers", "Content-Type")
        c.Writer.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT")
        c.Next()
  
    这种方法虽然可以成功通过`OPTION`和`GET`的请求。但是在`GET`对应方法中进行重定向的话，还是会出现跨域请求问题，然而此时，你没法在响应中加上上面的头。因为下一步的表单已经不是你控制得了的了。是由google接管了。所以这一步大家就是简单知道一下就行，然后就可以过啦。  
    
4. 这个问题是因为想实现多个不同的第三方认证，所以在这种情况下，只要代码写一套就够了，变的只是不同的第三方，对应的几个认证参数是不同的。上一次做json相关的东西真的记不得是多少年前了，所以很多东西都忘记了。另外，golang对json的解析，还有一些规定，这些都是不知道的，所以在这块也挣扎了一下下。我的json文件大概是  

        {  
            "Creds":[  
               {  
                  "ThirdParty":"google",
                  "Cid":"xxxx",
                  "Csecret":"xxxx",
                  "RedirectURL":"http://127.0.0.1:9090/auth",
                  "Scopes":[  
                    "https://www.googleapis.com/auth/userinfo.email"
                  ],
                  "AuthURL":"https://accounts.google.com/o/oauth2/auth",
                  "TokenURL":"https://accounts.google.com/o/oauth2/token"
               },
               {  
                  "ThirdParty":"auth0",
                  "Cid":"xxxx",
                  "Csecret":"xxxx",
                  "RedirectURL":"http://localhost:9090",
                  "Scopes":[
                    "openid",
                    "profile"
                  ],
                  "AuthURL":"xxxxxxx",
                  "TokenURL":"xxxxxxxx"
               }
            ]
         }
  
    在gin插件中，有一个方法是`func Unmarshal(data []byte, v interface{}) error`，第一个参数的获得比较容易，可以通过下面这段代码获得。  
            
            fileName := "\\conf\\creds.json" //json文件所在目录
            pwd, _ := os.Getwd() //获取所在项目的根目录
            file, err := ioutil.ReadFile(pwd + fileName) //获取json文件内容

    第二个参数就相对复杂一些，其实也不复杂，就是需要知道几个点。在这个例子中，我设计的结构体如下：  
    
            type Creds struct{
                Creds []Credential 
            }
            
            type Credential struct {
                ThirdParty	string
                Cid			string `json:"cid"`
                Csecret		string `json:"csecret"`
                RedirectURL	string
                Scopes		[]string
                AuthURL		string
                TokenURL	string
            }
            
    首先说一下需要注意的点，其一，json文件中的key要么与结构体字段同名，要么与tag同名。只有这样，在使用`func Unmarshal(data []byte, v interface{}) error`时，才能被一一对应到结构体的变量中去。其二，结构体中的变量一定要是可导出状态，也就是说需要首字母大写。也只有这样才能被上面的方法解析。所以最后的实现大概就是  
    
            creds := &Creds{}
            err = json.Unmarshal(file, creds)
            if err != nil {
                fmt.Println(err)
            }
            for _, v := range creds.Creds{
                if v.ThirdParty == authServer {
                    return &oauth2.Config{
                        ClientID:		v.Cid,
                        ClientSecret:	v.Csecret,
                        RedirectURL:	v.RedirectURL,
                        Scopes:			v.Scopes,
                        Endpoint: oauth2.Endpoint{
                            AuthURL:	v.AuthURL,
                            TokenURL:	v.TokenURL,
                        },
                    }
                }
            }
            
    
5. 这个问题遇到就是因为将子结构体里的变量小写了，这个上面其实已经说了。

*** 

## 关于OAuth认证的Authorization Code方法认证的第一步，请求授权码的解释

上面我们可以看到json文件中我写的几个参数，除了`ThirdParty`这个是我为了区分不同的第三方认证使用，其他的都是我根据OAuth的认证原理抽取出来的。在OAuth认证码认证的过程中，第一步就是我们将几个参数作为url的参数附加在资源服务器的url上。这几个分别为：  

1. `response_type` 这个值一直是`code`  
2. `client_id`  这个就是Json文件中的cid  
3. `redirect_uri`  这个是重定向url。其实是当第三方认证成功之后，它需要把相应的信息返回给我们，这个url是注册在我们自己的路由当中的，所以加有相应的处理方法  
4. `scope` 这个是可选的  
5. `state` 这个也是可选的，这个如果用的话，可以是个随机数。  
        
        {  
            "ThirdParty":"google",
            "Cid":"xxxx",
            "Csecret":"xxxx",
            "RedirectURL":"http://127.0.0.1:9090/auth",
            "Scopes":[  
            "https://www.googleapis.com/auth/userinfo.email"
            ],
            "AuthURL":"https://accounts.google.com/o/oauth2/auth",
        "TokenURL":"https://accounts.google.com/o/oauth2/token"
        }
        
以我上面的google的json数据为例的话，最后组成的url就是  

            https://accounts.google.com/o/oauth2/auth?client_id=xxxxx&
            redirect_uri=http%3A%2F%2F127.0.0.1%3A9090%2Fauth&response_type=code
            &scope=https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email&
            state=77VXenOTdA%2BRdiDKGEH51xsr2wtfDGCIAGXJNy4%2Fu2Q%3D  

如果大家想看完整的关于OAuth认证的东西，可以看看阮神的解释，真的真的很厉害。[点这里](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html).  


***


我只想说，遇到问题其实是对自己过去学习的一个反思，遇到问题不可怕，解决了就变成经验了，所以现在还是非常开心的。  

