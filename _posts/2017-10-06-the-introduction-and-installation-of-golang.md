---
title: "The Introduction and Installation of Go Language"
date: 2017-10-06
category: GoLang
tags: [Go language, Introduction]
comments: true

---

## Go Language

**Go** is an open source programming language that makes it easy to build simple, reliable, and efficient software. Currently, Go 1.9 is already released. The [official website](https://golang.org/#) includes tons of sources, such as documents, packages and projects. If you are interested in it, just go to have a look immediately.  

## History

**Go**(often called golang) is created by Google in 2007. It is compiled, statically typed language in the tradition of *Algol* and *C*, with garbage collection, limited structural typing, memory safety features and concurrent programming features added. The compiler and other language tools originally developed by Google are all free and open source. Referring [here](https://en.wikipedia.org/wiki/Go_(programming_language)) to know more.  

## Installation

In fact, this part is also showed in the official website. However, even though you follow the tips step one step, sometimes you also meet some problems that you have to spend some time to solve. Thus, I write this part to help you guys to install it smoothly.  

The download url is https://golang.org/dl/ . My operating system is Windows, so I download the mirror file *go1.9.1.windows-amd64.msi* which is only 90MB.  

After you download the msi file. Just install it immediately. By default, the file will be put in <mark>c:\Go</mark>. At the same time, you will see the <mark>c:Go\bin</mark> directory has already been put into the <mark>PATH environment variable</mark>.  

## Test the installation

First of all, we need to create our own workspace and build a simple program, as follow.  

By default, we create the workspace directory in <mark>%USERPROFILE%\go</mark>. If you are not good at using command line to operate, you also can input <mark>%USERPROFILE%</mark> in your <mark>windows explorer</mark> and then you will be lead to the directory. And then you can create a new folder called go here. The reason why we create our workspace here is when we install the msi file, the <mark>%USERPROFILE%\go</mark> was add into the <mark>GOPATH environment variable</mark>, but if you would like to use a different directory, you can set the <mark>GOPATH environment variable</mark> as your will.  

Next, we create directory <mark>src</mark> inside the <mark>%USERPROFILE%\go</mark> and then create directory <mark>hello</mark> inside the <mark>src</mark>.  

In the directory <mark>%USERPROFILE%\go\src\hello</mark>, we need to create a file named <mark>hello.go</mark> that looks like:  
    
    package main
    
    import "fmt"
    
    func main() {
        fmt.Printf("hello, world\n")
    }

Then in same directory, in command line environment, we input some instructs like below:  



<p align="center">
<img src="/images/post/20171006001.png" alt="go build" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>


If you get the same result, and the <mark>hello, world</mark> are printed out on the screen. That means you made it.  

In previous code, if you type it by yourself, you should mind one part that the `P` in the `Printf` is capitalized, otherwise you can not pass the compile. As in golang, it is case sensitive.  

***

After installing golang environment successfully, we can start to learn it.  
