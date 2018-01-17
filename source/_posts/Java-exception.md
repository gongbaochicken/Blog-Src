---
title: Java exception
date: 2015-06-26 23:02:17
tags: [Java]
categories:
  - Java
---
异常(Exception)，Ex前缀在中文大义就是排他、否定、外部，从字面上理解Exception就是和常态不一样的情况，也就是通常我们不希望出现的情况。人生之不如意，十之八九，比如吃烧饼烫后脑勺，交卷子的前一秒发现试卷有反面。

下面来谈谈Java中的程序异常和异常处理。程序也是生活的体现，99%的程序员都无法保证大型程序完全没有异常。所以，异常处理非常重要。对于异常的处理，可以将异常提示给程序员或者用户，让异常的程序以适当的方式继续进行或者终止，并让资源回滚。根据《Think in Java》Bruce写道的那样：*** “Java中的异常处理的目的在于通过少于目前数量的代码来简化大型、可靠的程序生成，并通过这种方式使你更自信：你的应用中没有你没有处理的错误。” ***
<!--more-->

##哪些异常？
###体系结构：
在吸血鬼(Vampire)、狼人(werwolf)的电视剧中，他们有共同的祖先，一般称为始祖。鬼知道他具体叫什么呢，反正就是一个父类。

![Throwable始祖](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150620-1.jpg)

在Java中，所有的异常，也有一个类似始祖的家伙。他们这些异常都继承自Throwable类。Throwable子孙很多，但Throwable的儿子只有有两个，一个叫Error,一个叫Exception。

![Throwable始祖的继承者们](http://7xjjbh.com1.z0.glb.clouddn.com/QQ20150620-2.jpg)

Error出现一般都是编译和系统错误，比如VirtualMachineError, ThreadDeadlock。一旦Error出现，程序就是彻底Crash的，所谓硬伤是也啊。作为程序员一般不用关心它们。

Exception是程序员讨论的重点。Exception又有3个子类，一个子类，称为“RuntimeException”,也被称作“非检查异常(unchecked exception)”；另一类，被称为“检查异常”;最后一类是“自定义异常”。

###RuntimeException
比如常见的下面四种非检查：NullPointerException, ArrayIndexOutOfBoundsException, ClassCastException, ArithmeticExcetpion.

* NullPointerException是空指针异常，典型错误代表例如：

```java
String str = null;
System.out.print(str.length());
```

* ArrayIndexOutOfBoundsException是数组越界，典型错误代表例如：

```java
int[] array = {1, 2, 3};
for(int i =0; i <= 3; i++){
	System.out.print(array[i]);
}
```

* ClassCastException是类型转换，典型错误代表例如：

```
class Animal{
}
class Dog extends Animal{
}
class Cat extends Animal{
}
public class Test{
	public static void main(String[] args) {
		Animal a = new Dog();
		Animal b = new Cat();
		Dog c = (Dog) a;
		Dog d = (Dog) b;
}
```

* ArithmeticExcetpion是算术异常，典型错误代表例如：

```java
int a = 1;
int b = 0;
System.out.print(a/b);
```

当然，Java有丰富的异常种类，还有其他很多RuntimeException未提及,他们都是由JVM自动抛出的异常。出现这种问题，表示代码本身存在BUG，一般都要细心地查找一下，改进一下代码的逻辑，或者修改一下Corner Case。

###检查异常
对于检查异常，可检测异常经编译器验证，对于声明抛出异常的任何方法，编译器将强制执行处理或声明规则.基本上都是一些比较奇葩的原因，比如SQL异常(sqlExecption)，当你连接JDBC时，不捕捉这个异常，编译器就通不过，不允许编译。又比如文件IO异常。遇到这类异常，需要手动地添加捕获、处理语句。

##try-catch

单个的try-catch语句就不想说了，直接上多重try-catch语句，其基本的模板语法为：

```java
try{
	//Dangerous Activites
} catch(Exception A){
	//Handler for the situation A
} catch(Exception B){
	//Handler for the situation B
}
```

try 块：将一个或者多个语句放入 try 时，则表示这些语句可能抛出异常。编译器知道可能要发生异常，于是用一个特殊结构评估块内所有语句。

catch 块：当问题出现时，一种选择是定义代码块来处理问题，catch块的目的便在于此。catch 块是try块所产生异常的接收者。基本原理是：一旦生成异常，则try块的执行中止，JVM将查找相应的JVM。

需要注意的是，处理多重异常时应该先处理子类异常，再处理父类异常。这是因为如果catch顺序写反了，编译器会报错，迫使你更正到正确的顺序。

##try-catch-finally

对于一些Java代码，可能希望的是无论try中是否抛出异常，它们都需要得到执行，比如关闭数据库连接之类的。所以，我们可以追加一个finally语句紧接其后。完整的代码看起来像这样：

```
try{
	//Dangerous Activites
} catch(Exception A){
	//Handler for the situation A
} catch(Exception B){
	//Handler for the situation B
}....{
} finally{
	//do it anyway
}
```

##抛出异常关键词
throw是抛出这个动作

throws: 声明将要抛出何种类型的异常（声明）
```
	public void 方法名（参数列表）{
		throws 异常列表{
			//调用会抛出异常的方法或者：
			throw new Exception();
		}
	}
```

##Java 异常处理的原则

* 尽可能地处理异常

要尽可能的处理异常，如果条件确实不允许，无法在自己的代码中完成处理，就考虑声明异常。如果人为避免在代码中处理异常，仅作声明，则是一种错误和依赖的实践。

* 具体问题具体解决

异常的部分优点在于能为不同类型的问题提供不同的处理操作。有效异常处理的关键是识别特定故障场景，并开发解决此场景的特定相应行为。为了充分利用异常处理能力，需要为特定类型的问题构建特定的处理器块。

* 记录可能影响应用程序运行的异常

至少要采取一些永久的方式，记录下可能影响应用程序操作的异常。理想情况下，当然是在第一时间解决引发异常的基本问题。不过，无论采用哪种处理操作，一般总应记录下潜在的关键问题。别看这个操作很简单，但它可以帮助您用很少的时间来跟踪应用程序中复杂问题的起因。

* 根据情形将异常转化为业务上下文

若要通知一个应用程序特有的问题，有必要将应用程序转换为不同形式。若用业务特定状态表示异常，则代码更易维护。从某种意义上讲，无论何时将异常传到不同上下文（即另一技术层），都应将异常转换为对新上下文有意义的形式。
