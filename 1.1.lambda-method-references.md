# 方法引用

在 Java 8 中，你可以使用 `class::methodName` 语法引用类或对象的方法。让我们来学习一下 Java 8 中不同类型的方法引用。

## 方法引用类型 - 快速预览

Java8 中包含了四种类型的方法引用。

| 方法引用       | 描述    |  例子  |
| --------   | -----   | ---- |
| 静态方法引用        | 用于引用类的静态方法     |   `Math::max` 相当于 `Math.max(x,y)`    |
| 从对象中引用实例方法       | 使用对象的引用来调用实例方法    |   `System.out::println` 相当于 ` System.out.println(x)`    |
| 从类中引用实例方法       | 在上下文提供的对象的引用上调用实例方法     |   `String::length` 相当于 `str.length()`    |
| 引用构造函数	| 引用构造函数		|	`ArrayList::new` 相当于 `new ArrayList()`|

## 引用静态方法 - Class::staticMethodName

一个使用 `Math.max()` 静态方法的例子。

```java
List<Integer> integers = Arrays.asList(1,12,433,5);
         
Optional<Integer> max = integers.stream().reduce( Math::max ); 
 
max.ifPresent(value -> System.out.println(value)); 
```

输出：

```java
433
```

## 从对象中引用实例方法 - ClassInstance::instanceMethodName

在上面的例子中，我们使用了 `System.out.println(value)` 打印集合中的最大值，我们可以使用```System.out::println```打印这个值。

```java
List<Integer> integers = Arrays.asList(1,12,433,5);        
Optional<Integer> max = integers.stream().reduce( Math::max ); 
max.ifPresent( System.out::println );
```

输出:

```bash
433
```

## 引用特定类型的实例方法  - Class::instanceMethodName

在这个例子中 `s1.compareTo(s2)` 被简写为 `String::compareTo`。

```java
List<String> strings = Arrays
        .asList("how", "to", "do", "in", "java", "dot", "com");
 
List<String> sorted = strings
        .stream()
        .sorted((s1, s2) -> s1.compareTo(s2))
        .collect(Collectors.toList());
 
System.out.println(sorted);
 
List<String> sortedAlt = strings
        .stream()
        .sorted(String::compareTo)
        .collect(Collectors.toList());
 
System.out.println(sortedAlt);
```

输出：

```bash
[com，do，dot，how，in，java，to] 
[com，do，dot，how，in，java，to]
```

## 引用构造函数 - Class::new

使用 `lambda` 表达式修改第一个例子中的方法，可以非常简单的创建一个从1到100的集合(不包含100)。创建一个新的 `ArrayList` 实例，我们可以使用 `ArrayList::new`。

```java
List<Integer> integers = IntStream
                .range(1, 100)
                .boxed()
                .collect(Collectors.toCollection( ArrayList::new ));
 
Optional<Integer> max = integers.stream().reduce(Math::max); 
 
max.ifPresent(System.out::println); 
```

输出：

```bash
99
```

这是Java8 lambda 增强功能中的四种方法引用。

学习愉快！

原文出处：https://howtodoinjava.com/java-8/lambda-method-references-example
