---
title: "The workflow of API authorization by Auth0"
date: 2017-11-10
category: Other
tags: [Auth0, workflow, API, authorization]
comments: true

---


## 背景介绍

最近终于找到了一家IT公司做 Part time 算是一个不错的开始(新西兰 Junior 的IT职位真的不好找)，公司用的都是对我来说比较新的技术，所以自己必须学习很多东西。隔了两年（学英语，留学）再次工作，感慨颇多。关于 Auth0 的认证，便是我遇到的第一个难点。 Auth0 关于认证的部分有些是基于 0Auth 2.0 标准的，但并不是全部，这也能看出Auth0的强大。但是对于我们的新项目，OAuth 2.0 标准只是一个小小的部分，是基础知识。虽然 Auth0 官网的文档很全，但是都没有直接把我想要的东西给我，粗略估计，自己是翻了20多个文档和几个 git 的项目，才最后把整个东西搞清楚。感觉自己好笨，但是弄清楚就真的很开心。  

我上面有多次提到 Auth0 和 OAuth 2.0标准，大家不要搞混啦。[Auth0](https://auth0.com/) 是一家公司，提供认证服务的。[OAuth 2.0](https://tools.ietf.org/html/rfc6749) 是一个认证标准，以前没有接触过，但是了解了之后感觉真的很好。现在最流行的单点登录也是基于其实现的。  

***

## 认证流程介绍

<span style="color:#ff0000">重点来啦！！！</span>  


<p align="center">
<img src="/images/post/20171110001.png" alt="authorization workflow" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

整个思路看起来挺容易的，但是在实现上，因为要配合着 Auth0 的标准，真的是探索了好久。一点点东拼西凑把整个东西给合起来了。上图我虽然分解成了10个步骤，但是主要是三个模块在实现上，分别是：  

<mark>
    <b>1-3</b> 前端实现<br/><br/>
    <b>4-9</b> 后端中间件实现<br/><br/>
    <b>10</b>  后端API功能实现<br/><br/>
</mark>

下面我详细介绍下每一步都做了哪些事情。  


1. 首先我们需要用户登录，对于用户的登录我们也是用 Auth0 去帮助认证，基于 OAuth 2.0 认证标准的`authorization_code`方式请求 Token。这个认证方法是 0Auth 2.0 提供的4种方法中最难的一种。具体实现可以参考 [OAuth 2.0 authorization code](https://tools.ietf.org/html/rfc6749#section-1.3.1) 认证标准。关于具体实现我会日后用 Golang 写个小例子。
  
2. 如果认证通过，Auth0 会返回给我们一个 Token。我们需要在本地保存此 Token。  
3. 登录成功后，当然是要请求 API。过去每次请求服务器资源，我们把用户名，密码存放在 cookie 中去请求。现在使用 OAuth 2.0 标准，我们每次只需要用 Token 去请求即可。即在 Header 中设置`authorization:bearer {token}`  


 
4. 服务器同样需要向 Auth0 验证自己的身份，但是因为是纯 Server-to-Server side 的操作，所以可以使用 OAuth 2.0 的另一种认证方法`client_credential`，在这里需要注意的是 Auth0 对此方法认证的要求与 OAuth 2.0 的标准还有一些不一样，其中要求添加`client_secret`和`audience`做为请求内容。关于这部分，我会单独一篇关于 Auth0 配置的文章，到时候我们就能弄清楚各个参数的用途。  

5. 如果认证通过，Auth0 会返回给我们一个 Token，但是需要注意的是，这个 Token 实际上是一个 [JWT](https://tools.ietf.org/html/rfc7519) 格式。关于 JWT 在 Auth0 中其实也有一些特殊地方的说明。官网有文章，大家可以自己搜搜，我之后也会写这部分。  

6. 关于验证 JWT 签名，这里就要涉及加密算法，是用`RS256`还是`HS256`，Auth0 官网推荐用`RS256`(非对称加密)，另外还涉及了 Auth0 的配置和 [JWK](https://tools.ietf.org/html/rfc7517#page-8) 和 [JWKS](https://tools.ietf.org/html/rfc7517#page-8)  

7. 当验证签名成功之后，说明我们请求的认证服务器是正确的那个，这样我们才能放心将步骤3得到的代表用户身份的 Token 发送给此认证服务器，这里我们用此 Token 向认证服务器请求用户信息（因为使用 Auth0 的话，它可以为我们管理用户信息，所以也相当于一个资源服务器）。  

8. 如果 Auth0 检测发送的 Token 没有问题，就会返回相应的用户信息。  

9. 当 Auth0 返回用户信息后，我们可以通过用户 ID 验证数据库中是否有相应的用户。  



10. 如果数据库中有对应的用户，我们就可以返回该客户端请求的 API 的功能。  

*** 

大家现在可以发现，虽然只有10个步骤，并且也很清晰，但是其实很一步都是一个很大的坑，要去了解很多东西，我在未来的文章中会更加详尽的把这个工作流相关的配置和实现分享出来。  

希望这篇文章能够对你有所帮助，互相学习。  

