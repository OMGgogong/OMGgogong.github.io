---
layout: post
title: "高效编程-java部分"
date: 2015-09-03 15:32:44
image: '/assets/img/'
description: 'java进阶 - 五组'
tags:
- java
- effectivejava 
categories:
- effectivejava
---
##1.考虑用静态工厂替代构造器
先看一段Boolean的源代码

{% highlight java %}
public static Boolean valueOf(boolean b){
    return b?Boolean.Ture:Boolean.False;
}
{% endhighlight %}

首先Boolean在java.lang包中被设计成
{% highlight java %}
public final class Boolean{
    public static fianl Boolean TRUE;
    //属性被final关键字修饰
    //并且连class都被final修饰
}
{% endhighlight %}
像这样的类被称为不可变类。一旦实例化就不会改变内部属性
这样的类使用静态工厂方法返回类本身的一个实例这样做的好处如下：
- 可以给他们随意起一个名字表明意图，而对应的构造器的方法只能用构造器的名字
- 不必在每次调用他自己的时候创建对象，看上边代码valueOf方法每次都会返回相同的静态属性,而静态属性属于类本身，和任 何一个实例都共享这个属性;多体现在这样的类中:不可变类,不可实例化类
- 在使用参数化类型的时候，他们使代码变得更加简介
看如下代码：
{% highlight java %}
Map<String,List<String>> m = new HashMap<String,List<String>>;
{% endhighlight %}
然而你可以么改进
{% highlight java %}
public static <K,V> HashMap<K,V> newInstance(){
   return new HashMap<K,V>();
}
//当你调用的时候可以这么写
Map<String,List<String>> = HashMap.newInstance();
{% endhighlight %}

##2.遇到多个构造器参数的时候考虑构建器
看下面一段代码:

{% highlight java %}
public class NutritionFacts{
    private final int servingSize;
    private final int fat;
    private final int sodium;
    private final int servings;
    public NutritionFacts(int servingSize){
        this(servingSize,0,0,0);
    }
    public NutritionFacts(int servingSize,int fat){
        this(servingSize,fat,0,0);
    }
    public NutritionFacts(int servingSize,int fat,int sodium){
        this(servingSize,fat,sodium,0);
    }

    public NutritionFacts(int servingSize,int fat,int sodium,int servings){
    this.servingSize = servingSize;
    this.fat = fat;
    this.sodium = sodium;
    this.servings = servings;

}
}
{% endhighlight %}
上述代码当你想使用第二个构造器的时候你会发现sodium，servings我们本来没用到我们还不得不为他们传递值，另外当有更多的参数的时候客户端代码会很难找出调用那一个构造器，阅读起来也挺麻烦的
那么请看下面的代码:
{% highlight java %}
public class NutritionFacts{
    private final int servingSize;
    private final int fat;
    private final int sodium;
    private final int servings;
    ...
    //
    public static class Builder{
    
    private final int servingSize = 0;
    private final int fat = 0;
    private final int sodium = 0;
    private final int servings = 0;
    public Builder(){

    }
    public Builder servingSize(int val){
    servingSize = val;
    return this;
    }
    public Builder fat(int val){
    fat = val;
    return this;
    }
     public Builder sodium(int val){
    sodium = val;
    return this;
    }
    public Builder servings(int val){
    servings = val;
    return this;
    }
    public NutritionFacts build(){
        return new NutritionFacts(this);
    }
    }

    //
    public NutritionFacts(Builder b){
    this.servingSize = b.servingSize;
    this.fat = b.fat;
    this.sodium = b.sodium;
    this.servings = b.servings;

}
}
//调用的时候这样子：
NutritionFacts n = new NutritionFacts.Builder().sodium(5).fat(4).build();
//把想初始化的属性用.连接起来，最终用build（）建立；
{% endhighlight %}
##3.用私有的构造器强化单例类
单例类这么写
{% highlight java %}
public class SingleTon{
    public static final singleTon = new SingleTon();
    private SingleTon(){
    if(singleTon != null){//这样做的目的是防止通过反射进行攻击
    new AssertionError();
    }
    }
    public static SingleTon getInstance(){
    return singleTon;
    }
    private Object readResolve(){//此方法的目的是防止序列化创建多个对象
    return singleTon;
    }
}
{% endhighlight %}
不要以为private方法是不会被外界访问到的 java的反射机制还有序列化是可以访问到的
所以要在构造器里面写一个判断还有多出一个readResolve（）方法保证这个类是真正的单例。
当##JDK 1.5 版本##出现之后 他支持类枚举类型 我们的单例可以这么写
{% highlight java %}
public enum SingleTon{
    single;
}
{% endhighlight %}
就这么简单不用写private构造器不用担心序列化和反射机制
