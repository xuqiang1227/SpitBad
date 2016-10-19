# 学习JAVA
## Java 8 Optional类深度解析


```java
	//建立一个为空的数据
	Optional<Integer> op = Optional.ofNullable(null);
    //如果为空，自动赋值为2
    System.out.println(op.orElse(2));
    //判断是否为空，输出boolean值，false为空
    System.out.println(op.isPresent());
```
[代码片段](https://github.com/wenisy/javaLearning/blob/master/src/com/thoughtworks/helloWorld/HelloWorld.java)

[Optional](http://www.importnew.com/6675.html)

