---
layout: post
title:  "Core Java（2）Java环境"
date:   2022-10-20 17:08:35 +0800
categories: Core Java
permalink: /:categories/2
---
Java 术语表

| 术语名|缩写|解释|
| :---:|:--:|:--:|
|Java Development Kit（Java 开发工具包）|JDK|编写Java 程序的程序员使用的软件|
|Java Runtime Environment（Java 运行时环境）|JRE|运行 Java 程序的用户使用的软件|
|Server JRE（服务器JRE）|——|在服务器上运行 Java 程序的软件|
|Standard Edition（标准版）|SE|用于桌面或简单服务器应用的 Java 平台|
|Enterprise Edition（企业版）|EE|用于复杂服务器应用的Java平台|
|Micro Edition（微型版）|ME|用于小型设备的 Java 平台|
|Java FX|——|用于图形化用户界面的一个备选工具包，在Java 11 之前的某些 Java SE 发布版本中提供|
|OpenJDK|——|Java SE 的一个免费开源实现|
|Java 2|J2|一个过时的术语，用于描述1998～2006 年之间的 Java 版本|
|Software Development Kit（软件开发工具包）|SDK|一个过时的术语，用于描述 1998～2006 年之间的 JDK|
|Update|u|Oracle 公司的术语，表示 Java 8 之前的 bug 修正版本|
|NetBeans|——|Oracle 公司的集成开发环境|


## 安装 Java 开发工具包（JDK)

由术语表可知，JDK 就是 Java Development Kit 的缩写，它提供了大量用于开发的 API。值得注意的是 JRE 即 Java 运行时环境只包含了虚拟机，用来运行 Java 程序，只是专门为不需要编译器的用户提供的。

在 Java 9 之后，Oracle JDK 不再提供 32 位了，要想在 32 位机器上运行 JDK 就只能使用 Java 9 之前的版本。

要想安装 JDK，可以到 [Oracle官方下载](https://www.oracle.com/java/technologies/downloads/)对应的系统和版本。也可以通过其他方法下载，例如在 Linux 系统中使用 **yum**, **apt** 等包管理程序下载。我的系统是 Ubuntu 22.04， 使用的就是 **apt** 下载。
```shell
sudo apt install default-jdk # 默认下载OpenJDK 11
```
使用命令行下载就是图方便，使用 apt 下载会把 java 源文件下载到 `/usr/lib/jvm` 目录下，并且自动配置好环境变量，将 java 运行变量加载到`/usr/bin`目录下，而不用自己再配置环境变量。

如果不是包软件下载，而是从官网下载压缩包，那么就需要配置 Java 环境变量才能使用，在 Linux 环境下配置环境变量只需要在`~/.bashrc`或`~/.bash_profile`文件中的导入 JAVA_HOME(jdk文件所在的路径)就行了：
```shell
export JAVA_HOME={your_jdk_path} # JAVA_HOME 变量为你jdk文件所在的路径
export PATH=$JAVA_HOME/bin:$PATH
```
在 Windows 系统中，需要通过设置中搜索“环境”，找到“环境变量”对话框，修改 `Path` 变量即可。网上关于 Java 环境变量配置的教程多如牛毛，这里就不复述了。

### 安装库源文件和文档

类库源文件在 JDK 中以压缩文件 `lib/src.zip` 的形式发布，将其解压缩后才能访问源代码。使用 `apt` 等包软件时则需要单独下载源码。
```shell
sudo apt install openjdk-11-source # 下载 OpenJDK 源文件
```
确认 `lib/src.zip` 存在后就可以通过解压得到库源文件和代码：
```shell
mkdir ~/java-src
cd ~/java-src
jar svf $JAVA_HOME/lib/src.zip
``` 
Java 文档包含在一个独立与 JDK 的压缩文件中，可以直接从 [Oracle 官方网站下载](https://www.oracle.com/java/technologies/javase-jdk11-doc-downloads.html)。也可以通过 **apt** 命令行下载：
```shell
sudo apt install openjdk-11-doc
```
通过 **apt** 下载的的文档在 `/usr/share/doc/openjdk-11-doc/` 目录下。通过浏览器打开 `index.html` 就可以查看 Java 的离线文档了。
```shell
firefox /usr/share/doc/openjdk-11-doc/index.html # 使用火狐浏览器打开 Java API 文档网页
```

## 使用命令行工具

Java 源文件的后缀是 ‘.java', 而 Java 的执行文件的后缀是 '.class'。和 C 语言不同，Java 的可执行文件不是二进制文件，不能直接执行，而是要通过 `java` 命令来启动 Java 虚拟机，然后由虚拟机执行。例如：

创建 Welcome.java 文件，在命令行输入：
```shell
touch Welcome.java # 创建 Welcome.java 文件
```
然后修改 Welcome.java ，写入以下代码：
```java
/**
 * 一个简单的示例程序，这个程序输出一行问候信息。
 */
public class Welcome{
    public static void main(String[] args){
        String greeting = "Welcom to Core Java!";
        System.out.println(greeting);
        for(int i = 0; i < greeting.length(); i++)
            System.out.print("=");
        System.out.println();
    }
}
```
退出并保存，在命令行输入命令：
```shell
javac Welcome.java # 编译 Welcome.java
```
使用 ls 命令，你会发现编译生成了一个 Welcome.class 文件，这时候输入命令：
```shell
java Welcome # 运行 Welcome 类
```
就会执行代码中的内容，在终端输出
```
 Welcom to Core Java!
 ====================
```

## 使用集成开发环境

Java 有很多集成开发环境（Integrated development environment，IDE），例如 Eclipse, IntelliJ IDEA 和 NetBeans。我使用的是 VSCode(Visual Studio Code) 搭配 Java 插件，使用 VSCode 的主要原因是因为它比起其他几个 IDE 要更加轻量化，而且支持不同的语言，缺点是没有其他几个 IDE 对 Java 的支持全面（毕竟人家就是集成 Java 的）。使用 IDE 的好处是不言而喻的，毕竟不得已没人想要用文本或者 Vim 来写代码（虽然确实有不少大佬用 Vim 写代码，Vim 也有许多插件可以使用，用顺手后效率非常高）。

## JShell

Jshell 是 Java 9 之后引进的方法，类似于 Python 的 shell 解释器，Python 在命令行输入 `python3` 后就会进入到一个 shell 里，在里面可以输入 Python 语句并执行，这是解释型语言的特性与方便之处，很明显 Jshell 也是这样一个解释器。

启动 jshell 后会输出一段问候和版本信息，

```shell
jshell # 启动 jshell 工具
 
 |  欢迎使用 JShell -- 版本 11.0.16
 |  要大致了解该版本, 请键入: /help intro

 jshell> 

输入 `/help` 可以查看帮助信息：
 jshell> /help
   键入 Java 语言表达式, 语句或声明。
   或者键入以下命令之一:
   /list [<名称或 id>|-all|-start]
        列出您键入的源
   /edit <名称或 id>
        编辑源条目
   /drop <名称或 id>
        删除源条目
   /save [-all|-history|-start] <文件>
        将片段源保存到文件
   /open <file>
        打开文件作为源输入
   /vars [<名称或 id>|-all|-start]
        列出已声明变量及其值
   /methods [<名称或 id>|-all|-start]
        列出已声明方法及其签名
   /types [<名称或 id>|-all|-start]
        列出类型声明
   /imports 
        列出导入的项
   /exit [<integer-expression-snippet>]
        退出 jshell 工具
   /env [-class-path <路径>] [-module-path <路径>] [-add-modules <模块>] ...
        查看或更改评估上下文
   /reset [-class-path <路径>] [-module-path <路径>] [-add-modules <模块>]...
        重置 jshell 工具
   /reload [-restore] [-quiet] [-class-path <路径>] [-module-path <路径>]...
        重置和重放相关历史记录 -- 当前历史记录或上一个历史记录 (-restore)
   /history [-all]
        您键入的内容的历史记录
   /help [<command>|<subject>]
        获取有关使用 jshell 工具的信息
   /set editor|start|feedback|mode|prompt|truncation|format ...
        设置配置信息
   /? [<command>|<subject>]
        获取有关使用 jshell 工具的信息
   /! 
        重新运行上一个片段 -- 请参阅 /help rerun
   /<id> 
        按 ID 或 ID 范围重新运行片段 -- 参见 /help rerun
   /-<n> 
        重新运行以前的第 n 个片段 -- 请参阅 /help rerun
   
   有关详细信息, 请键入 '/help', 后跟
   命令或主题的名称。
   例如 '/help /list' 或 '/help intro'。主题:
   
   intro
        jshell 工具的简介
   keys
        a description of readline-like input editing
   id
        片段 ID 以及如何使用它们的说明
   shortcuts
        片段和命令输入提示, 信息访问以及
        自动代码生成的按键说明
   context
        /env /reload 和 /reset 的评估上下文选项的说明
   rerun
        重新评估以前输入片段的方法的说明
```
在学习一些简单例子而不想写 public static void main 时，就可以使用 Jshell 来快速检验代码结果，而不用再建立文件编译运行。
