# Optional

我们在写程序时一定都遇到过 ```空指针异常```。它通常发生在你试图去引用一个没有被初始化，被初始化为空或者没有指向任何实例的对象的情况下。***空仅仅只是意味着没有值***，最有可能的是，罗马人是唯一没有遇到过空，从 1 开始计数 1,2,3...（没有0） 的人，或许，他们没有模拟市场上没有苹果的情况。[:-)]

“我叫它价值十亿美元的错误” —— [Sir C. A. R. Hoare](https://en.wikipedia.org/wiki/Tony_Hoare)，他在提出空引用这个概念时说。

在本文中，我将讨论 [java 8 新特性](https://howtodoinjava.com/category/java-8/) 中 [Optional](https://docs.oracle.com/javase/8/docs/api/java/util/Optional.html) 的具体用法。为了阐明诸多概念之间的异同，本文将被划分为多个部分一一讲解。	

```讨论的要点```