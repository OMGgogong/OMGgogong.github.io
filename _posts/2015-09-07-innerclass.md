---
layout: post
title: "内部类-java基础"
date: 2015-09-03 15:32:44
image: '/assets/img/'
description: 'java基础 - 五组'
tags:
- java
- 内部类 
categories:
- java基础
---

##1.内部类

**内部类共分为4种**

- 静态内部类
- 成员内部类
- 局部内部类
- 匿名内部类

首先看一下**静态内部类**

不能和外部类有相同的名字。

被编译成一个完全独立的.class文件，名称为OuterClass$InnerClass.class的形式。

只可以访问外部类的静态成员和静态方法，包括了私有的静态成员和方法。
{% highlight java %}


{% endhighlight %}
下面是使用定义的方法
{% highlight java %}
class StaticInner {
	private static int a = 4;

	// 静态内部类
	public static class Inner {
		public void test() {
			// 静态内部类可以访问外部类的静态成员
			// 并且它只能访问静态的
			System.out.println(a);
		}

	}
}
public class StaticInnerClassTest {

	public static void main(String[] args) {
		StaticInner.Inner inner = new StaticInner.Inner();
		inner.test();
	}
}

{% endhighlight %}

接下来就是**成员内部类**

不能与外部类重名

去掉static关键字定义的类

可以访问外部类的任何属性

具体使用和定义方法如下：

{% highlight java %}

class MemberInner
{
    private int d = 1;
    private int a = 2;

    // 定义一个成员内部类
    public class Inner2
    {
        private int a = 8;

        public void doSomething()
        {
            // 直接访问外部类对象
            System.out.println(d);
            System.out.println(a);// 直接访问a，则访问的是内部类里的a

            // 如何访问到外部类里的a呢？
            System.out.println(MemberInner.this.a);
        }

    }

}

public class MemberInnerClassTest
{

    public static void main(String[] args)
    {

        // 创建成员内部类的对象
        // 需要先创建外部类的实例
        MemberInner.Inner2 inner = new MemberInner().new Inner2();

        inner.doSomething();
    }
}
{% endhighlight %}

接下来就是**局部内部类**

局部内部类定义在方法中，比方法的范围还小。是内部类中最少用到的一种类型。

像局部变量一样，不能被public, protected, private和static修饰。

只能访问方法中定义的final类型的局部变量。

使用和定义如下：

{% highlight java %}
class LocalInner
{
    int a = 1;

    public void doSomething()
    {
        int b = 2;
        final int c = 3;
        // 定义一个局部内部类
        class Inner3
        {
            public void test()
            {
                System.out.println("Hello World");
                System.out.println(a);

                // 不可以访问非final的局部变量
                // error: Cannot refer to a non-final variable b inside an inner
                // class defined in a different method
                // System.out.println(b);

                // 可以访问final变量
                System.out.println(c);
            }
        }

        // 创建局部内部类的实例并调用方法
        new Inner3().test();
    }
}

public class LocalInnerClassTest
{
    public static void main(String[] args)
    {
        // 创建外部类对象
        LocalInner inner = new LocalInner();
        // 调用外部类的方法
        inner.doSomething();
    }

}
{% endhighlight %}

最后一个是**匿名内部类**

匿名内部类就是没有名字的局部内部类，不使用关键字class, extends, implements, 没有构造方法。

匿名内部类隐式地继承了一个父类或者实现了一个接口。

匿名内部类使用得比较多，通常是作为一个方法参数。

使用和定义如下：
{% highlight java %}
public class AnonymouseInnerClass
{

    @SuppressWarnings("deprecation")
    public String getDate(Date date)
    {
        return date.toLocaleString();

    }

    public static void main(String[] args)
    {
        AnonymouseInnerClass test = new AnonymouseInnerClass();

        // 打印日期：
        String str = test.getDate(new Date());
        System.out.println(str);
        System.out.println("----------------");

        // 使用匿名内部类
        String str2 = test.getDate(new Date()
        {
        });// 使用了花括号，但是不填入内容，执行结果和上面的完全一致
            // 生成了一个继承了Date类的子类的对象
        System.out.println(str2);
        System.out.println("----------------");

        // 使用匿名内部类，并且重写父类中的方法
        String str3 = test.getDate(new Date()
        {

            // 重写父类中的方法
            @Override
            @Deprecated
            public String toLocaleString()
            {
                return "Hello: " + super.toLocaleString();
            }

        });

        System.out.println(str3);
    }
}
{% endhighlight %}
##2.什么是内部类？Static Nested Class 和 Inner Class 的不同。？
