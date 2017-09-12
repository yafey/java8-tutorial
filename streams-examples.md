# Stream 示例

在上一篇文章,我们学习了关于["内部迭代VS外部迭代"](https://junicorn.gitbooks.io/java8-tutorial/internal-vs-external-iteration.html)
以及内部迭代是如何有助于业务逻辑，而不是如何迭代某些需要被应用逻辑的对象。
它有助于写出更整洁并且更具有可读性的代码。
在本章节，我们将会讨论一个新的抽象概念 —— ["Streams"](http://download.java.net/lambda/b78/docs/api/java/util/stream/Stream.html)

> 注意：Streams 可以被定义为元素集合从源支持聚合操作

这里的源指的是向 Stream 提供数据的集合或数组。Stream 保持原始数据的顺序，
并且聚合操作和批量操作可以使我们容易且清晰地对这些值表达常见的操作。

我将在以下部分讨论 Java 8 中 Streams 的各个方面：
> [Streams vs. Collections](#anchor1)  
  [不同的方式构建 Streams](#anchor2)  
  [将 Streams 转换成 Collections](#anchor3)  
  [Stream核心操作](#anchor4)  
      [中间操作](#anchor5)  
      [终端操作](#anchor6)  
  [短路操作](#anchor7)  
  [并行](#anchor8)

在进行之前，了解 Java8 中 Streams 被设计成大多数流操作只返回 Streams 这一点很重要。
这有助于我们创建各种流操作链。这被称为流水线式操作(pipelining)，我将在这篇文章中多次使用这个词，
所以请记住。

## <span id = "anchor1">Streams vs. Collections</span>

我们所有人都在YouTube或者其他此类网站上观看在线视频。当你开始观看视频时，
文件的一小部分将先被加载到计算机中并开始播放。在开始播放之前，你不需要下载完整的视频。
这被称为流式传输。我将尝试将这个概念与 Collections 相关联，并与 Streams 相区分。

在基础层面上，Collections 和 Streams 之间的区别与被计算的对象有关。
集合是一种内存中的数据结构，它保存数据结构当前具有的所有值 —— 必须先计算集合中的每个元素,
然后才能将其添加到集合中。流是概念上固定的数据结构，其中的元素需要根据需要计算，
这产生了巨大的编程效益。这个想法是用户只能从流中提取所需的值，
并且这些元素仅在需要时向用户生成 —— 对用户不可见。这是一种生产者-消费者关系的形式。

在java中，java.util.Stream表示为一个或多个可以执行操作的流。
流操作是中间或终端流程。当终端操作返回某种类型的结果时，中间操作将返回流本身，
以便可以将多个方法调用连接在一行。流是在源对象上创建的，例如像列表或集合（不支持maps）的java.util.Collection。
流操作可以串行执行或者并行执行。

基于上述几点，我们尝试列出Stream的各种特征：

* 不是数据结构
* 专为lambdas设计
* 不支持下标访问
* 可以容易地作为数组或列表输出
* 支持懒访问
* 可并行

## <span id = "anchor2">不同的方式构建 Streams</span>

以下是几个较常用的方式将集合构建成流。

### 1)使用 Stream.of(val1, val2, val3….)
```java
public class StreamBuilders {
     public static void main(String[] args){
         Stream<Integer> stream = Stream.of(1,2,3,4,5,6,7,8,9);
         stream.forEach(p -> System.out.println(p));
     }
}
```
### 2)使用 Stream.of(arrayOfElements)
```java
public class StreamBuilders {
     public static void main(String[] args){
         Stream<Integer> stream = Stream.of( new Integer[]{1,2,3,4,5,6,7,8,9} );
         stream.forEach(p -> System.out.println(p));
     }
}
```
### 3) 使用 someList.stream()
```java
public class StreamBuilders {
     public static void main(String[] args){
         List<Integer> list = new ArrayList<Integer>();
         for(int i = 1; i< 10; i++){
             list.add(i);
         }
         Stream<Integer> stream = list.stream();
         stream.forEach(p -> System.out.println(p));
     }
}
```
### 4) 使用 Stream.generate() 或者 Stream.iterate() functions
```java
public class StreamBuilders {
     public static void main(String[] args){
         Stream<Date> stream = Stream.generate(() -> { return new Date();});
         stream.forEach(p -> System.out.println(p));
     }
}
```
### 5) 使用 String chars 或者 String tokens
```java
public class StreamBuilders {
     public static void main(String[] args){
        IntStream stream = "12345_abcdefg".chars();
        stream.forEach(p -> System.out.println(p));

        //OR

        Stream<String> stream = Stream.of("A$B$C".split("\\$"));
        stream.forEach(p -> System.out.println(p));
     }
}
```
除了以上方法外还有其他方法使用 Stream ,构建者模式或者使用中间操作。
我们将会在不同的章节中了解它们。

## <span id = "anchor3">将 Streams 转换成 Collections</span> 

我本该说转换 Streams 到其他数据结构

> 注意：请不要说这是转变。这只是将 Stream 中的数据收集到 Collection 或数组中

### 1) 使用 stream.collect(Collectors.toList()) 将 Stream 转换成 List
```java
public class StreamBuilders {
     public static void main(String[] args){
         List<Integer> list = new ArrayList<Integer>();
         for(int i = 1; i< 10; i++){
             list.add(i);
         }
         Stream<Integer> stream = list.stream();
         List<Integer> evenNumbersList = stream.filter(i -> i%2 == 0).collect(Collectors.toList());
         System.out.print(evenNumbersList);
     }
}
```
### 2) 使用 stream.toArray(EntryType[]::new) 将 Stream 转换成数组
```java
public class StreamBuilders {
     public static void main(String[] args){
         List<Integer> list = new ArrayList<Integer>();
         for(int i = 1; i< 10; i++){
             list.add(i);
         }
         Stream<Integer> stream = list.stream();
         Integer[] evenNumbersArr = stream.filter(i -> i%2 == 0).toArray(Integer[]::new);
         System.out.print(evenNumbersArr);
     }
}
```
还有很多其他方式可以收集 Stream 进 Set、Map等结构。去熟悉下 Collectors 类并尽量记住他们。

## <span id = "anchor4">Stream核心操作</span> 
Stream 有一个很长且有用的功能列表。
我不会提到他们的全部，但我计划在这里列出最重要、你必须先了解的一些功能。

在前进之前，我们可以预先构建一个 String 集合。我们将在这个列表中建立一个用例，
这有助于我们更简单地去接触和理解。
```java
List<String> memberNames = new ArrayList<>();
memberNames.add("Amitabh");
memberNames.add("Shekhar");
memberNames.add("Aman");
memberNames.add("Rahul");
memberNames.add("Shahrukh");
memberNames.add("Salman");
memberNames.add("Yana");
memberNames.add("Lokesh");
```
这些核心方法分为以下两部分:

## <span id = "anchor5">中间操作</span>
A) filter()

Filter 接受一个断言条件去过滤流的所有元素。
这个流的操作是中间的，
这使得我们能够对结果调用另外一个流操作（例如forEach）
```java
memberNames.stream().filter((s) -> s.startsWith("A"))
                    .forEach(System.out::println);

Output:

Amitabh
Aman
```
B) map()

中间操作 map 通过给定的功能将每个元素转换成另一个对象。
以下示例将每个字符串转换成大写字符串。当然你也可以使用 map 将每个对象转换成另一种类型。
```java
memberNames.stream().filter((s) -> s.startsWith("A"))
                     .map(String::toUpperCase)
                     .forEach(System.out::println);

Output:

AMITABH
AMAN
```
C) sorted()

Sorted 是一个中间操作，返回流的排序视图。元素按照自然顺序进行排序，除非传递了一个自定义的比较器。
```java
memberNames.stream().sorted()
                    .map(String::toUpperCase)
                    .forEach(System.out::println);
Output:

AMAN
AMITABH
LOKESH
RAHUL
SALMAN
SHAHRUKH
SHEKHAR
YANA
```
请记住排序只会创建流的排序视图，而无需操纵后续集合的排序。
成员名称的顺序是不变的。

## <span id = "anchor6">终端操作</span>
终端操作返回某个类型的结构，而不是再次返回一个流。

A) forEach()

该方法有助于迭代流的所有元素，并对他们中的每一个执行一些操作。
改操作作为 lambda 表达式参数传递
```java
memberNames.forEach(System.out::println);
```
B) collect()

collect() 方法用于从一个 stream 中接收元素，并将他们存储在一个集合中，并通过参数分类。
```
List<String> memNamesInUppercase = memberNames.stream().sorted()
                            .map(String::toUpperCase)
                            .collect(Collectors.toList());

System.out.print(memNamesInUppercase);

Outpout: [AMAN, AMITABH, LOKESH, RAHUL, SALMAN, SHAHRUKH, SHEKHAR, YANA]
```
C) match()

可以使用各种匹配操作检查某个断言条件是否与流匹配。所有这些操作都是终端并返回一个 Boolean 结果。
```java
boolean matchedResult = memberNames.stream()
                    .anyMatch((s) -> s.startsWith("A"));

System.out.println(matchedResult);

matchedResult = memberNames.stream()
                    .allMatch((s) -> s.startsWith("A"));

System.out.println(matchedResult);

matchedResult = memberNames.stream()
                    .noneMatch((s) -> s.startsWith("A"));

System.out.println(matchedResult);

Output:

true
false
false
```
D) count()

Count 是一个终端操作，将流中的元素的数量返回一个 long 类型的值
```java
long totalMatched = memberNames.stream()
                    .filter((s) -> s.startsWith("A"))
                    .count();

System.out.println(totalMatched);

Output: 2
```
E) reduce()

该终端操作使用给定的函数来对流的元素进行操作。
返回值类型是 Optional。
```java
Optional<String> reduced = memberNames.stream()
                    .reduce((s1,s2) -> s1 + "#" + s2);

reduced.ifPresent(System.out::println);

Output: Amitabh#Shekhar#Aman#Rahul#Shahrukh#Salman#Yana#Lokesh
```

## <span id = "anchor7">短路操作</span>

虽然可以对满足断言条件的集合中的所有元素执行流操作，但每次在迭代期间遇到
匹配元素时，通常都希望中断操作。在外部迭代中，你可以使用if-else块。
在内部迭代中，有一些方法可以用于实现这样的目的。让我们看看两个这样方法的例子:

A) anyMatch()

一旦满足断言条件，该方法将返回 true。它不会处理任何更多的元素。
```java
boolean matched = memberNames.stream()
                    .anyMatch((s) -> s.startsWith("A"));

System.out.println(matched);

Output: true
```
B) findFirst()

该方法将从流返回第一个元素，然后不再处理任何元素。
```java
String firstMatchedName = memberNames.stream()
                .filter((s) -> s.startsWith("L"))
                .findFirst().get();

System.out.println(firstMatchedName);

Output: Lokesh
```

## <span id = "anchor8">并行</span>
随着Java SE 7中添加了 Fork / Join 框架，我们可以在应用程序中实施并行操作的高效机制。
但实施这个框架本身就是一个复杂的任务，如果没有做好，这将是复杂的多线程错误的源头，有可能使应用程序崩溃。
随着内部迭代的引入，我们有了另一个并行完成操作的可能。

要启用并行，你只需要创建一个并行流，而不是串行流。
并给你惊喜的是，这真的非常简单。
在任何上面列出的流示例中，任何时候你想要在并行核心中使用多个线程的特点作业，
你只需调用 parallelStream() 方法而不是 stream()方法。
```java
public class StreamBuilders {
     public static void main(String[] args){
        List<Integer> list = new ArrayList<Integer>();
         for(int i = 1; i< 10; i++){
             list.add(i);
         }
         //Here creating a parallel stream
         Stream<Integer> stream = list.parallelStream();
         Integer[] evenNumbersArr = stream.filter(i -> i%2 == 0).toArray(Integer[]::new);
         System.out.print(evenNumbersArr);
     }
}
```
这项工作的关键驱动因素是让开发人员更易于使用并行性。
虽然 Java 平台已经提供了对并发和并行的强大支持，但开发人员在根据需要
将代码从串行迁移到并行时面临着不必要的障碍。
因此，重要的是鼓励形成串行与友好的并行相结合的风格。
这促进了将焦点转移到描述应该执行什么计算，而不是如何实现执行的步骤。
同样重要的是，要使得并行更容易但不会使其失去可视化之间的平衡。
使并行透明化将引入不确定性以及用户可能不期望的数据竞争的可能性。

这就是我想要分享的关于 Java 8中引入的 Stream 概念的基础知识。我将在后面的章节中讨论与 Streams 有关的其它事项。

学习快乐 ！！