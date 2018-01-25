# Regex as Predicate
学习如何将正则表达式编译到`java.util.function.Predicate`中。当您想要对匹配的符号执行某些操作时，这可能很有用。

## 将正则表达式转换为断言
我有不同域名的电子邮件列表，我只想对域名为`example.com`的电子邮件`ID` 进行一些操作。

现在使用`Pattern.compile().asPredicate()`方法从编译的正则表达式获取断言。该断言可以与`lambda`表达式一起使用，以将其应用到每个符号上。
```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Predicate;
import java.util.regex.Pattern;
import java.util.stream.Collectors;
 
public class RegexPredicateExample {
    public static void main(String[] args) {
        // Compile regex as predicate
        Predicate<String> emailFilter = Pattern
                                        .compile("^(.+)@example.com$")
                                        .asPredicate();
 
        // Input list
        List<String> emails = Arrays.asList("alex@example.com", "bob@yahoo.com", 
                            "cat@google.com", "david@example.com");
 
        // Apply predicate filter
        List<String> desiredEmails = emails
                                    .stream()
                                    .filter(emailFilter)
                                    .collect(Collectors.<String>toList());
 
        // Now perform desired operation
        desiredEmails.forEach(System.out::println);
    }
}
```
输出：
```
alex@example.com
david@example.com
```
## 使用`Pattern.matcher()`使用正则表达式

如果要使用旧的`Pattern.matcher()`，那么可以使用下面的代码模板。

```java
public static void main(String[] args) 
{
     
    Pattern pattern = Pattern.compile("^(.+)@example.com$");
     
    // Input list
    List<String> emails = Arrays.asList("alex@example.com", "bob@yahoo.com", 
                        "cat@google.com", "david@example.com");
      
    for(String email : emails)
    {
        Matcher matcher = pattern.matcher(email);
         
        if(matcher.matches()) 
        {
            System.out.println(email);
        }
    }
}
```
输出：
```
alex@example.com
david@example.com
```

欢迎在评论部分告诉我你的疑问。

学习愉快！
