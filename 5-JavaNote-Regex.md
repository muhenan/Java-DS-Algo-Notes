[toc]

# Regex 正则表达式

正则表达式是一个非常有力的东西。

简单来说就是一个字符串，且这个字符串可以匹配一些其他的字符串。

regex 有很多应用比如：

* String.splite

## 一些符号

正则表达式中\s匹配任何空白字符，包括空格、制表符、换页符等等, 等价于[ \f\n\r\t\v]

- \f -> 匹配一个换页
- \n -> 匹配一个换行符
- \r -> 匹配一个回车符
- \t -> 匹配一个制表符
- \v -> 匹配一个垂直制表符

而“\s+”则表示匹配任意多个上面的字符。

另因为反斜杠在Java里是转义字符，所以在Java里，我们要这么用`“\\s+”`。

```java
List<String> words = Arrays.asList(s.split("\\s+"));
```
