# 内部迭代 vs. 外部迭代

## 外部迭代

直到 Java 7 为止，集合框架都一直依赖于外部迭代的概念。一个集合通过实现 `Iterable` 接口提供一个遍历自身所有元素的方法，即 `Iterator`，使用者则通过它来顺序地遍历集合中的元素。举例来说，如果我们希望将一组字符串全部变成大写，那么我们可能会这样实现:

```java
public class IterationExamples {
    public static void main(String[] args){
        List<String> alphabets = Arrays.asList(new String[]{"a","b","b","d"});

        for(String letter: alphabets){
            System.out.println(letter.toUpperCase());
        }
    }
}
```

或者我们也可以这样写：

```java
public class IterationExamples {
    public static void main(String[] args){
        List<String> alphabets = Arrays.asList(new String[]{"a","b","b","d"});

        Iterator<String> iterator = alphabets.listIterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next().toUpperCase());
        }
    }
}
```

以上两种写法采用了外部迭代的方式，这种方式足够直截了当，但也有下列问题：

1. Java 的 for-each 循环/迭代器天生就是顺序的，因此在处理集合元素时必须按照集合制定的顺序。

1. 它减弱了对控制流的优化，这些优化本能够能通过数据的乱序、并行、短路和懒惰等方法去实现。

## 内部迭代

有时我们需要 for-each 循环（顺序、依次）的特性，但是通常情况下这些特性有碍于性能的提升。通过将外部迭代替换为内部迭代，用户能够让库来控制遍历，从而只需要编写操作数据的代码。

上一个例子的内部迭代的实现为：

```java
public class IterationExamples {
    public static void main(String[] args){
        List<String> alphabets = Arrays.asList(new String[]{"a","b","b","d"});

        alphabets.forEach(l -> l.toUpperCase());
    }
}
```

> 外部迭代将“干什么”（将每个字符串变成大写）和“怎么做”（使用 for 循环/迭代器）混杂在一起，内部迭代则通过库来控制“怎么做”，而将“干什么”留给用户去实现，这样的方式能够提供很多潜在的好处：用户的代码能够变得更加简洁并聚焦于如何处理问题而不是通过什么方式去处理问题，同时也能够将复杂的性能优化代码移动到库的内部从而使所有用户受益。
