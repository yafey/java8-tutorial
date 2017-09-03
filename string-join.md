# 字符串拼接

从Java7到目前为止，我们可以通过向 `String.split()` 方法中传递参数的方式来分割一个字符串，然后把分割后的字符串列表以数组的形式返回。
但是，如果需要连接字符串或者通过分隔符连接的字符串来创建一个CSV文件，则必须遍历字符串列表或数组，
然后使用 `StringBuilder` 或 `StringBuffer` 对象拼接这些字符串，最后得到 `CSV`。

## 使用 `join()` 进行字符串连接（CSV创建）

在 Java8 中使得字符串拼接任务变得容易。现在你可以使用 `String.join()` 方法，
其中**第一个参数是分隔符**，然后可以传递多个字符串或实现了 `Iterable` 接口的实例作为第二个参数，下面的示例将返回 `CSV`：

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

```bash
usr/local/bin
Asia/Aden, America/Cuiaba, Etc/GMT+9, Etc/GMT+8.....
```

所以下次当你在使用 java8 并且想拼接字符串时，记得你的工具箱中已经有一个方便的方法，请尽情使用它吧！

学习愉快！
