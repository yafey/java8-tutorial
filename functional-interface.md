# 函数式接口

  java8 提供了函数式接口，该接口同样也被成为 *单例抽象方法接口*  (SAM 接口) . 正如名字所示，接口内部只允许存在一个抽象方法 . 同时引入了名为 `@FunctionalInterface` 的注解 , 
  该注解会在你违反了函数式接口的约定之时, 提示一个编译错误.
    
  一个典型的`函数式接口`如下：
  
    @FunctionalInterface
    public interface MyFirstFunctionalInterface {
        public void firstWork();
    }
  
   不过请注意,即使没有写 `@FunctionalInterface` 这个注解 ，这个函数式接口依旧是合理的 . 因为它仅仅是用来通知编译器确保接口内部只有一个抽象方法 .
   
   同样,因为默认方法不是抽象方法,因此只要你喜欢,你就能自由的添加默认方法到你的函数式接口当中.
   另一个重要的点就是记住如果一个接口申明的抽象方法是重载了一个 `java.lang.Object` 的一个公共方法 , 同样不会计算在接口的抽象方法数目当中因为该接口的任意实现都会实现 `java.lang.Object` 或者在别处实现.
   
   
   如下所示便是一个非常合理的函数式接口.
   
        @FunctionalInterface
        public interface MyFirstFunctionalInterface
        {
        public void firstWork();    
        @Override
        public String toString();                //重载自 Object 类
        @Override
        public boolean equals(Object obj);        //重载自 Object 类
    }
   以上就是基本的函数式接口的介绍
   
   原文出处：https://howtodoinjava.com/java-8/functional-interface-tutorial/