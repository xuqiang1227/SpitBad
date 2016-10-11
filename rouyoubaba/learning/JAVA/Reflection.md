# [反射] 学习中~
```java
//第一种方式：  
Class c1 = Class.forName("Employee");  
//第二种方式：  
//java中每个类型都有class 属性.  
Class c2 = Employee.class;  
   
//第三种方式：  
//java语言中任何一个java对象都有getClass 方法  
Employeee = new Employee();  
Class c3 = e.getClass(); //c3是运行时类 (e的运行时类是Employee) 
```
