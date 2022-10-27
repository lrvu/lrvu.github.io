---
layout: post
title:  "Core Java（1）Java概述"
date:   2022-10-20 16:31:11 +0800
categories: Core Java
permalink: /:categories/1
---
1996 年 Java 第一次发布以来就备受关注，Java 为什么这么特别？

## Java 程序设计平台

Java 并不只是一种语言。在 Java 出现之前的那么多种语言都没有像 Java 这样引起那么多轰动。Java 是一个完整的平台，有一个庞大的库，其中包含了很多可复用的代码，以及一个提供诸如安全性，跨操作系统的可移植性以及自动垃圾收集等服务的执行环境。

在如今已经很难感受到当年开发环境为什么会对 Java 如此青睐，毕竟如今的 Java 略显臃肿，而且 Java 的库中包含的功能，C++，Python等其他语言的库里也有，但是在二十多年前可能编程语言生态还没现在这么丰富吧。

## Java “白皮书”的关键术语

Java 的设计者编写的“[白皮书](https://www.oracle.com/java/technologies/language-environment.html)”，用来解释设计的初衷以及完成的情况。这个白皮书用了11个关键术语组织摘要：

1. 简单性
2. 面向对象
3. 分布式
4. 健壮性
5. 安全性
6. 体系结构中立
7. 可移植性
8. 解释型
9. 高性能
10. 多线程
11. 多态性

对于这些特性简单了解即可。

## Java applet 与 Internet

在网页中运行的 Java 程序称为 applet 。它的初衷想法就是：用户从 Internet 下载 Java 字节码，并在自己的机器上的启用 Java 的 Web 浏览器上运行。但是因为服务器和浏览器的 Java 版本的冲突问题以及其他的安全问题，applet 很快就被 Adobe 的 Flash 技术淘汰。

## Java 发展简史

需要记住的 Java 基本常识：
- Java 是由 Patrick Naughton 和 James Gosling（书中对这位大佬的评价是：“一个全能的计算机奇才”） 带领的 Sun 公司在 1996 年发布的，
- Java 于 2009 年被 Oracle 收购。这时 Java 的版本是2006 年发布的 Java 6 。之后的版本都是 Oracle 公司发布的。
- 2014 年 Java 8 发布，这是在近20年中改变最大的一个版本。
## 关于 Java 的常见误解
略。