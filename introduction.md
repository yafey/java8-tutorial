# Java8 介绍

> 原文出处：http://howtodoinjava.com/java-8-tutorial

Java8 在 [2014年初](https://blogs.oracle.com/thejavatutorials/jdk-8-is-released) 发布，在 Java8 中大家讨论最多的特性是 lambda 表达式。
它还有许多重要的功能，像默认方法、Stream API、新的日期时间API。让我们通过示例来了解这些新功能。

## Lambda 表达式

有许多使用过高级编程语言（比如Scala）的人不知道 lambda 表达式。在编程中，lambda 表达式（或者函数）只是一个匿名函数，
即一个没有名称也没有标识符的函数。它们都被写在你需要使用的地方，通常作为其他函数的参数。

lambda 表达式的基本语法是：

```bash
either
(parameters) -> expression
or
(parameters) -> { statements; }
or
() -> expression
```

一个典型的 `lambda` 表达式如下所示：

```java
(x, y) -> x + y  //这个函数接受两个参数并返回它们的和。
```

请注意根据 `x` 和 `y` 的类型，方法可能会在多个地方使用。参数可以匹配到 `int` 类型整数或者字符串类型。
根据上下文，它将两个整数相加或者两个字符串拼接。

#### 编写 lambda 表达式的规则

1. lambda表达式可以有零个、一个或多个参数
2. 可以显式声明参数的类型，也可以从上下文推断参数的类型
3. 多个参数必须包含在括号中，并用逗号分隔，空括号用于表示零个参数
4. 当只有一个参数时，如果推断它的类型，可以不使用括号。如 `a -> return a * a`
5. lambda表达式的函数体可以包含零个，一个或多个语句。
6. 如果lambda表达式的函数体只有一行，则可以不用大括号，匿名函数的返回类型与函数体表达式的返回类型相同。当函数体中大于一行代码则需要用大括号包含。

> 阅读更多: [Java 8 Lambda 表达式](complete-lambda-expressions.md)

## 函数式接口（Functional Interface）

`Functional Interface` 也被称为单一抽象方法的接口（SAM接口）。就像它的名字一样，
它 **只能有一个抽象方法** 。在 Java8 种引入了一个注解 `@FunctionalInterface`，
当你在某个接口使用了该注解，接口违反了函数式接口的规定时，会产生编译报错。

一个函数式接口的示例：

```java
@FunctionalInterface
public interface MyFirstFunctionalInterface {
    public void firstWork();
}
```

请注意，即使省略 `@FunctionalInterface` 注解，函数式接口也是有效的。它仅用于通知编译器在接口内强制定义单个抽象方法。

此外，由于默认方法不是抽象的，您可以随意添加默认方法到你的接口中，只要你喜欢。

另一个要记住的要点是，如果一个接口中定义的函数是重写了 `Object` 类的方法，
那么这个方法不会被计入接口的抽象方法，因为接口的任何实现类都会继承自 `Object`。
例如，下面是完全有效的函数式接口。

```java
@FunctionalInterface
public interface MyFirstFunctionalInterface{

    public void firstWork();

    @Override
    public String toString();                 //重写toString

    @Override
    public boolean equals(Object obj);        //重写equals
}
```

> 阅读更多：[函数式接口](functional-interface.md)

## 默认方法（Default Methods）

Java8 允许你在接口中添加非抽象方法。这些方法必须被声明为 **默认方法**。java8中引入了默认方法来启用 lambda 表达式的功能。

默认方法可以让你在接口中添加新功能，并确保与旧版本的这些接口编写的代码的二进制兼容。

我们以一个例子来理解：

```java
public interface Moveable {
    default void move(){
        System.out.println("I am moving");
    }
}
```

`Moveable` 接口定义了一个方法 `move` 并且提供了默认实现。如果任何类实现了这个接口，
那么它可以不实现 `move` 方法。子类可以直接调用 `move` 方法，例如：

```java
public class Animal implements Moveable{
    public static void main(String[] args){
        Animal tiger = new Animal();
        tiger.move();
    }
}

输出: I am moving
```

如果子类愿意定制 `move` 方法的行为，那么它可以提供它自己的自定义实现并覆盖该方法。

> 阅读更多：[默认方法](default-methods.md)

## Streams

另一个重大变化是引入了 **Streams API**，它提供了一种可以用各种方式处理一组数据的机制，包括过滤、转换或任何其他可能对应用程序有用的方式。

Java8中的`Streams API`支持不同类型的迭代，你只需定义要处理的项目集合，对每个项目执行的操作以及要存储操作后的输出。

下面是一个 `Stream API` 的例子。在这个示例中，`items` 是 `String` 值的集合，你需要筛选出一些不是 `prefix` 开头的项目。

```java
List<String> items;
String prefix = "base";
List<String> filteredList = items.stream()
                    .filter(e -> (!e.startsWith(prefix)))
                    .collect(Collectors.toList());
```

这里的 `items.stream()` 表示我们希望使用 `Streams API` 处理集合中的数据。

> 阅读更多：[内部迭代 VS 外部迭代](internal-vs-external-iteration.md)

## 时间日期API的改变

新的日期和时间API（JSR-310）也称为*ThreeTen*，它们简单地改变了在java应用程序中处理日期的方式。

### 日期相关

`Date` 类甚至已经过时了。用于替换 `Date` 类的新类是 `LocalDate`，`LocalTime` 和 `LocalDateTime`。

1. `LocalDate` 表示没有时区的日期
2. `LocalTime` 表示没有时区的时间
3. `LocalDateTime` 表示没有时区的日期时间

如果要使用带有时区信息的日期功能，Java8为您提供了类似于上述的3个类，
即 `OffsetDate`，`OffsetTime` 和 `OffsetDateTime`。
时区偏移可以用 `05:30` 或 `Europe/Paris` 格式表示。
这是通过使用另一个类 `ZoneId` 来完成的。

### 时间戳和周期

为了表示任何时刻的具体时间戳，需要使用的类是 `Instant`。
`Instant` 类代表一个即时的时间精度为纳秒。即时操作包括与另一个时间的比较，添加或减去一个周期的时间。

```java
Instant instant = Instant.now();
Instant instant1 = instant.plus(Duration.ofMillis(5000));
Instant instant2 = instant.minus(Duration.ofMillis(5000));
Instant instant3 = instant.minusSeconds(10);
```

`Duration` 类是Java语言中首次引入的概念。它用来代表两个时间戳之间的时差。

```java
Duration duration = Duration.ofMillis(5000);
duration = Duration.ofSeconds(60);
duration = Duration.ofMinutes(10);
```

`Duration` 一般处理小的时间单位，如毫秒，秒，分钟和小时。它们在程序代码中的交互更多一些。
要与人交互，你需要操作 **更长的时间**，这些更长的时间在 `Period` 类中可以体现。

```java
Period period = Period.ofDays(6);
period = Period.ofMonths(6);
period = Period.between(LocalDate.now(), LocalDate.now().plusDays(60));
```

> 阅读更多：[日期时间API的改变](date-and-time-api-changes.md)

