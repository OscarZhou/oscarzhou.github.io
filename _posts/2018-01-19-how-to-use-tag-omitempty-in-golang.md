---
title: "如何使用标签omitempty"
date: 2018-01-19
category: GoLang
tags: [Go language, tag, omitempty]
comments: true

---


## 背景

前几天准备写一个API的`PATCH`方法，就是修改用户的信息，但是问题是，用户的信息是由第三方维护，第三方给出接口，我去调用。但是第三方有一些规则，比如说，修改email的时候，需要加上另外一个xx值（xx值也是由第三方维护，跟用户信息接口存在一起）。并且发送请求的`body`部分不能存在多余的字段。比如改email，就发一个`{email:xxxx@xx.com}`就可以了，不要有其他的。所以，我就要判断每次哪个属性被修改，然后生成相应的json。  

如何一劳永逸的解决自动生成新的json，并且没有其他多余属性呢？最古老的方法当然是遍历绑定后的实体，然后取出那些值为`""`,`false` 或者`nil`的字段，然后剩余的组成新的字符串，再编码成Json。不是不行，但是在 Golang的`tag` 中有一个属性叫`omitempty`，正好可以用在这个例子当中。


*** 


## 代码实现

下面让我们看看具体的代码实现：  

    package main
    
    import (
        "encoding/json"
        "fmt"
    )
    
    type User struct {
        Name          string `json:",omitempty"`
        PhoneNumber   string `json:",omitempty"`
        PhoneVerified bool   `json:",omitempty"`
    }
    
    func main() {
        var user1 = User{"jack", "", false}
    
        body, _ := json.Marshal(user1)
        fmt.Println(string(body))
    }



这里写了一个简单的小例子。Playground: [https://play.golang.org/p/BFjmpaPHyor](https://play.golang.org/p/BFjmpaPHyor    

我们可以看到上面代码的结果会是`{"Name":"jack"}`。也就是说，当对象里的属性值为 `""`,`false` 或者`nil`这些时，并且对应的标签里有`omitempty`，当调用`json.Marshal`时，生成的json就会把无值或空值的内容去掉。  


<span style="color:#ff0000">注意！！！</span>   
是不是很简单？在这里再说一个小坑，那就是一定要注意在tag`json:",omitempty"`中，`','`一定要和`'omitempty'`紧挨着，要不`omitempty`不会起任何作用！  

下面的截图是官方对该标签的解释，大家可以看看。[点这里](https://golang.org/pkg/encoding/json/)

<p align="center">
<img src="/images/post/20180119001.png" alt="json omitempty" width="80%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>



***  



希望这篇文章能够对你有所帮助，互相学习。  

