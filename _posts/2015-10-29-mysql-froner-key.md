---
layout: post
title: "mysql-外键建与不建的讨论"
date: 2015-10-29 15:32:44
image: '/assets/img/'
description: 'mysql - 五组'
tags:
- mysql
- mysql 
categories:
- mysql数据库
---

从效率上来讲：
建外键的缺点：用户在执行插入操作时会去检索主表。如果插入的数据在主表中找不到，就会抛异常。
这个异常用户是看不懂的。所以程序员要捕捉这个异常。但用Exception来捕捉，就有点太笼统了。
所以最好的办法还是要通过程序员自己去查主表。这也就意味着要执行两次查询工作。
优点：在参差不齐的程序员当中有时会忘记在程序当中作出判断，从而导致不合法的数据录入到数据库中去。
而在建外键的情况下，这种不合法的数据就杜绝了。

从复杂度来讲：
建外键的缺点：对于程序员来讲。在众多建立关系的表中去理解出一条思路是非常困难的。相对于不建外键来说。
显得有点复杂。感觉不知从何下手解决问题。在一个考虑在程序当中的实现问题。假如有三张表建立了外键关系。
那么我要删第一张表中的某条记录时。也许你就会要去检索后面两张表的数据。如果在后面两张表中有这个数据。
他就会抛异常。所以先要删除后面两张表的数据。这种牵一动百的事情对于程序员来说：这是一种可耻的行为。
假如他们之间没有建立关系。也许也将要删除后面两张表的数据。但他是直接删除。而没有再去检索数据。

只有两种情况是不需要建立外建的：
1、两表之间没有关联关系，或者两表之间的关联关系无法用外建进行约束。
2、两表之间虽然有关联关系而且也能符合外建约束要求，但由于只是作为数据采集的占存表，并没有参与进一步的业务逻辑处理。
除了以上两条都应该建立外建约束，为什么，因为查错成本要远远高于开发成本。

有一个建议:
当系统处于开发阶段时,对速度要求不是很高,但业务逻辑又不十分完善时,先建.
当系统运行良好,但访问量增大,速度成瓶颈时,考虑去掉外键关系以加愉运行速度.