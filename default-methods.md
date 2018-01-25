# 默认方法
`java 8` 允许你在接口中添加非抽象方法 , 这些方法必须定义为 `default` 方法 , `java 8` 引入默认方法是为了使用 `lambda` 函数表达式 .

`default` 方法使您能够向库的接口添加新功能，并确保与旧版本的这些接口编写的代码的二进制兼容性 。

用一个例子去理解：
    
        public interface Moveable {
        		default void move(){
        			System.out.println("I am moving");
        		}
        }
`Moveable` 接口定义了一个 `move()` 方法并且提供了默认的实现 . 如果任意一个 `class` 实现这个
接口都没有必要去实现这个 `move()` 方法，能够直接使用 `instance.move()` 进行调用 .

eg:
	
	public class Animal implements Moveable{
		public static void main(String[] args){
			Animal tiger = new Animal();
			tiger.move();
		}
	}
	Output: I am moving
	
如果该 `class` 想要自定义一个 `move()` 也能提供自定义的实现去覆写这个 `move` 方法 .

原文 ：https://howtodoinjava.com/java-8-tutorial/

read more ： https://howtodoinjava.com/java-8/java-8-tutorial-internal-vs-external-iteration/
