# 字符串拼接
直到目前的JAVA7，我们可以通过向`String.split()`方法中传递参数的方式来分割一个字符串，然后把分割后的字符串列表以字符串数组的形式返回。但是，如果要连接字符串或通过使用它们之间的一些分隔符连接字符串来创建CSV，则必须遍历字符串列表或字符串数组，然后使用StringBuilder或StringBuffer对象连接这些字符串并最后得到CSV。
## 使用join()进行字符串连接
在Java 8中字符串拼接任务变得容易。你可以使用String.join（）方法，其中第一个参数是分隔符，然后可以传递多个字符串或一些具有字符串实例的Iterable实例作为第二个参数，它将返回CSV。示例：
```java
package java8features;
 
import java.time.ZoneId;
 
public class StringJoinDemo {
public static void main(String[] args){
String joined = String.join("/","usr","local","bin");
System.out.println(joined);
 
String ids = String.join(", ", ZoneId.getAvailableZoneIds());
System.out.println(ids);
}
}
```
输出：
```
usr/local/bin
Asia/Aden, America/Cuiaba, Etc/GMT+9, Etc/GMT+8.....
```
所以下次当你在使用java 8并且想拼接字符串时，记得你的工具箱中已经有一个方便的方法，请尽情使用它吧！

学习愉快！
