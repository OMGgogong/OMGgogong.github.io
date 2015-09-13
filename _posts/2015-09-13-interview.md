---
layout: post
title: "面试题-java部分"
date: 2015-09-13 15:32:44
image: '/assets/img/'
description: '面试 - 五组'
tags:
- java
- 面试 
categories:
- 面试
---

两天没写博客了 真是对不住大家 对不住

##1.Collection 和 Collections的区别？

Collection是集合类的上级接口，子接口主要有Set 和List。

Collections是针对集合类的一个帮助类，提供了操作集合的工具方法：一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。

##2.Collection 框架中实现比较要实现什么接口 ？

comparable/comparator 

##3.ArrayList 和 Vector 的区别？

这两个类都实现了 List 接口（List 接口继承了 Collection 接口） ，他们都是有序集合，即 

存储在这两个集合中的元素的位置都是有顺序的， 相当于一种动态的数组， 我们以后可以按 位置索引号取出某个元素， 

并且其中的数据是允许重复的，这是 HashSet 之类的集合的最 大不同处，HashSet 之类的集合不可以按索引号去检索其中的元素，也不允许有重复的元素

 （本来题目问的与 hashset 没有任何关系，但为了说清楚 ArrayList 与 Vector 的功能，我们使 用对比方式，更有利于说明问题） 。 

 接着才说 ArrayList 与 Vector 的区别，这主要包括两个方面： 

- 同步性： Vector 是线程安全的，也就是说是它的方法之间是线程同步的，而 ArrayList 是线程 序不安全的，它的方法之间是线程不同步的。如果只有一个线程会访问到集合，那最好是使 用 ArrayList，因为它不考虑线程安全，效率会高些；如果有多个线程会访问到集合，那最好 是使用 Vector，因为不需要我们自己再去考虑和编写线程安全的代码。 备注：对于 Vector&ArrayList、Hashtable&HashMap，要记住线程安全的问题
 - 数据增长： ArrayList 与 Vector 都有一个初始的容量大小，当存储进它们里面的元素的个数超过 了容量时，就需要增加 ArrayList 与 Vector 的存储空间，每次要增加存储空间时，不是只增 加一个存储单元， 而是增加多个存储单元， 每次增加的存储单元的个数在内存空间利用与程 序效率之间要取得一定的平衡。Vector 默认增长为原来两倍，而 ArrayList 的增长策略在文档 中没有明确规定（从源代码看到的是增长为原来的 1.5 倍） 。ArrayList 与 Vector 都可以设置 初始的空间大小，Vector 还可以设置增长的空间大小，而 ArrayList 没有提供设置增长空间的 方法。 总结：即 Vector 增长原来的一倍，ArrayList 增加原来的 0.5 倍。 

 ##4.HashMap 和 Hashtable 的区别？

 hashnap 线程不安全 key值可以为空 有containsValue和containsKey方法代替contains方法

 hashtable 线程安全的 key不允许为空 有contains方法

 ##5.List, Set, Map 是否继承自 Collection 接口? 

 List，Set 是，Map 不是 

 ##6.去掉一个 Vector 集合中重复的元素

 {% highlight java %}
    HashSet set = new HashSet(vector);
 {% endhighlight %}

##7.Set 里的元素是不能重复的，那么用什么方法来区分重复与否呢?是用==还是 equals()? 

它们有何区别? Set 里的元素是不能重复的，元素重复与否是使用 equals()方法进行判断的。equals()和 ==方法决定引用值是否指向同一对象 equals()

在类中被覆盖，为的是当两个分离的对象的内 容和类型相配的话，返回真值 

##8.你所知道的集合类都有哪些？主要方法？ 

最常用的集合类是 List 和 Map。 List 的具体实现包括 ArrayList 和 Vector，它们是可 变大小的列表，比较适合构建、存储和操作任何类型对象的元素列表。

Map 提供了一个更通用的元素存储方法。 Map 集合类用于存储元素对（称作"键"和"值"） 

##9.两个对象值相同(x.equals(y) == true)，但却可有不同的 hash code，这句话对不对? 
对。如果对象要保存在 HashSet 或 HashMap 中，它们的 equals 相等，那么，它们的 hashcode 值就必须相等。

 如果不是要保存在 HashSet 或 HashMap，则与 hashcode 没有什么关系了，这时候 hashcode 不等是可以的，例如 arrayList 存储的对象就不用实现 

 hashcode，当然，我们没有理由不实 现，通常都会去实现的。

 ##10.说出一些常用的类，包，接口，请各举 5 个?

要让人家感觉你对 java ee 开发很熟，所以，不能仅仅只列 core java 中的那些东西，要多列 你在做 ssh 项目中涉及的那些东西。

 类 BufferedReader BufferedWriter FileReader FileWirter String Integer java.util.Date，System，Class，List,HashMap

 常 用 的 包 ； java.lang java.io java.util java.sql,javax.servlet,org.apache.strtuts.action,org.hibernate 

 接口 List Map NodeList,Servlet,HttpServletRequest,compareable



