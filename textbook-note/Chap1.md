# Introduction to Java

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
// 经典 Hello World 起手
```

Key Syntactic Features：

- Class Declaration：In Java, all code lives inside of classes



## Essentials

### Run a Java Prog

#### Normal Way

```shell
HelloWorld.java

# c means Compiler
javac HelloWorld.java	=> HelloWorld.class (Java Bytecode)

# Interpreter use .class file to execute
java HelloWorld
```

所以，Java 的编译执行过程可以看成是两步，`<Compiler, Interpreter>` 

（不过你愿意的话也可以自己开发一个 Java Interpreter 直接解析 Java Code



#### Why Class File

- 保护对 static typing 静态类型的处理

- 针对计算机来说，解析 .class 文件的效率会更高

- 因为 .class 文件无法被人类理解，某种程度上保护了原作者的权力（好吧，可以反解析的，没用



### Variable and Loops

```java
public class HelloNumber {
    public static void main(String[] args) {
        int x = 0;
		int sum = 0;
        while (x < 10) {
            System.out.print(sum + " ");
            x = x + 1;
			sum += x;
        }
    }
}
```

关于这段代码也有一些特性：

- 变量在声明后使用，并且 Java **强调类型**（和 Python 的区别
- Java 的执行，判断之类的行为都离不开括号（对比 Python



### GradeScope

```
code: 93PK75
```

[Gradescope](https://www.gradescope.com/) 

还没有试这个提交代码，不一定能用，得去网上看一下



### Static Typing

Java 的 Variable 和 expr 都会有一个 `static type`，the type of a variable can never change

Java variables can only contain values of that type

和所谓的 `static type` 静态变量无关，这里描述的是 Java 的可变数据类型和不可变数据类型

> 可变数据类型：当该数据类型对应的变量的值改变时，内存地址不会变
>
> 不可变数据类型：值改变对应的内存地址也会变



声明的变量类型本身不可变化

```java
public class HelloNumber {
    public static void main(String[] args) {
        int x = 0;
        while (x < 10) {
            System.out.print(x + " ");
            x = x + 1;
        }
        // String cannot be converted to int
        x = "string";	// CompilerError: Type Error
    }
}
```

Python 就没有这个问题，应该还是和内存存储有关，比如声明 `char type` 的时候直接分配了 1B 的空间，而如果更新变量的值到 `string type`，Java 字符串本身是字符串指针（引用）这个时候内存空间不支持，会报错

（只不过 Python 可能在运行的时候报错，只能说编译越严格的，执行出 bug 概率越小

```python
# TypeError 回忆一下 这种情况的输出要把数字变成字符串
x = 5 + "string"
```

```java
// Success!
String h = 5 + "string";

// CompilerError
int h = 5 + "string";

// 510 => 转变 string 输出
System.out.println(5 + "10");
```



### Defining Functions

Java 中所有的函数都属于某一个类，所以 Java 中的函数都可以称为方法，不像 Python 中有区分

```java
public class LargerDemo {
    public static int larger(int x, int y) {
        if (x > y) {
            return x;
        }
        return y;
    }
    
    public static void main(String[] args) {
        System.out.println(larger(8, 10));
    }
}
```

> 我现在可能缺一个帮我自动补全分号的工具，习惯性写冒号

这里书也吐槽说这段 Java 代码很繁琐，但是在构建大型项目的时候，繁琐会带来优势



### Code Style

代码风格这件事对 Java 确实很重要啊

- 复用风格：空格，变量命名之类的
- 一行的代码的长度：不能太长！
- 描述性命名
- 避免重复造轮子：和程序本身的结构相关

核心：即使对于一个陌生人来说也能很快看懂



### Comments

所有的 Java 方法和 Java Class 都应该使用 `Javadoc format` 的注释进行描述

```java
/**	XXX */
```

书里给出一个工具 [javadoc (oracle.com)](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/javadoc.html) 可以生成 HTML 形式的 Javadoc 
