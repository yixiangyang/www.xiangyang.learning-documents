### Java8新特性 

1. Lambda表达式

2. 新的日期API

3. 引入Optional

4. 使用Base64

5. 接口的默认方法和静态方法

6. 新增方法引用格式

7. 新增Stream类

8. 注解相关的改变

9. 支持并行（parallel）数组

10. 对并发类（Concurrency）的扩展

#### 一、Lambda表达式

Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。

Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。

使用 Lambda 表达式可以使代码变的更加简洁紧凑。

Lambda表达式语法:

```java
(parameters) -> expression
或
(parameters) ->{ statements; }
```

Lambda表达式的特征:

- 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
- 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
- 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
- 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指明表达式返回了一个数值。

使用Lambda注意以下两点：

- Lambda表达式主要用来定义行内的方法类型接口，

- Lambda表达式免去了使用匿名方法的麻烦，并且给与Java简单但是强大的函数化的编程能力。

变量作用域：
- Lambda表达式只能引用标记了final的外层局部变量，这就是说不能再Lambda内部修改定义在域外的局部变量，否则会编译错误
- Lambda 表达式的局部变量可以不用声明为final，但是必须不可被后面的代码修改(即隐形的具有final的语句)
- Lambda 表达式当中不允许声明一个与局部变量名同名的参数或局部变量


### 二、日期时间API 

Java 8通过发布新的Date-Time API (JSR 310)来进一步加强对日期与时间的处理。

在旧版的 Java 中，日期时间 API 存在诸多问题，其中有：

- **非线程安全** − java.util.Date 是非线程安全的，所有的日期类都是可变的，这是Java日期类最大的问题之一。
- **设计很差** − Java的日期/时间类的定义并不一致，在java.util和java.sql的包中都有日期类，此外用于格式化和解析的类在java.text包中定义。java.util.Date同时包含日期和时间，而java.sql.Date仅包含日期，将其纳入java.sql包并不合理。另外这两个类都有相同的名字，这本身就是一个非常糟糕的设计。
- **时区处理麻烦** − 日期类并不提供国际化，没有时区支持，因此Java引入了java.util.Calendar和java.util.TimeZone类，但他们同样存在上述所有的问题。

Java 8 在 **java.time** 包下提供了很多新的 API。以下为两个比较重要的 API：

- **Local(本地)** − 简化了日期时间的处理，没有时区的问题。
- **Zoned(时区)** − 通过制定的时区处理日期时间。

新的java.time包涵盖了所有处理日期，时间，日期/时间，时区，时刻（instants），过程（during）与时钟（clock）的操作。

### 三、Optional类

Optional 类是一个可以为null的容器对象。如果值存在则isPresent()方法会返回true，调用get()方法会返回该对象。

Optional 是个容器：它可以保存类型T的值，或者仅仅保存null。Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

Optional 类的引入很好的解决空指针异常。

### 四、Base64

Base64编码的作用：

由于某些系统中只能使用ASCII字符。Base64就是用来将非ASCII字符的数据转换成ASCII字符的一种方法。Base64其实不是安全领域下的加解密算法，而是一种编码，也就是说，它是可以被翻译回原来的样子。它并不是一种加密过程。所以Base64只能算是一个编码算法，对数据内容进行编码来适合传输。虽然Base64编码过后原文也编程不能看到的字符格式，但是这种方式很初级，很简单。

使用Base64编码的原因：

1. Base64是网络上最常见的用于传输8bit字节代码的编码方式之一。有时我们需要把二进制数据编码为适合URL中的形式，这时采用Base64编码具有不可读性，既所编码的数据不会被人直接看出。

2. 用于在http环境下传递较长的标识信息。

### 五、接口的默认方法和静态方法

简单说，默认方法就是接口可以有实现方法，而且不需要实现类去实现其方法。

我们只需要在方法名前价格default关键字即可实现默认方法。

 	为什么要实现这个特性？

​	首先，之前的接口是个双刃剑，好处是面向抽象而不是面向具体编程，缺陷是，当需要修改接口的时候，需要修改全部实现该接口的类，目前的Java8之前的集合框架没有foreach方法，通常能想到的解决办法是在JDK里给相关的接口添加新的方法及实现。然而，对于已经发布的版本，是没有办法再给接口添加新方法的同时不影响已有的实现。所以引进的默认方法他们是为了解决接口的修改与现有的实现不兼容的问题。

一个接口有多个默认方法， 考虑这样的情况，一个类实现了多个接口，且这些接口有相同的默认方法，以下实例说明了这种情况的解决方法： 

```java
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
}
 
public interface FourWheeler {
   default void print(){
      System.out.println("我是一辆四轮车!");
   }
}
```

 第一个解决方案是创建自己的默认方法，来覆盖重写接口的默认方法： 

```java
public class Car implements Vehicle, FourWheeler {
   default void print(){
      System.out.println("我是一辆四轮汽车!");
   }
}
```

第二种解决方案可以使用 super 来调用指定接口的默认方法：

```java
 public class Car implements Vehicle, FourWheeler {
   public void print(){
      Vehicle.super.print();
   }
}
```

**静态默认方法：** 

 Java 8 的另一个特性是接口可以声明（并且可以提供实现）静态方法。例如： 

```
public interface Vehicle {
   default void print(){
      System.out.println("我是一辆车!");
   }
    // 静态方法
   static void blowHorn(){
      System.out.println("按喇叭!!!");
   }
}
```

### 六、方法引用

方法引用可以通过方法的名字来指向一个方法。

方法引用可以使用语言的构造更紧凑简洁，减少冗余代码。

方法引用使用一对冒号 : : 。

### 七、Stream

Java8 API添加了一个新的抽象称为流Stream,可以让你以一种声明的方式处理数据库。

Stream 使用一种类似用SQL语句从数据库查询数据的直观方式来提供一种对Java 集合运算和表达的高级抽象。

Stream API可以极大提高Java程序员的生产力，让程序员写出高效率、干净简洁的代码。

这种风格将要处理的元素集合看作一种流，流在管道中传输，并且可以在管道的节点上进行处理，比如筛选，排序，聚合等。元素流在管道中经过中间操作的处理 最后由最终操作（terminal operation）得到前面处理的结果。

**什么是Stream?**

Stream 是一个来自数据源的元素队列并支持聚合操作

- 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
- **数据源** 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
- **聚合操作** 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。

和以前的Collection操作不同， Stream操作还有两个基础的特征：

- **Pipelining**: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
- **内部迭代**： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

在 Java 8 中, 集合接口有两个方法来生成流：

​    stream() − 为集合创建串行流。
​    parallelStream() − 为集合创建并行流。

```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

**forEach**

Stream 提供了新的方法forEach来迭代流中每个数据。一下代码使用forEach输出了10个随机数：

```java
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```

**Filder(过滤)**

过滤通过一个predicate接口来过滤并只保留符合条件的元素，该操作属于**中间操作**，所以我们可以在过滤后的结果来应用其他Stream操作（比如forEach）。forEach需要一个函数来对过滤后的元素依次执行。forEach是一个最终操作，所以我们不能在forEach之后来执行其他Stream操作。

```java
List<String> stringList = new ArrayList<>();
stringList.add("ddd2");
stringList.add("aaa2");
stringList.add("bbb1");
stringList.add("aaa1");
stringList.add("bbb3");
stringList.add("ccc");
stringList.add("bbb2");
stringList.add("ddd1");

// 测试 Filter(过滤)
stringList.stream()
    .filter((s) -> s.startsWith("a"))
     .forEach(System.out::println);//aaa2 aaa1
```

forEach 是为 Lambda 而设计的，保持了最紧凑的风格。而且 Lambda 表达式本身是可以重用的，非常方便

**map（映射）**

map方法用于映射每个元素到对应的结果，以下代码片段使用map输出了元素对应的平方数：

```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
// 获取对应的平方数
List<Integer> squaresList = numbers.stream().map( i -> i*i).distinct().collect(Collectors.toList());
```

**limit**

limit方法用于获取指定数量的流。以下代码片段使用limit方法打印10条数据：

```java
Random random = new Random();
random.ints().limit(10).forEach(System.out::println);
```

**sorted（排序）**

sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法对输出的 10 个随机数进行排序：

```java
Random random = new Random();
random.ints().limit(10).sorted().forEach(System.out::println);
```

并行(parallel)程序

parallelStream 是流并行处理程序的代替方法。以下实例我们使用 parallelStream 来输出空字符串的数量：

```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
// 获取空字符串的数量
int count = strings.parallelStream().filter(string -> string.isEmpty()).count();
```

**Collectors**

Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串：

```java
List<String>strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
 
System.out.println("筛选列表: " + filtered);
String mergedString = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.joining(", "));
System.out.println("合并字符串: " + mergedString);
```

**统计**

另外，一些产生统计结果的收集器也非常有用。它们主要用于int、double、long等基本类型上，它们可以用来产生类似如下的统计结果。

```java
List<Integer> numbers = Arrays.asList(3, 2, 2, 3, 7, 3, 5);
 
IntSummaryStatistics stats = numbers.stream().mapToInt((x) -> x).summaryStatistics();
 
System.out.println("列表中最大的数 : " + stats.getMax());
System.out.println("列表中最小的数 : " + stats.getMin());
System.out.println("所有数之和 : " + stats.getSum());
System.out.println("平均数 : " + stats.getAverage());
```

**Reduce(规约)**

这是一个 **最终操作** ，允许通过指定的函数来讲stream中的多个元素规约为一个元素，规约后的结果是通过Optional 接口表示的：

```
        //测试 Reduce (规约)操作
        Optional<String> reduced =
                stringList
                        .stream()
                        .sorted()
                        .reduce((s1, s2) -> s1 + "#" + s2);

        reduced.ifPresent(System.out::println);//aaa1#aaa2#bbb1#bbb2#bbb3#ccc#ddd1#ddd2
```

**译者注：** 这个方法的主要作用是把 Stream 元素组合起来。它提供一个起始值（种子），然后依照运算规则（BinaryOperator），和前面 Stream 的第一个、第二个、第 n 个元素组合。从这个意义上说，字符串拼接、数值的 sum、min、max、average 都是特殊的 reduce。例如 Stream 的 sum 就相当于`Integer sum = integers.reduce(0, (a, b) -> a+b);`也有没有起始值的情况，这时会把 Stream 的前面两个元素组合起来，返回的是 Optional。

```
// 字符串连接，concat = "ABCD"
String concat = Stream.of("A", "B", "C", "D").reduce("", String::concat); 
// 求最小值，minValue = -3.0
double minValue = Stream.of(-1.5, 1.0, -3.0, -2.0).reduce(Double.MAX_VALUE, Double::min); 
// 求和，sumValue = 10, 有起始值
int sumValue = Stream.of(1, 2, 3, 4).reduce(0, Integer::sum);
// 求和，sumValue = 10, 无起始值
sumValue = Stream.of(1, 2, 3, 4).reduce(Integer::sum).get();
// 过滤，字符串连接，concat = "ace"
concat = Stream.of("a", "B", "c", "D", "e", "F").
 filter(x -> x.compareTo("Z") > 0).
 reduce("", String::concat);
```

上面代码例如第一个示例的 reduce()，第一个参数（空白字符）即为起始值，第二个参数（String::concat）为 BinaryOperator。这类有起始值的 reduce() 都返回具体的对象。而对于第四个示例没有起始值的 reduce()，由于可能没有足够的元素，返回的是 Optional，请留意这个区别

### 八、注解相关

1）可以进行重复注解

自从Java 5引入了注解机制，这一特性就变得非常流行并且广为使用。然而，使用注解的一个限制是相同的注解在同一位置只能声明一次，不能声明多次。Java 8打破了这条规则，引入了重复注解机制，这样相同的注解可以在同一地方声明多次。

重复注解机制本身必须用@Repeatable注解。事实上，这并不是语言层面上的改变，更多的是编译器的技巧，底层的原理保持不变。

（2）扩展注解的支持

Java 8扩展了注解的上下文。现在几乎可以为任何东西添加注解：局部变量、泛型类、父类与接口的实现，就连方法的异常也能添加注解。

### 九、 **并行（parallel）数组** 

  Java 8增加了大量的新方法来对数组进行并行处理。可以说，最重要的是parallelSort()方法，因为它可以在多核机器上极大提高数组排序的速度。

### 十、并发

  在新增Stream机制与lambda的基础之上，在java.util.concurrent.ConcurrentHashMap中加入了一些新方法来支持聚集操作。同时也在java.util.concurrent.ForkJoinPool类中加入了一些新方法来支持共有资源池（common pool）（请查看我们关于Java 并发的免费课程）。

新增的java.util.concurrent.locks.StampedLock类提供一直基于容量的锁，这种锁有三个模型来控制读写操作（它被认为是不太有名的java.util.concurrent.locks.ReadWriteLock类的替代者）。

在java.util.concurrent.atomic包中还增加了下面这些类：

1. DoubleAccumulator
2. DoubleAdder 
3. LongAccumulator 
4. LongAdder
