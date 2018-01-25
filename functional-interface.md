# 函数式接口

  `java 8` 提供了函数式接口，该接口同样也被称为 *单例抽象方法接口*  (*SAM 接口*) . 如名字所示，接口中只允许一个抽象方法 . `java 8` 引入了 `@FunctionalInterface` 注解 , 
  `@FunctionalInterface` 会在你注解的接口违反了函数式接口的约定时提示一个编译错误 . 
    
  一个典型的`函数式接口`例子：
  
    @FunctionalInterface
    public interface MyFirstFunctionalInterface {
        public void firstWork();
    }
  
   不过请注意,即使没有写 `@FunctionalInterface` 这个注解 ，这个函数式接口依旧是合理的 . 因为它仅仅是用来通知编译器确保接口内部只有一个抽象方法 .

   同样,因为默认方法不是抽象方法,因此只要你喜欢,你就能自由的添加默认方法到你的函数式接口当中.
   另一个重要的点就是记住如果一个接口申明的抽象方法是重载了一个 `java.lang.Object` 的一个公共方法 , 同样不会计算在接口的抽象方法数目当中因为该接口的任意实现都会实现 `java.lang.Object` 或者在别处实现.
   
   一个合理而有效的函数式接口.
   
```java
@FunctionalInterface
public interface MyFirstFunctionalInterface{
    public void firstWork();    
    @Override
    public String toString();                //重写自 Object 类
    @Override
    public boolean equals(Object obj);        //重写自 Object 类
}
```
   
   原文出处：https://howtodoinjava.com/java-8/functional-interface-tutorial/
