---
layout: post
title: "高效编程-java部分"
date: 2015-09-10 15:32:44
image: '/assets/img/'
description: 'effectivejava - 五组'
tags:
- java
- effectivejava 
categories:
- effectivejava
---

##1.复合优先于继承

关于继承大家学的差不多了吧，继承的优点在于代码的重用，然而运用不当会产生意想不到的bug

孰不知 在java三大特性里面 继承打破了封装的特性

子类依赖于超类中的特定功能的实现细节，超类实现有可能随着发行版本不同而有所变化

如果真的发生了，子类有可能发生破坏。比如 子类里面有个新方法，这时候父类里面要增加一个和子类的相同方法名的

方法只不过返回类型不相同 ，那么子类是不能通过编译的。

继承的运用不好的地方还有下面的例子

假如我们要写一个类 看看自从他被创建以来添加了多少元素


{% highlight java %}

//这是超类
public class Sup {

    public void add() {
        System.out.println("this is a add");
    }

    public void addAll(int i) {
        System.out.println("sup   addAll");
        for (int j = 0; j < i; j++) {
            add();
        }
    }
}
//这是子类
public class Chr extends Sup {
    int i = 0;

    @Override
    public void add() {
        System.out.println("this is chr add");
        i++;
        super.add();
    }

    @Override
    public void addAll(int i) {
        this.i = i;
        super.addAll(i);
    }

    public int getI() {
        return i;
    }

}
//这是测试类
public class Test {
public static void main(String[] args) {
    Chr ch = new Chr();
    ch.addAll(3);
    System.out.println(ch.getI());
}
}

{% endhighlight %}
你先想想 这里会你打印什么呢？

.

.

.

.

{% highlight java %}
sup   addAll
this is chr add
this is a add
this is chr add
this is a add
this is chr add
this is a add
6
{% endhighlight %}
看到打印的是6而不是3会不会感到有些奇怪

请你结合这父类与子类之间的动态连接知识和打印的结果自己进行分析

调用流程分析大体如下

这里的父类有一个addAll方法调用了自己的add的方法 然而 在父类中确实就是这么调用

关键是在子类中的情况

在子类中调用自己的addAll方法之后会调用父类的addAll方法 然后父类addAll中有add方法 但不调用自己的add 这时候会运用动态连接机制 调用子类的add方法

我们发现父类这一现象

**父类方法里面调用了可以被子类覆盖的方法**

像这样的类在java类库里面不少 向HashSet类 等

你若继承他们会让你达不到想要的效果

这时候你若用复合就能解决

{% highlight java %}
public class Sup {

    public void add() {
        System.out.println("this is a add");
    }

    public void addAll(int i) {
        System.out.println("sup   addAll");
        for (int j = 0; j < i; j++) {
            add();
        }
    }
}




public class Chr {
private Sup su = new Sup();
private int i = 0;
public void add() {
    i++;
    su.add();
}

public void addAll(int i) {
    this.i=i;
    su.addAll(i);
}

public int getI() {
    return i;
}
}




public class Test {
public static void main(String[] args) {
    Chr ch = new Chr();
    ch.addAll(3);
    System.out.println(ch.getI());
}
}


{% endhighlight %}

你会发现打印结果为

{% highlight java %}
sup   addAll
this is a add
this is a add
this is a add
3
{% endhighlight %}

这就是复合

将类作为自己私有属性 然后调用其方法

到了这里你们终于看完了 只希望你们在考虑用继承的时候想到一句话

自己写的子类和父类之间 到底是不是“is-a”的关系  比如 猫和狗 是 动物

而在课堂上老师举得例子就有一个不恰当的

“ 全能空调可以继承制冷空调 ”这个就是继承使用不当

他们之间是“hava-a”的关系  全能空调有制冷的功能才对

所以向老师这个空调的例子应该用复合。

大家辛苦！
