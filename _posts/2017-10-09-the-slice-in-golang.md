---
title: "The Slice in Go"
date: 2017-10-09
category: GoLang
tags: [Go language, go slice, usage, internals]
comments: true

---

## 介绍

slice是在Go语言中对一组相同类型数据操作的方法。有点类似其他语言中的数组，但是又有些不同。在Go语言中也有array类型，并且slice就是array的一个抽象，所以在理解slice之前，我们要先了解一下array。  

***

## 数组（Array）

我们都知道array类型需要指定长度和元素类型。比如说`[4]int`代表的是一个数组包含4个整形数。数组长度是固定的。如果数组未被初始化，则值为0。在内存中，此数组可以被理解为  

<p align="center">
<img src="/images/post/20171009001.png" alt="array" width="70%"  /><br/>
<center><h5><b>Figure 1</b></h5></center>
</p>

另外，拿`a := [4]int`来说，a是数组变量，代表整个数组，而不是数组第一个元素的指针。这个是与C语言的区别。大家要注意。  

在Go语言中，声明array的文法有两种，一种是显式给出数组长度，如`b := [2]string{"Penn", "Teller"}`，另外一种是让编译器自己去计算数组的长度`b := [...]string{"Penn", "Teller"}`，在这两种方法中，b的类型都是`[2]string`。  

***

## 切片（slice）

slice是无定长的，文法接近数组，只是没有长度的声明。同样，声明slice的文法也有两种，一种是传统方法，如`letters := []string{"a", "b", "c", "d"}`，或者用`make`语句。  

简单先介绍一下`make`语句，函数声明是`func make([]T, len, cap) []T`。我们可以像这样声明，  
    
    var s []byte
    s = make([]byte, 5, 5)
    // s == []byte{0, 0, 0, 0, 0}
    
这表示创建了一个长度为5，容量为5，类型为byte的slice。在这个示例中，如果声明改为`s := make([]byte, 5)`，则容量默认与长度一样的值。  

在slice中，我们同样也可以使用`len(s)`或者`cap(s)`去得到该slice的长度或者容量。对于长度和容量的关系，下面会详细讨论。一个slice的0值，我们用`nil`来表示。如果一个slice是nil，则对应的长度和容量的值为0。  

另外一个比较有趣的特性是slice可以被切片化(slicing)。在python中实际上也有同样的操作。举个例子，  

    b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
    // b[1:4] == []byte{'o', 'l', 'a'}, sharing the same storage as b
    
Go官方对切片化的解释是 Slicing is done by specifying a half-open range with two indices separated by a colon. 切片化表达式的起始位置和结束位置是可选的。默认的起始位置是0，结束位置是slice的长度。  

    // b[:2] == []byte{'g', 'o'}
    // b[2:] == []byte{'l', 'a', 'n', 'g'}
    // b[:] == b
    
再用另外一个例子展示一下，  

    package main
    import "fmt"
    func main(){
        b := make([]byte, 5)
        
        for i:=0; i<5; i++{
            b[i] = byte(i)
        }
        
        print_slice(b)
        
        m := b[2:]
        print_slice(m)
        
        m = b[2:4]
        print_slice(m)
    }
    
    func print_slice(b []byte){
        fmt.Printf("the slice is %v, the length is %v, the capacity is %v\n", b, len(b), cap(b))
    }

对应的结果是

<p align="center">
<img src="/images/post/20171009002.png" alt="slicing" width="70%"  /><br/>
<center><h5><b>Figure 2</b></h5></center>
</p>

要注意，`[a:b]` a是开包，b是闭包。  


***

## 切片内部（slice internals）

切片内部可以用下图来表示，  

<p align="center">
<img src="/images/post/20171009003.png" alt="slicing" width="70%"  /><br/>
<center><h5><b>Figure 3</b></h5></center>
</p>

如果我们用make去声明一个slice的话，比如` s := make([]byte, 5)`，可以用下图表示，  

<p align="center">
<img src="/images/post/20171009004.png" alt="slicing" width="70%"  /><br/>
<center><h5><b>Figure 4</b></h5></center>
</p>

左边其实就是s，切片描述符。再举个例子，`s = s[2:4]`  

<p align="center">
<img src="/images/post/20171009005.png" alt="slicing" width="70%"  /><br/>
<center><h5><b>Figure 5</b></h5></center>
</p>

在这个例子中，你会注意到，slice的长度为2，容量是3，也就是说，当slicing的时候，以起始位置开始到原slice的结束位置的长度是新切片的容量，而起始位置开始到结束位置的长度是新切片的长度。  

同样，在上面的例子中，我们可以知道，slicing不是对slice数据的复制，实际上，它是创建了一个指向原数组数据的切片描述符。也就是上图中左侧竖着的部分。 如  

    
    package main
    
    import "fmt"
    
    func main(){
        d := []byte{'r', 'o', 'a', 'd'}
        print_slice(d)
        
        e := d[2:] 
        print_slice(e)
        
        e[1] = 'm'
        print_slice(d)
    }
    
    func print_slice(b []byte){
    
        fmt.Printf("the slice is %c, the length is %v, the capacity is %v\n", b, len(b), cap(b))
    }

结果是  

<p align="center">
<img src="/images/post/20171009006.png" alt="slicing" width="70%"  /><br/>
<center><h5><b>Figure 6</b></h5></center>
</p>

所以对一个re-slice的修改实际上是修改原slice.  

***
## 增长切片（growing splices）

在这一节中主要介绍copy和append方法。Go语言中对增长切片的实现，实际上是重新创建一个更大容量的slice，并把原slice放到新的slice中。因此我们可以用copy和append对其进行实现。  
    
    t := make([]byte, len(s), (cap(s)+1)*2) // +1 in case cap(s) == 0
    for i := range s {
            t[i] = s[i]
    }
    s = t
    
最后的`s = t`，最好是回看一下上面的结构图，实际上是对s把之前的内存地址的指针指向了t指向的新地址。希望这么说你们会明白。这是比较土的一种实现，目前还没有用到copy和append方法。  
 
**copy方法**：`func copy(dst, src []T) int`, 对于这个方法，如果src的长度更小，那么它可以全部复制到dst中。如果dst的长度更小，那么只从src复制与dst等长的数据。我们可以像这样使用它  
      
    t := make([]byte, len(s), (cap(s)+1)*2)
    copy(t, s)
    s = t

**append方法**：`func append(s []T, x ...T) []T`, 在这个方法中，我们可以在原slice s后面加上一个T类型的值或者几个T类型的值，但是需要用“,”逗号分开。如  
    
    a := make([]int, 1)
    // a == []int{0}
    a = append(a, 1, 2, 3)
    // a == []int{0, 1, 2, 3}
    
有一种更简单的方法同时添加几个值，而不用一一列出来，那就是使用“...”在另一个同类型数组的描述符后面。如  
    
    a := []string{"John", "Paul"}
    b := []string{"George", "Ringo", "Pete"}
    a = append(a, b...) // equivalent to "append(a, b[0], b[1], b[2])"
    // a == []string{"John", "Paul", "George", "Ringo", "Pete"}


***

这篇文章大致把slice的使用和内部原理给阐述出来了，希望对你有帮助。  

