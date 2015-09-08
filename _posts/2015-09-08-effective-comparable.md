---
layout: post
title: "高效编程-java部分"
date: 2015-09-08 15:32:44
image: '/assets/img/'
description: 'effectivejava - 五组'
tags:
- java
- effectivejava 
categories:
- effectivejava
---

##考虑实现comparable接口

上次我们讨论了重写object类里面的equals方法

这次我们还是要关注对于所有对象都通用的方法

首先为什么实现comparable接口呢 

一旦你将自己设计的类实现comparable接口，你的类就可以和许多

泛型算法以及依赖该接口的集合经行协作

比如我们可以用 

{% highlight java %}
Arrays.sort(a);
{% endhighlight %}

其中a为自己设计类的实例化对象 我们要对a里面的数据排序 我们要与Arrays集合交互协作

我们来看一下comparable接口的定义

{% highlight java %}

{% endhighlight %}

{% highlight java %}
public interface Comparable<T>{
    int comparaTo(T t);
}
{% endhighlight %}

上边看到的就是Comparable接口的源代码

实现这个接口就要实现int comparaTo （T t）方法

首先我们要明确他的通用约定 具体可以看javaSE来自Comparable的约定

- 实现者必须要满足 sgn（x.comparaTo(y)） == - sgn(y.comparaTo(x));
- 实现者必须要保证这个比较是可以传递的 (x.comparaTo(y) > 0 ) && (y.comparaTo(z) > 0) 暗示着 x.comparaTo(z) > 0
- x.comparaTo(y) == 0 意味着 x.equals(y) == 0  这一条并非必须要满足-----为什么呢? 因为这种情况不会造成灾难性的后果

比如new BigDecimal("1.0")与new BigDecimal("1.00")他们的用equals比较是不相等的 而 把他放在hash相关的集合中他们是相等的

明白了上边的通用约定 我们就可以实现comparable接口了

写接口的规则是

当比较整数型的基本数据的时候我们可以使用关系操作符<或者>

当比较float或者double类型的时候 我们需要用Double.compara()或Folat.compara()方法

当比较的是数组的时候 我们要比较每一个元素

比如我们写一个类 Myclass


{% highlight java %}

public class Myclass implements Comparable<Myclass>{
private int age;
private float price;
private double monkey;
@Override
public int compareTo(Myclass arg0) {
    if(age < arg0.age)
        return -1;
    
    if((Float.compare(price, arg0.price))>0?false:true){
        return -1;
    }
    if((Double.compare(monkey, arg0.monkey))>0?false:true){
        return -1;
    }
    
    return 0;
}

}

{% endhighlight %}





