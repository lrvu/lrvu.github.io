---
layout: post
title: Core Java（5）接口/lambda表达式与内部类
categories: Core Java
permalink: "/:categories/5"
---

## 接口

接口(interface)用来描述类应该做什么, 而不指定它们具体应该如何做, 一个类可以实现(implement)一个或多个接口. 在 Java 中, 接口不是类, 而是对希望符合这个接口的类的一组需求.

Arrays 类中的 sort 方法承诺可以对对象数组进行排序, 但要求满足对象所属的类必须实现 Comparable 接口.
```java
public interface Comparable<T>{
    int compareTo(T other);
}
```
这说明, 任何实现 Comparable 接口的类都需要包含 compareTo 方法.

接口中所有方法都自动是 public 方法, 因此, 在接口中声明方法, 不必提供关键字 public.

compareTo 接口要求, 在调用 x.compareTo(y) 时, 这个 compareTo 方法必须确实能够比较两个对象, 并返回比较结果, 即 x 和 y 哪一个更大. 当 x 小于 y 时, 返回一个负数; 当 x 等于 y 时, 返回 0 ; 否则返回一个正数. 例如, 当 x 和 y 都是整型时, 那么接口实现返回值应该是 `x-y` 或 `Integer.compare(x, y)`.

### 接口的属性

接口不是类, 不能使用 new 运算符实例化一个接口. 不过, 尽管不能构造接口的对象, 却能够声明接口的变量. 接口变量必须引用实现了这个接口的类对象. 可以使用 instanceof 检查一个对象是否实现了某个特定的接口.
```java
if(anObject instanceof Comparable){...}
```
接口中不能包含实例字段, 但是可以包含常量, 与建立类的继承层次一样, 也可以扩展接口.
```java
public interface Movable{
    void move(double x, double y);
}
public interface Powered extends Movable{
    double milesPerGallon();
    Double SPEED_LIMIT = 95;
}
```
有的接口只定义常量, 而没有定义方法. 例如 SwingConstants 就是这样一个接口, 任何实现 SwingConstants 接口的类都自动地继承了这些常量.

### 接口与抽象类

有些设计语言例如 C++ 允许一个类有多个超类, 这种特性被称为多重继承(multiple inheritance). Java 不支持多重继承, 但实际上, 接口可以提供多重继承, 同时还能避免多重继承的复杂性和低效性.

### 静态和私有方法

在 Java 8 中, 允许在接口中增加静态方法. 理论上没有任何理由认为这是不合法的, 只是这有违于将接口作为抽象规范的初衷.

在此之前, 通常的做法是将静态方法放在伴随类中. 在标准库中, 你会看到成对出现的接口和实用工具类, 如 Collection/Collections 或 Path/Paths.

可以由一个 URL 或者字符串序列构造一个文件和目录的路径, 如 Paths.get("jdk-11", "conf", "security"). 在 Java 11 中, Path 接口提供了等价的方法:
```java
public interface Path{
    public static Path of(URI url){...}
    public static Path of(String first, String... more){...}
}
```

这样一来 Paths 类就不再是必要的了. 类系地, 在实现你自己的接口时, 没有理由再为实用工具方法另外提供一个伴随类.

在 Java 9 中, 接口中的方法可以说 private. private 方法可以是静态方法或者实例方法. 由于私有方法只能在接口本身的方法中使用, 所以只能作为接口中其他方法的辅助方法.

### 默认方法

可以为接口方法提供一个默认实现. 必须使用 default 修饰符标记这样一个方法.
```java
public interface Comparable<T>{
    default int compareTo(T other){return 0; }
    // by default, all elements are the same
}
```
当然, 这并没有太大用处. 因为 Comparable 的每一个具体实现都会覆盖这个方法. 但是在有些时候可能会很有用, 实现接口的程序员只需要实现必须实现的方法就行了.

默认方法的一个重要用法是 "接口演化" (interface evolution). 以 Collection 接口为例, 假设很久以前你提供了这样一个类:
```java
public class Bag implements Collection
```
后来, 在 Java 8 中, 又为这个接口增加了一个 stream 方法. 假设 stream 不是一个默认方法, 那么 Bag 类将不能编译, 因为它没有实现这个方法. 为接口增加一个非默认方法不能保证 "源代码兼容" (source compatible). 只要将方法实现为一个默认(default)方法就可以解决这个问题.

### 解决默认方法冲突

如果先在一个接口中将一个方法定义为默认方法, 然后又在超类或另一个接口中定义同样的方法, 就会发生冲突, 针对这些冲突 Java 的规则是:
1. 超类优先, 如果超类提供了一个具体方法, 同名而且参数类型的默认方法会被忽略.
2. 接口冲突. 如果两个接口提供了同名且参数类型相同的默认方法, 必须覆盖这个方法来解决冲突.

### 接口与回调

回调(callback)是一种常见的程序设计模式. 在这种模式中, 可以指定某个特定事件发生时应该采取的动作.

在 java.swing 包中有一个 Timer 类, 如果希望经过一定时间间隔就得到通知, 就可以使用 Timer 类, 构造定时器需要传入一个 ActionListener 接口的实现类, 来告诉定时器要做什么. 例如下面这个实现类会打印一条消息, 然后响一声.
```java
class TimePrinter implements ActionListener{
    public void actionPerformed(ActionEvent event){
        System.out.println("At the tone, the time is "
            + Instant.ofEpochMilli(event.getWhen()));
        Toolkit.getDefaultToolkit().beep();
    }
}
``` 
需要注意 actionPerformed 方法的 ActionEvent 参数, 这个参数提供了事件的相关信息, 例如, 发生这个事件的时间. event.getWhen 会返回这个事件发生时间的毫秒数, 如果将它传入静态方法 Instant.ofEpochMilli, 可以得到一个更可读的描述.

接下来构造这个类的一个对象, 并将它传递给 Timer 构造器.
```java
var listener = new TimerPrinter();
Timer t = new Timer(1000, listener);
t.start();
```

### Comparator 接口

比较器是实现了 Comparator 接口的类的实例:
```java
public interface Comparator<T>{ int compare(T first, T second); }
```
和 comparable 接口规则一样, 当first大于second时, 返回值为正数; 相等时返回0; 否则返回负数.

### 对象克隆 Cloneable 接口

Cloneable 接口提供了一个安全的 clone 方法. 为一个包含对象引用的变量建立副本, 原变量和副本都是一个对象的引用, 任何一个变量改变都会影响另一个变量:
```java
var original = new Employee(...);
Employee copy = original;
copy.raiseSalary(10); // also changed original
```
如果希望 copy 是一个新对象, 它的初始状态与 original 相同, 但是之后它们各自会有自己不同的状态, 这种情况下就要用 clone 方法
```java
Employee copy = original.clone();
copy.raiseSalary(10); // original unchanged
```
不过并没有这么简单, clone 方法是 Object 的一个 protected 方法, 这说明你的代码不能直接调用这个方法. 只有 Employee 类可以克隆 Employee 对象. 这个限制是有原因的. Object 类实现 clone 是逐个字段的拷贝, 因为它对这个对象一无所知, 如果对象包含子对象的引用, 拷贝字段就会得到相同子对象的另一个引用, 这样一来, 原对象和克隆对象仍然会共享一些信息. 也就是说, 默认的克隆操作是 "浅拷贝", 并没有克隆对象中引用的其他对象. 如果原对象和浅克隆对象共享的子对象是不可变的, 那么这种共享就是安全的.

不过, 通常子对象都是可变的, 必须重新定义 clone 方法来建立一个深拷贝(deep copy), 同时克隆出所有子对象.

对于每一个类, 需要确定:
1. 默认的 clone 方法是否满足需求;
2. 是否可以在可变的子对象上调用clone来修补默认的clone方法;
3. 是否不该使用clone.

实际上第3个选项是默认选项. 如果选择第1项和第2项, 类必须:
1. 实现Cloneable接口;
2. 重新定义clone方法, 并指定 public 访问修饰符.

在这里, Cloneable 接口的出现与接口的正常使用并没有关系, 具体来说, 它没有指定clone方法, 这个方法是从 Object 类继承的. 这个接口只是作为一个标记, 指示类设计者了解克隆过程. 如果一个对象请求克隆, 但是没有实现这个接口, 就会产生一个检查型异常. Cloneable 接口是 Java 提供的少数标记接口(tagging interface)之一. 标记接口不包含任何方法, 它唯一的作用就是允许在类型查询中使用 instanceof.

即使 clone 的默认(浅拷贝)实现能够满足要求, 还是要实现 Cloneable 接口, 将 clone 重新定义为 public, 再调用 super.clone(). 
```java
class Employee implements Cloneable{
    public Employee clone() throws CloneNotSupportedException{
        return (Employee) super.clone();
    }
}
```

## lambda 表达式

lambda 表达式是一个可传递的代码块, 可以在以后执行一次或多次. 在之前接口的例子中, 我们有时需要将一个代码块传递到某个对象中, 这个代码块会在将来某个时间调用. 例如:
```java
class LengthComparator implements Comparator<String>
{
    public int compare(String first, String second)
    {
        return first.length() - second.lentgh();
    }
}
``` 
传入 `first.length() - second.length()` 代码块来检查字符串是否比另一个字符串短. Java 是一种强语言类型, 所以我们要指定这个代码块的类型:
```java
(String first, String second)
    -> first.length() - second.length()
```

上述代码就是一个完整的 lambda 表达式, lambda 表达式就是一个代码块, 以及必须传入代码的变量的规范. 如果代码要完成的计算无法放在一个表达式中, 可以放在 {} 中, 并包含显式的 return 语句. 例如:
```java
(String first, String second){
    if(first.length() < second.length()) return -1;
    else if(first.length() > second.length()) return 1;
    else return 0;
}
```
即使 lambda 表达式没有参数, 仍然要提供空括号, 就像无参数方法一样:
```java
() -> {for(int i = 100; i>=0;i--) System.out.println(i);}
```
如果可以推导出一个 lambda 表达式的参数类型, 就可以忽略其类型. 例如:
```java
Comparator<String> comp
    = (first, second) -> first.length() - second.length();
```
如果方法只有一个参数, 而且这个参数的类型可以推到得出, 那么甚至还可以省略小括号:
```java
ActionListener listener = event -> System.out.println("...");
```
无需指定 lambda 表达式的返回类型, lambda 表达式的返回类型总是会由上下文推到得出.

### 函数式接口

对于只有一个抽象方法的接口, 需要这种接口的对象时, 就可以提供一个 lambda 表达式. 这种接口称为函数式接口(functional interface). 考虑 Arrays.sort 方法, 它的第二个参数需要一个 Comparator 实例, Comparator 就是只有一个方法的接口, 所以可以提供一个 lambda 表达式:
```java
Arrays.sort(words, (first, second) -> first.length()-second.length());
```
最好把 lambda 表达式看作是一个函数, 而不是一个对象, 另外要接受 lambda 表达式可以传递到函数式接口. 实际上, 在 Java 中, 对 lambda 表达式所能做的也只是转换为函数式接口.

Java API 在 java.util.function 包中定义了很多非常通用的函数式接口. 其中有一个接口 `BiFunction<T, U, R>` 描述了参数类型为T和U而且返回类型为R的函数, 可以把我们的字符串比较 lambda 表达式保存在这个类型的变量中:
```java
BiFunction<String, String, Integer> comp 
    = (first, second) -> first.length()-second.length();
```
不过, 这对于排序并没有帮助. 没有哪个 Arrays.sort 方法想要接收一个 BiFunction.

java.util.functon 包中有一个尤其有用的接口 Predicate:
```java
public interface Predicate<T>{
    boolean test(T t);
    // additional default and static methods
}
```
ArrayList 类中有一个 removeIf 方法, 它的参数就是一个 Predicate. 例如, `list.removeIf(e -> e == null)` 将从一个数组列表删除所有 null 值.

另一个有用的接口是 Supplier<T>:
```java
public interface Supplier<T>{
    T get();
}
```
供应者(supplier)没有参数, 调用时会生成一个 T 类型的值. 供应者用于实现懒计算. 例如:
```java
LocalDate hireDay = Object.requireNonNullOrElse(day, new LocalDate(1970, 1, 1));
```
上面这种写法会先生成一个 LocalDate 对象传入方法, 这不是最优的写法, 我们预计 day 很少为 null, 所以希望只在必要时才构造默认的 LocalDate. 通过使用供应者, 就能延迟这个计算:
```java
LocalDate hireDay = Object.requireNonNullOrElseGet(day, () -> new LocalDate(1970, 1, 1));
```
requireNonNullOrElseGet 方法只在需要值时才调用供应者.

### 方法引用

有时, lambda 表达式涉及一个方法. 例如, 需要一个定时器事件打印这个事件:
```java
var timer = new Timer(1000, event -> System.out.println(event));
```
但是, 如果直接把 println 方法传递到 Timer 构造器就更好了. 具体做法如下:
```java
var timer = new Timer(1000, System.out::println);
```
表达式 `System.out::println` 是一个方法引用(method reference), 它指示编译器生成一个函数式接口的实例, 覆盖这个接口的抽象方法来调用给定的方法. 在这个例子中, 会生成一个 ActionListener, 它的 actionPerformed(ActionEvent e) 方法要调用 System.out.println(e).

如果想对字符串进行排序, 而不考虑字母的大小写, 可以传递以下方法表达式:
```java
Arrays.sort(str, String::compareToIgnoreCase);
```

从例子可以看出, 要用 `::` 运算符分隔方法名与对象或类名.

有时 API 包含一些专门用作方法引用的方法. 例如, Objects 类有一个方法 isNull, 用于测试一个对象引用是否为 null. 咋看上去好像没有什么用, 不过可以把方法引用传递到任何有 Predicate 参数的方法. 例如, 要从一个列表删除所有 null 引用:
```java
list.removeIf(Objects::isNull);
```

### 构造器引用

构造器引用和方法引用很类似, 只不过方法名为 new. 例如, `Person::new` 是 Person 构造器的一个引用. 哪一个字符串呢? 这取决于上下文, 假设你有一个字符串列表, 可以把它转换为一个 Person 对象数组, 为此要在各个字符串上调用构造器, 调用如下:
```java
ArrayList<String> names = ...;
Stream<Person> stream = names.stream().map(person::new);
List<Person> people = stream.collect(Collectors.toList());
```
关于流的详细内容会在[卷II](13)讨论. 就现在来说, 重点是map方法会为各个列表元素调用 Person(String) 构造器. 如果有多个 Person 构造器, 编译器会选择有一个 String 参数的构造器, 因为它从上下文推导出这是在调用带一个字符串的构造器.

可以用数组类型建立构造器引用, 例如 `int[]::new` 是一个构造器引用, 它有一个参数: 即数组的长度, 这等价于 `x -> new int[x]` .

Java 有一个限制, 无法构建泛型类型 T 的数组. 数组构造器引用对于克服这个限制很有用. 表达式 `new T[n]` 会产生错误, 因为这回改为 `new Object[n]`. Stream 接口有一个 toArray 方法可以返回 Object 数组:
```java
Object[] people = stream.toArray();
```
要想得到一个 Person 引用数组, 可以把 `Person[]::new` 传入 toArray 方法:
```java
Person[] people = stream.toArray(Person[]::new);
```
toArray 方法调用这个构造器来得到一个有正确类型的数组. 然后填充并返回这个数组.

> 扩展: 数组构造器引用参数的方法签名为 `stream.toArray(IntFunction<A[]> generator)`.

### 变量作用域

lambda 表达式可以捕获外围作用域中变量的值, 例如:
```java
public static void repeatMessage(String text, int delay){
    AcctionListener listener = even -> System.out.println(text);
    new Timer(delay, listener).start();
}
```
在 Java 中, 要确保所捕获的值是明确定义的, 这里有一个重要的限制, 在 lambda 表达式中, 只能引用值不会改变的变量, 即必须是事实最终变量(effectively final). 事实最终变量是指, 这个变量初始化后就不会再为它赋新值.

lambda 表达式的体与嵌套块有相同的作用域. 这里同样适用命名冲突和遮蔽的有关规则. 在 lambda 表达式中声明与一个局部变量同名的参数或局部变量是不合法的.

### 处理 lambda 表达式

使用 lambda 表达式的重点是**延迟执行**(deferred execution). 毕竟, 如果想要立即执行代码, 完全可以直接执行, 而无须把它包装在一个 lambda 表达式中. 之所以希望以后再执行代码, 这有很多原因, 如:
- 在一个单独的线程中运行代码;
- 多次运行代码;
- 在算法适当位置运行代码(例如, 排序中的比较操作);
- 发生某种情况时执行代码(如, 点击了一个按钮, 数据到达, 等等);
- 只在必要时才运行代码.

例如, 假设你想要重复一个动作 n 次, 将这个动作和重复次数传递到一个 repeat 方法, 可以使用 Java API 提供的函数式接口 Runnable 接口:
```java
public static void repeat(int n, Runnable action){
    for (int i = 0; i < n; i++) action.run();
}
```
常用函数式接口有:

|函数式接口|参数类型|返回类型|抽象方法名|描述|其他方法|
|-------|-------|-------|--------|---|-------|
|Runnable|无|void|run|作为无参数或返回值的动作运行||
|Supplier<T>|无|T|get|提供一个 T 类型的值||
|Consumer<T>|T|void|accept|处理一个 T 类型的值|andThen|
|BiConsumer<T, U>|T, U|void|accept|处理 T 和 U 类型的值|andThen|
|Function<T, R>|T|R|apply|有一个 T 类型参数的函数|compose, andThen, identity|
|BiFunction<T, U, R>|T, U|R|apply|有 T 和 U 类型参数的函数|andThen|
|UnaryOperator<T>|T|T|apply|类型 T 上的一元操作符|compose, andThen, identity|
|BinaryOperator<T>|T, T|T|apply|类型 T 上的二元操作符|andThen, maxBy, minBy|
|Predicate<T>|T|boolean|test|布尔值函数|and, or, negate, isEqual|
|BiPredicate<T, U>|T, U|boolean|test|有两个参数的布尔值函数|and, or, negate|

现在让这个例子更复杂一些. 我们希望告诉这个动作出现在哪一次迭代中. 为此需要选择一个合适的函数式接口, 其中要包含一个方法, 这个方法有一个 int 参数而且返回类型为 void. 处理 int 值的标准接口如下:
```java
public interface IntConsumer{
    void accept(int value);
}
```

下面给出 repeat 方法的改进版本:
```java
public static void repeat(int n, IntConsumer action){
    for(int i = 0; i < n; i++) action.accept(i);
}
```
可以这么调用 `repeate(10, i -> System.out.println("Countdown: " + i));`.

下表列出了基本类型 int, long 和 double 的34个可用的特殊化接口. 使用这些特殊化接口比使用通用接口更高效. (注: *p/q* 是 int, long, double; *P/Q* 是 Int, Long, Double)

|函数式接口|参数类型|返回类型|抽象方法名|
|--------|-------|-------|--------|
|BooleanSupplier|无|boolean|getAsBoolean|
|*P*Supplier|无|p|getAs*P*|
|*P*Consumer|p|void|accept|
|Obj*P*Consumer\<T>|T, *p*|void|accept|
|*P*Function\<T>|*p*|T|apply|
|*P*ToQFunction|*p*|*q*|applyAs*Q*|
|To*P*Function\<T>|T|*p*|applyAs*P*|
|To*P*BiFunction\<T, U>|T, U|*p*|applyAs*P*|
|*P*UnaryOperator|*p*|*p*|applyAs*P*|
|*P*BinaryOperator|*p*, *p*|*p*|applyAs*P*|
|*P*Predicate|*p*|boolean|test|

大多数标准函数式接口都提供了非抽象方法来生成或合并函数. 例如, `Predicate.isEqual(a)` 等同于 `a::equals`, 不过如果 a 为 null 也能正常工作. 已经提供了默认方法 and, or, negage 来合并谓词. 例如, `Predicate.isEqual(a).or(Predicate.isEqual(b))` 就等同于 `x -> a.equals(x) || b.equals(x)`.

如果设计你自己的接口, 其中只有一个抽象方法, 可以用 `@FunctionInterface` 注解来标记这个接口.

### 再谈 Comparator

Comparator 接口包含很多方便的静态方法来创建比较器. 这些方法可以用于 lambda 表达式或方法引用.

静态 coparing 方法取一个 "键提取器" 函数, 它将类型 T 映射为一个可比较的类型(如 String). 对要比较的对象应用这个函数, 然后对返回的键完成比较. 例如:
```java
Arrays.sort(people, Comparator.comparing(Person::getName));
```

可以把比较器于 thenComparing 方法串起来, 来处理比较结果相同的情况. 例如:
```java
Arrays.sort(people, 
    Coparator.comparing(Person::getLastName)
    .thenComparing(Person::getFirstName));
```
如果两个人的姓相同, 就会使用第二个比较器.

这些方法有很多变体形式. 可以为 comparing 和 thenComparing 方法提取的键指定一个比较器. 例如:
```java
Arrays.sort(people, Comparator.comparing(Person::getName, 
    (s, t) -> Integer.compare(s.length(), t.length())))
```
另外, comparing 和 thenComparing 方法都有变体形式, 可以避免 int, long 或 double 值的装箱.要完成前一个操作, 还有一种更容易的做法:
```java
Arrays.sort(people, Comparator.comparingInt(p -> p.getName.length()));
```
如果键函数可以返回 null, 可能就要用到 nullsFirst 和 nullsLast 适配器. 这些静态方法会修改现有的比较器, 从而在遇到 null 值时不会抛出异常, 而是将这个值标记为小于或大于正常值. nullsFirst 方法需要一个比较器, 在这里就是比较两个字符串的比较器. naturalOrder 方法可以为任何实现了 Comparable 的类建立一个比较器. 下面是一个完整的调用, 可以按可能为 null 的中名进行排序. 这里使用了一个静态导入 java.util.Comparator.*, 
```java
Arrays.sort(people, comparing(Person::getMiddleName, nullsFirst(naturalOrder())));
```
静态 reverseOrder 方法会提供自然顺序的逆序. 要让比较器逆序比较, 可以使用 reversed 实例方法. 例如 naturalOrder().reversed() 等同于 reverseOrder().

## 内部类

## 服务加载器

## 代理