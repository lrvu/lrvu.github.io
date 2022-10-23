---
layout: post
title:  "Core Java（3）Java的基本结构"
categories: Core Java
permalink: /:categories/3
---
- [一个简单的 Java 应用程序](#一个简单的-java-应用程序)
- [注释](#注释)
- [数据类型](#数据类型)
  - [整型](#整型)
  - [浮点类型](#浮点类型)
  - [char 类型](#char-类型)
  - [Unicode 和 char 类型](#unicode-和-char-类型)
- [变量与常量](#变量与常量)
  - [变量](#变量)
  - [常量](#常量)
  - [枚举类](#枚举类)
- [运算符](#运算符)
- [字符串](#字符串)
  - [字串](#字串)
  - [拼接](#拼接)
  - [不可变字符串](#不可变字符串)
  - [检测字符串是否相等](#检测字符串是否相等)
  - [空串与 Null 串](#空串与-null-串)
  - [码点与代码单元](#码点与代码单元)
  - [String API](#string-api)
  - [构建字符串](#构建字符串)
- [输入输出](#输入输出)
  - [读取输入](#读取输入)
  - [格式化输出](#格式化输出)
  - [文件输入与输出](#文件输入与输出)
- [控制流](#控制流)
  - [块作用域](#块作用域)
  - [条件语句](#条件语句)
  - [循环](#循环)
  - [确定循环](#确定循环)
  - [多重选择: switch语句](#多重选择-switch语句)
  - [中断控制流程](#中断控制流程)
- [大数](#大数)
- [数组](#数组)
  - [数组遍历](#数组遍历)
  - [数组拷贝](#数组拷贝)
  - [命令行参数](#命令行参数)
  - [数组排序](#数组排序)
  - [多维数组](#多维数组)
  - [不规则数组](#不规则数组)


本章内容包含了 Java 的基本结构, 程序设计的基本概念(如数据类型, 分支以及循环)在 Java 中的实现方式.

## 一个简单的 Java 应用程序

下面是一个最简单的 Java 应用程序, 它只发送一条消息到终端中:

```java
public class FirstSample{
    public static void main(String[] args){
        System.out.println("We will not use 'Hello, World!'");
    }
}
```

首先, Java 是大小写敏感的, 如果出现大小写拼写错误, 则程序将无法运行.

接下来逐行讲解这个程序的结构, `public` 关键字称为*访问修饰符*(**access modifier**), 用于控制所修饰的对象的访问级别; `class` 关键字声明一个类, 类将在下一章详细介绍; `FirstSample` 是我们自定义的类名, 这个类名要和文件名相同. 类的内容使用 "{}" 包括, 被"{}"包括的代码叫做`代码块`(或简称为块), 可以把类名后被"{}"包括的代码视为这个类的代码块, 也就是这个类的代码内容. Java 声明类的通用语法是

*accessModifier class className{...}*

第二行 `public static void main(String[] args)` 声明了一个`main`方法, `main`方法的声明格式是约定俗称的, Java 程序运行时会首先查找并执行`main`方法,. 第三行调用了`System`系统包下的"out"对象的"**println**"方法, 这个方法会使用标准输出(STDOUT)输出内容, 方法的参数写在"()"内, 每个方法都要用";"结尾. Java 方法调用的通用语法是    

*objec.method(parameters)*

## 注释

Java 最常用的注释方式是 // , 其注释内容从 // 开始到本行结尾.

```java
System.out.println("We will not use 'Hello, World!'"); // This is '//' comment
```

第二种注释方式是使用 /* 和 */ 注释界定符将一段比较长的注释括起来.

最后一种方式可以用来自动地生成文档. 这种注释以 /** 开始, 以 */ 结束. 如

```java
/**
 * This is the first sample program in Core Java Chapter 3
 * @version 1.011 1997-03-22
 * @author Gary Cornell
 */
public class FirstSample{
    public static void main(String[] args){
        System.out.println("We will not use 'Hello, World!'");
    }
}
```

关于使用注释生成文档需要使用 javadoc 工具, 它可以由源文件生成一个 HTML 文档. 具体详情见下一章.

## 数据类型

Java 是一种 **强类型语言**, 这就意味着必须为每一个变量声明一种类型.在 Java 中一共有 **8 种基本类型(primitive type)**, 其中有 4 种整型, 2 种浮点类型, 1 种字符类型 char (用来表示 Unicode 编码的代码单元)和一种用来表示真值的 boolean 类型.

学过计算机基础和C语言的应该都不会陌生, 唯一值得注意的是 Java 的 boolean 类型和C语言的"0/1"真值表达是完全不同的,不能混为一谈. 其次在 Java 中所有的基本类型大小都是固定的, 而在C语言中 int 和 long 的大小有时会随平台变化,这一点也要注意.

### 整型

整型用于表示整数, 允许负数. Java 中没有无符号(unsinged)形式的整型.

|类型|大小|取值范围|
|---|----|------|
|byte|1bytes|-$2^{7}$ ~ $2^{7}$-1|
|short|2bytes|-$2^{15}$ ~ $2^{15}$-1|
|int|4bytes|-$2^{31}$ ~ $2^{31}$-1(约21亿多)|
|long|8bytes|-$2^{63}$ ~ $2^{63}$-1|

### 浮点类型

浮点数分为单精度(float)和双精度(double), 用于表示有小数的数.

|类型|大小|取值范围|
|---|----|------|
|float |4bytes|大约 $\pm3.40E+37F$ (有效位数 6~7 位)|
|double|8bytes|大约 $\pm1.79E+308$(有效位数为 15 位)|

浮点数值可以用于表示小数, 但是不建议用浮点类型直接进行小数运算, 尤其是无法接受舍入误差的运算, 这是因为浮点数值采用二进制系统表示, 有计算机数制基础的就能明白浮点数计算是有误差的, 这是底层硬件导致的. 如果需要浮点数计算, 应该使用 `BigDecimal` 类进行运算, 详情在[大数](#大数)一节介绍.

### char 类型

char 类型原本用于表示单个字符. 不过如今有些 Unicode 字符可以用一个 char 值描述, 另外一些 Unicode 字符则需要两个 char 值. 具体原因在下一小节分析.

char 类型的字面值要用单引号括起来, 例如: 'A' 是编码值为65的字符常量. 它与 "A" 不同, "A" 是一个包含字符 A 的字符串. char 类型可以表示为十六进制, 其范围从 \u0000 到 \uFFFF , 例如, \u2122 表示商标符号(&#x2122;), \u03c0 表示希腊字母 &#x03c0;

除了转义序列\u之外,还有一些表示特殊字符的转义序列:

|转义序列|名称|Unicode值|
|:-----:|:-:|:-------:|
|\b|退格|\u0008|
|\t|制表|\u0009|
|\n|换行|\u000a|
|\r|回车|\u000d|
|\\"|双引号|\u0022|
|\\'|单引号|\u0027|
|\\\\ |反斜杠|\u005c|

Unicode 转义序列会在解析代码之前得到处理. 所以理论上你可以使用转义序列替换掉所有的普通字符来写 Java 代码并成功编译. 即使你不想要这么做也要在平时注意 \ 符号, 避免发生了转义导致语法错误.

### Unicode 和 char 类型

最初 Java 使用 2 个字节(16位) 的 Unicode 字符集来表示 char 类型, 但是现在 Unicode 字符已经超过了 $2^{16}$ (65 536)个, 16位的 char 类型已经无法满足需求了.

为了理解 Java 从 Java 5 开始如何解决这个问题, 需要借助一些术语. **码点**(code point)是指与一个编码表中的某个字符对应的代码值. 在 Unicode 标准中, 码点采用十六进制书写, 并加上前缀 U+, 例如 U+0041 就是拉丁字母 A 的码点. Unicode 码点可以分成17个**代码平面**(code plane). 第一个代码平面称为 **基本多语言平面**(basic multilingual plane), 包括码点从 U+0000 到 U+FFFF 的"经典" Unicode 代码; 其余的 16 个平面的码点为从 U+10000 到 U+10FFFF, 包括 **辅助字符**(supplementary character).

UTF-16 编码采用不同长度的编码表示所有 Unicode 码点. 在基本多语言平面中, 每个字符用 16 位表示, 通常称为**代码单元**(code unit); 而辅助字符编码为一对连续的代码单元. 采用这种编码对表示的各个值落入基本多语言平台中未用的 2048 个值范围内, 通常称为 **替代区域**(surrogate area)( U+D800~U+DBFF 用于第一个代码单元, U+DC00~U+DFFF 用于第二个代码单元). 这样我们可以从中迅速知道一个代码是一个字符的编码, 还是一个辅助字符的第一或第二部分. 例如 &#x1d546; 字符的码点为 U+1D546 , 编码为两个代码单元 U+D835 和 U+DD46. (其中编码的算法描述见 <https://tools.ietf.org/html/rfc2781>)

在 Java 中, **char 类型描述了 UTF-16 编码中的一个代码单元**. 在程序中尽量不要使用 char 类型, 除非确实需要处理 UTF-16 代码单元. 最好将字符串作为抽象数据类型类型处理, 详情见[字符串](#字符串)一节.

## 变量与常量

### 变量

在 Java 中每个变量都有一个类型(type). 声明一个变量, 要使用数据类型名称修饰变量名

*typeName variableName*

变量名不能使用 Java 保留字, 不能用数字作为开头, 应当避免使用除英文字母外的字符, 一些字符如空格是禁止出现在变量名中的. 此外变量名的长度基本上没有限制.

声明一个变量后需要通过 "=" 号对变量赋值, 不然变量会使用默认值, 如整型是 0, 类对象初始默认之是 null .如

```java
int a;
a = 1;
int b = 2; // 在同一行中声明和初始化
```
从 **Java 10** 开始, 对于局部变量, 如果可以从变量的初始值推断出她的类型, 就不再需要声明类型. 只需要使用关键字 `var` 而无需指定类型:

```java
var vacationDays = 12; // vacationDays is an int
var greeting = "Hello"; // greeting is a String
var cat = new Cat(); // cat is a cat class
```
### 常量

在 Java 中, 利用关键字 `final` 指示常量. 如

```java
final int CONST_VAR = 1; // 声明一个常量
```

常量名约定俗成要使用大写, 用下划线 _ 连接, 常量只能被赋值一次, 一旦被赋值就不能够再修改. 此外 `const` 是 Java 保留的关键字, 但目前并没有使用, 定义常量必须使用 `final` 关键字.

### 枚举类

枚举类用于变量的取值只在一个有限的集合时的情况. 一个枚举类包括了有限个命名的值. 例如自定义枚举类 Size,

```java
enum Size {SMALL, MEDIUM, LARGE, EXTRA_LARGE};
```

现在, 可以声明 Size 类型的变量, 变量值从声明的枚举值中选择,

```java
Size s = Size.MEDIUM;
```

Size 类型的变量只能存储这个类型声明中给定的某个枚举值, 或者 null.

## 运算符

Java 运算符包括

**二元运算符**: 加+, 减-, 乘*, 除/, 取模%, 逻辑与&&, 逻辑或||, 逻辑非!, 小于<, 小于等于<=, 大于>, 大于等于>=, 等于==, 与&, 或|, 异或^, 非~, 右移>>, 左移<< .

**一元运算符**: 自增++, 自减-- . 前缀自运算会使用变量运算过后的值, 而后缀自运算会使用变量原来的值然后再进行运算.

**三元运算符**: ?: 表达式

*condition ? expression1:expression2*

如果条件表达式为 true 则返回第一个表达式, 否则返回第二个表达式, 例如

x < y ? x : y

会返回 x 和 y 中较小的一个.

此外在 Math 类中, 包含了许多常用的数学函数, 如取整函数, 随机数函数, 三角函数, 指数函数和对数函数等等, 以及一些数学常量如 PI 和 E 的值.

## 字符串

从概念上讲, Java 字符串就是 Unicode 字符序列. Java 没有内置的字符串类型, 而是在标准 Java 类库中提供了一个预定义类 String. 每一个用双引号括起来的字符串都是 Sting 类的一个实例.

### 字串

String 类的 substring 方法可以从一个较大的字符串中提取一个字串. 例如

```java
String greeting = "Hello";
String s = greeting.substring(0,3); // "Hel"
```

substring 方法截取从第一个参数a开始(例如从0开始), 直到第二个参数b位置(例如到 3 为止, 但不包含 3 )的字串. 字串的长度为 b-a (例如 3-0=3).

### 拼接

和大多数程序设计语言一样, Java 允许使用 + 号连接(拼接)两个字符串.

```java
String name = "John";
int age = 13;
String message = name + " is " + age; // int 会自动转换为String类型
```

如果需要把多个字符串放在一起, 用一个界定符分割, 可以使用静态 join 方法:

```java
String all = String.join("/", "S", "M", "L", "XL"); // all is "S/M/L/XL"
```

Java 11 还提供了一个 repeat 方法:

```java
String repeated = "java".repeat(3) // repeated is "javajavajava"
```

### 不可变字符串

String 类没有提供修改字符串中某个字符的方法. 在 Java 文档中将 String 类称为是 *不可变的* (immutable). 例如, 字符串 "Hello" 永远包含字符 H, e, l, l, o 的代码单元序列. 你不能改变这些值, 但是你可以修改字符串变量 greeting, 让它引用另外一个字符串.

```java
greeting = greeting.substring(0, 3) + "p!";
```

从这一点上就可以发现, String 和 C 中的字符串是不一样的, C 中的字符串是字符数组, 是可变的, String 不可变意味着每次都要分配一组新空间放置字符串的字串或者拼接结果. 不过 String 却要比字符数组方便很多, 而且程序员根本不需要再为内存问题操心, Java 虚拟机会自动分配内存并进行垃圾回收.

为了理解这种不可变的工作形方式, 可以想象将各种字符串存放在一个公共存储池中, 字符串变量指向存储池中相应的位置, 如果复制一个字符串变量, 原始字符串与复制的字符串共享相同的字符串.

总而言之, Java 的设计者认为共享带来的效率要远胜于提取字串/拼接字符串所带来的低效率. 如果需要使用可变字符串来接收输入时 Java 提供了单独的一个类 StringBuilder 用来处理这种情况, 详情参见 [构建字符串](#构建字符串)一节.

### 检测字符串是否相等

可以使用 equals 方法检测两个字符串是否相等.

```java
s.equals(t)
```

如果字符串 s 与字符串 t 相等, 则返回 true, 否则返回 false.

**一定不要使用 == 运算符检测两个字符串是否相等!** 这个运算符只能确定两个字符串的地址是否相同.

如果虚拟机始终将相同的字符串共享, 就可以使用 == 运算符. 但实际上只有字符串*字面量*是共享的, 而 + 或 substring 等操作得到的字符串并不共享.

```java
String greeting = "Hello";
if (greeting == "Hello") ...
    // probably true
if (greeting.substring(0, 3) == "Hel") ...
    // probably false
```

### 空串与 Null 串

空串 "" 是长度为0的字符串, 可以通过 `str.length() == 0` 或 `str.equals("")` 检查一个字符串是否为空.

空串是一个 Java 对象, 有自己的长度(0)和内容(空). null 串则表示没有任何对象与该变量关联. 可以通过 `str == null` 来检查一个字符串是否为 null.

有时候要检查一个字符串既不是 null 串也不是空串, 就要用到一下条件

```java
if (str != null && str.length() != 0)
```

首先要检查 str 不为 null. 否则如果在一个 null 值上调用方法, 会出现错误.

### 码点与代码单元

Java 字符串由 char 值序列组成. 由 [char](#unicode-和-char-类型) 类型一节可以看到, char 数据类型是一个采用 UTF-16 编码表示 Unicode 码点的代码单元. 

length 方法将返回采用 UTF-16 编码表示给定字符串所需的代码单元数量. 例如

```java
String greeting = "Hello";
int n = greeting.length();
```

要想得到实际的长度, 即码点数量, 可以调用 codePointCount 方法. 例如

```java
int cpCount = greeting.codePointCount(0, greeting.length());
```

调用 s.charAt(n) 将返回位置 n 的代码单元, n 介于 0 ~ s.length()-1 之间. 例如

```java
char first = greeting.charAt(0) // 'H'
char last = greeting.charAt(4) // 'o'
```

要想得到第 i 个码点, 应该使用下列语句

```java
int index = greeting.offsetByCodePoints(0, i);
int cp = greeting.codePointAt(index);
```

为什么要格外重视代码单元? 考虑这样的语句:

> "&#x1d546; is the set of octonions."

使用 UTF-16 编码表示 &#x1d546;(U+1D546)需要两个代码单元. 调用

```java
char ch = sentence.charAt(1) // ch ==> '?'
```
返回的不是一个空格, 而是 &#x1d546; 的第二个代码单元. 为了避免这个问题, 不要使用 char 类型.

如果想要遍历一个字符串, 并且依次查看每一个码点, 例如:

```java
int i = 0;
int len = sentence.length();
System.out.print("[");
while(i < len){
    int cp = sentence.codePointAt(i);
    System.out.print("'" + Character.toString(cp) + "'" + (i == len - 1 ? "":", ")); // 将码点转换为 String 输出
    if(Character.isSupplementaryCodePoint(cp)) // Character.isSupplementary(cp) 判断 cp 是否是辅助字符
        i += 2;
    else i++;
}
System.out.println("]");
```

输出结果为:
> ['𝕆', ' ', 'i', 's', ' ', 't', 'h', 'e', ' ', 's', 'e', 't', ' ', 'o', 'f', ' ', 'o', 'c', 't', 'o', 'n', 'i', 'o', 'n', 's', '.']

可以使用下列语句实现反向遍历:
```java
i--;
if (Character.isSurrogate(sentence.charAt(i))) // Character.isSurrogate(char ch) 判断字符单元是否是替代区域(surrogate area)的代码单元
    i--;
int cp = sentence.codePointAt(i);
```

更容易的办法是使用 codePoints 方法, 它会生成一个 int 值的"流", 每个 int 值对应一个码点.(流将在[第二卷](13)中学习)可以将它转换为一个数组, 再完成遍历.
```java
int[] codePoints = sentence.codePoints().toArray();
// codePoints ==> int[26] { 120134, 32, 105, 115, 32, 116, 104, 101 ... , 105, 111, 110, 115, 46 }
```

反之, 要把一个码点数组转换为一个字符串, 可以使用构造器.
```java
String str = new String(codePoints, 0, codePoints.length);
// str ==> "𝕆 is the set of octonions."
```

### String API

Java 中的 String 类包含了 50 多个方法, 它们绝大多数都很有用, 碍于篇幅, 这里就不再赘述, 详情可以参见 Java 的[联机文档](https://docs.oracle.com/en/java/javase/11/docs/api/index.html)(或者本地离线文档, 文档下载方式在[Java 环境](2022-10-20-Core-Java（2）Java环境.markdown#安装库源文件和文档)中有过提及.)

### 构建字符串

如果需要用许多小段的字符串来构建一个字符串, 采用字符串拼接的方式效率会比较低. 每次拼接字符串时, 都会构建一个新的 String 对象, 既耗时, 又浪费空间. Java 提供了 StringBuilder 类来专门应对这种问题, 首先, 构建一个空的字符串构建器:
```java
StringBuilder builder = new StringBuilder();
```

当每次需要添加一部分内容时, 就调用 append 方法
```java
builder.append(ch);
builder.append(str);
```

在字符串构建完成时就调用 toString 方法, 将可以得到一个 String 对象,
```java
String copletedString = builder.toString();
```

除了拼接方法, StringBuilder 还有插入和删除方法(详情可见 [API](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/StringBuilder.html)), 可以灵活修改字符串.

## 输入输出

"标准输入流"(STDIN) 和 "标准输出流"(STDOUT) 是操作系统I/O方面的知识, 我会在[操作系统](/os/io)一章详细学习. 在本节不用关心底层原理, 把输入输出当作控制台(终端)的输入输出就行, 在这里我们更关心的是 Java 的输入输出方法.

### 读取输入

前面的例子已经看到, 将输出打印到"标准输出流"(即控制台窗口)只需要调用 `System.out.println` 即可. 然而, 读取 "标准输入流" System.in 就没有那么容易了, 要想通过控制台进行输入, 首先需要构造一个与 "标准输入流" System.in 关联的 Scanner 对象.(Scanner 的 [API 文档](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Scanner.html#method.summary))
```java
Scanner in = new Scanner(System.in);
```

然后使用 Scanner 的 next 方法获取输入. 以下是一个简单的输入实例

```java
import java.util.*;
public class InputTest{
    public static void main(String[] args){
        Scanner in = new Scanner(System.in);

        // get first input
        System.out.print("What is your name"):
        String name = in.nextLine(); // 读取下一行内容

        // get secont input
        System.out.print("How old are you?");
        int age = in.nextInt(); // 读取一个整数

        // display output on console
        System.out.println("Hello, " + name + ". Next year, you'll be " + (age + 1));
    }
}
```

标准I/O在操作系统中实际上有着更深层的意义. 如果专门为了接收控制台输入, 可以参考 Java 6 特别引用的 Console 类, 它用来专门接收控制台的输入, 而且针对密码输入要求保密的需求提供了 readPassword 方法(输入不可见).
```java
Console cons = System.console();
String username = cons.readLine("User name: ");
char[] passwd = cons.readPassword("Password: ") ;
```

为了安全起见, 返回的密码存放在一个字符数组中, 在对密码处理完成后, 应该马上用一个填充值覆盖数组元素. 采用 Console 对象处理输入必须每次读取一行输入, 没有能够单独读取单个单词或数值的方法.

### 格式化输出

Java 提供格式化输出 `System.out.printf` 方法, 使用方式和C语言的 printf 函数相仿, 可以使用转换符与参数替换, 如
```java
System.out.printf("Hello, %s, Next year, you'll be %d.", name, age);
```

可以使用静态的 String.format 方法创建一个格式化的字符串,
```java
String message = String.format("Hello, %s, Next year, you'll be %d.", name, age);
```

关于格式化语法和参数的详情可以参加 [Formatter](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Formatter.html#syntax) 文档.

### 文件输入与输出

要想读取一个文件, 需要构造一个 Scanner 对象, 例如
```java
Scanner in = new Scanner(Path.of("myfile.txt"), StandardCharsets.UTF_8);
```

之后就可以使用 Scanner 的 next 方法读取文件, 例如 nextLine 方法读取文档的每一行内容.

要想写入文件, 就需要构造一个 PrintWriter 对象, 例如
```java
PrintWriter out = new PrintWriter("myfile.txt", StandardCharsets.UTF_8);
```

如果文件不存在, 会自动创建该文件. 可以像输出到 System.out 一样使用 print , println , printf 方法写入文档.(PrintWriter [API 文档](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/PrintWriter.html#method.summary))

## 控制流

Java 的控制流程结构与 C 和 C++ 的控制流程结构一样, 不过 Java 没有 goto 语句, 但是 break 语句可以带标签, 可以利用它从内层循环跳出.

### 块作用域

块(block)(即复合语句)是指由若干条 Java 语句组成的语句, 并用一对大括号括起来. 块确定了变量的作用域. 一个块可以嵌套在另一个块中. 例如
```java
public static void main(String[] args){
    int n;
    ...
    {
        int k;
        ...
    } // 变量 k 只能在里面的这个块中使用
}
```

但是, 不能在嵌套的两个块中声明同名的变量. 例如下面的代码就有错误, 而无法通过编译.
```java
public static void main(String[] args){
    int n;
    ...
    {
        int n; // ERRO!!!
        ...
    } 
}
```

### 条件语句

在 Java 中, 条件语句的形式为

*if (condition) statement*

与绝大多数程序设计语言一样, Java 希望在条件为真时执行多条语句, 在这种情况下, 就可以使用块语句(block statement)
```
{
    statement1
    statement2
    ...
}
```

在 Java 中, 更一般的条件语句为

*if (condition) statement1 else statement2*

多分支条件语句

*if (condition1) statement1 <br>
else if (condition2) statement2 <br>
... <br>
else statementn*

### 循环

当条件为true时, while 循环执行一条语句(也可以是一个块语句).

*while (condition) statement* 

while 循环会先检测条件在决定是否执行, 可能会一次都不执行, 如果想要至少执行一次, 使用 do/while 循环将检测放在最后

*do statement while (condition)*

### 确定循环

for 循环语句是支持迭代的一种通用结构, 由一个计数器或类似的变量控制迭代次数, 每次迭代后这个变量将会更新. 例如
```java
for(int i = 1; i <= 10; i++)
    System.out.println(i);
```

for 语句的第1部分通常是对计数器的初始化; 第2部分给出每次新一轮循环前要检测的循环条件; 第3部分指定如何更新计数器.

### 多重选择: switch语句

在处理多个选项时, 使用 if/else 结构显得有些笨拙. Java 提供和 C/C++ 完全一样的 switch 语句.
```java
switch(choice){
    case 1:
        ...
        break;
    case 2;
        ...
        break;
    ...
    default:
        // 如果没有相匹配的 case 标签, 就执行 default
        ...
        break;
}
```

 case 标签可以是类型为 char, byte, short 或 int 的基本类型, 也可以是枚举常量, 从 Java 7 开始, case 标签还可以是字符串字面量.

如果在 case 分支语句的末尾没有 break 语句, 那么就会接着执行下一个 case 语句. 在一些情况下可以运用这个技巧优化代码效率, 这种技巧叫做多路复用技术, 然而大多数情况下, 如果因为疏忽 break 语句, 可能会导致严重的错误.

如果在编译时加上 -Xlint:fallthrough 选项, 如
```shell
java -Xlint:fallthrough Test.java
```

那么编译器就会检测分支, 如果分支末尾缺少 break 语句, 编译器就会给出一个警告消息. 如果你确实是想要这种"直通式"(fallthrough)行为,可以在外围方法加一个注解 @SuppressWarnings("fallthrough") , 这样就不会对这个方法生成警告了.(注解会在第2卷中详细学习)

### 中断控制流程

尽管 Java 的设计者将 goto 作为保留字, 但实际上并没有打算在语言中使用它. 取而代之的是带标签的 break, 例如
```java
Scanner in = new Scanner(System.in);
int n;
read_data: // 标签
while(...){ // 这个循环体被 read_data 标签标记
    ...
    for(...){ // 内循环
        System.out.print("Enter a number");
        n = in.next.Int();
        if(n<0) // 一旦不符合条件, 就终止所有循环
            break read_data; // 中断整个 read_data 循环而不只是内部的for循环
    }

}
```

最后, 还有一个 continue 语句, 与 break 语句一样, 它将中断正常的控制流程, continue 语句将控制转移到内层循环的首部, 即下一次循环开始. 同样的, continue 也可以带标签, 它将跳转到与标签匹配的循环的首部(下一次循环开始).

## 大数

如果基本的整数和浮点数精度不能满足需求, 那么可以使用 java.math 包下的两个类: BigInteger 和 BigDecimal . 这两个类可以处理包含任意长度数字序列的数值. BigInteger 类实现任意精度的整数运算, BigDecimal 类实现任意精度的浮点运算.

使用静态的 valueOf 方法可以将普通的数值转换为大数:
```java
BigInteger a = BigInteger.valueOf(100);
BigInteger a = BigInteger.valueOf(-100);
// 对于更大的数, 可以使用一个带字符串参数的构造器
BigInteger reallyBig = new BigInteger(""222222222223333333444444499999999920394212221333000);
// 大数不能直接运算, 要使用实例方法运算
BigInteger c = a.add(b); // c = a + b
BigInteger d = c.multiply(b.add(a)); // d = c * (b + a)
```

## 数组

数组是一种数据结构, 用来存储同一类型值的集合.
```java
// 声明数组
int[] a;
// 使用 new 操作符创建数组
a = new int[100];
// 声明并赋值
int[] b = new int[10];
// 同样合法的声明和赋值方式
int c[] = new int[2];
int[] d = {1,2,3};
```

数组的长度在声明过后就是固定的, 不可以再次改变, 当然数组元素可以随意改变. 如果程序中需要经常扩展数组的大小, 就应该使用另一种数据结构--数组列表(array list), 有关数组列表的详情见 [集合](9) 一章.

### 数组遍历

数组的长度可以通过 array.length 获得, 要遍历一个数组可以这样
```java
for(int i=0; i < a.length; i++)
    System.out.println(a[i]);
```

Java 同时提供了一种功能强大的循环结构, 可以依次来处理数组中的每一个元素, 这种增强的 for 循环叫做 for each 循环, 例如
```java
for(int element:a)
    System.out.println(ele);
```

这个循环应该读作"循环 a 中的每一个元素"(for each element in a).

### 数组拷贝

在 Java 中, 允许将一个数组变量拷贝到另一个数组变量. 这时, 两个变量将引用同一个数组:
```java
int[] luckyNumbers = smallPrimes;
luckyNumbers[5] = 12; // now smallPrimes[5] is 12
```

如果希望将一个数组的所有值都拷贝到一个新的数组中去, 就应该使用 Arrays 类的 copyOf 方法:
```java
int[] copiedLuckyNumbers = Arrays.copyOf(luckyNumbers, luckyNumbers.lentgh);
```

第 2 个参数是新数组的长度. 这个方法通常用来增加数组的大小:
```java
luckyNumbers = Arrays.copyOf(luckyNumbers, 2 * luckyNumbers.length);
```

如果数组元素是数值型, 那么额外的元素将被赋值为0; 如果数组元素是布尔型, 则将赋值为 false. 相反, 如果长度小于原始数组的长度, 则只拷贝前面的值.

### 命令行参数

每一个 Java 应用程序都有一个带 String[] args 参数的 main 方法. 这个参数表明 main 方法将接收一个字符串数组, 也就是命令行上指定的参数. 例如:
```java
public class ArgsTest{
    public static void main(String[] args){
        for(int i = 0; i < args.length; i++)
            System.out.println(args[i]);
    }
}
```

在命令行编译, 并执行
```shell
java ArgsTest Hello cruel world
```

那么将会输出这段信息
> Hello cruel world

### 数组排序

要对数值型的数组进行排序, 可以使用 Arrays 类中的 sort 方法:
```java
int[] a = {...};
Arrays.sort(a);
```

这个方法使用了优化的快速排序(QuickSort)算法. Arrays API 还提供了许多便利的方法, 详情可以参见[Arrays 文档](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Arrays.html#method.summary)

### 多维数组

Java 中的数组可以嵌套, 例如一个二位数组可以这样声明
```java
int[][] matrix = new int[m][n]; // 声明一个 m*n 的二维数组

// 遍历二维数组
for (int i = 0; i < m; i++) // 先按列数遍历行
    for (int j = 0; j < n; j++) // 再按一般的行遍历数组元素
        System.out.println(matrix[i][j]);

// 使用foreach遍历
for (int[] row:matrix)
    for (int element:row)
        System.out.println(element);

// 快速打印一个二维数组
System.out.println(Arrays.deepToString(matrix))

```

### 不规则数组

Java 实际上没有多维数组, 只有一维数组. 多维数组被解释为"数组的数组".例如下面的程序将创建一个数组, 第i行到第j行将存放"从i个数中抽取j个数"可能的结果数.
```java
final int NMAX = 10;
// 声明一个"数组的数组"
int[][] odds = new int[NMAX+1][];
// 为每一个行数组赋值
for(int n = 0; n <= NMAX; n++)
    odds[n] = new int[n + 1];
// 填充元素
for (int n = 0; n < odds.length; n++)
    for (int k = 0; k < odds[n].length; k++){
        int lotteryOdds = 1;
        for (int i = 1; i <=k; i++)
            lotteryOdds = lotteryOdds * (n - i + 1)/i;

        odds[n][k] = lotteryOdds;
    }

// print triangular array
for(int[] row : odds){
    for(int odd : row)
        System.out.printf("%4d", odd);
    
    System.out.println();
}
```

输出结果:
```
   1
   1   1
   1   2   1
   1   3   3   1
   1   4   6   4   1
   1   5  10  10   5   1
   1   6  15  20  15   6   1
   1   7  21  35  35  21   7   1
   1   8  28  56  70  56  28   8   1
   1   9  36  84 126 126  84  36   9   1
   1  10  45 120 210 252 210 120  45  10   1
```