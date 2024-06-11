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



## Objects

类的定于与实例化 

### Static v.s. NonStatic Methoods

#### Static Methods

Java 中所有的代码都是类的一部分，绝大部分的函数都是写在方法中的，在一个完整的 Java 程序中

```java
public class Dog {
    public static void makeNoise() {
        System.out.println("Bark!");
    }
}

'Error': "Main Method not found"
```

如果想让这段代码跑起来，方法一是添加一个 `dogLaunch()` 方法，方法二是把当前这个方法改成 `main()` 方法

在其他类中定义执行然后通过 `main Class` 调用也是个不错的方法，可以把一个大问题拆成一个小问题 



#### Instance Variable and Object Instantiation

不同类型的狗有不同的发声方式，可以根据每种狗的方式单独写一个 Java Class，当然，也可以用类的方式

```java
public class Dog {
    public int weightInPounds;	// non-static variables
    
    public void makeNoise() {
        // 根据不同的 weightInPounds 完成不同的行为
        ...;
    }
}

public class DogLauncher {
    public static void main(String[] args) {
        Dog d;
        d = new Dog();
        d.weightInPounds = 20;
        d.makeNoise();	// 通过实例调用成员方法
    }
}
```



对上面的 Dog Class 添加构造方法如下

```java
public class Dog {
    public int weightInPounds;	// non-static variables
    
    public Dog(int w) {
        // be invoked when try to use the 'new' keyword
        // similar to '__init__' method
        weightInPounds = w;
    }
    
    public void makeNoise() {
        // 根据不同的 weightInPounds 完成不同的行为
        ...;
    }
}
```



### Terminology Summary

Non-static method = Instance Method

被 `static` 修饰的类方法不能访问实例变量，否则会报错



### Array Instantiation, Arrays of Objects

这个其实在 Proj0 里面已经实践了，二维数组的声明，传参，返回

这个视频其实更集中在声明一个类数组

```java
Dog[] dogs = new Dog[2];
dogs[0] = new Dog(8);
dogs[1] = new Dog(20);
dogs[0].makeNoise();
```



### Class Methods v.s. Instance Methods

这里其实就是对 `static` 这个关键字的讨论

```java
public class Dog {
    public static void makeNoise() {
        ...;
    }
}

Dog.makeNoise();

public class Dog {
    public void makeNoise() {
        ...;
    }
}
maya = new Dog(100);
maya.makeNoise();
```



#### 静态修饰有什么好处

有一些类根本就不会有实例 `Math Class`，直接使用方法

对比使用实例来调用方法，需要通过 `this` 关键字来获取调用的实例

```java
public static Dog maxDog(Dog d1, d2) {
    // 由狗狗上帝进行比较
    if (d1.weight > d2.weight) {
        return d1;
    }
    return d2;
}

public Dog maxDog(Dog d1) {
    // 由某个特定的狗狗进行比较
    if (this.weight > d1.weght) {
        return this;
    }
    return d1;
}

// 两个方法的签名不同 可以共存
```



针对静态变量：可以被该类实例化的所有对象访问

注意静态方法中不能直接访问实例变量，一定是通过传参的方式，得到实例本身去访问



### Public static void main(String[] args)

- public：能否被其他文件访问的状态
- static：静态方法，不会与其他特殊实例有关
- void：没有返回值类型
- main：该方法的名称
- String[] args：传参的名称 args



### Command Line Arguments

这里的 args 参数是由 Java Interpreter 提供的

```shell
javac XXXClass
java XXXClass args
```

实际上是在执行的时候可以传参 依次写在后面就行（突然想起来那个实习项目的文件执行就是这么做的）

哪怕不是文件执行，命令执行本身就需要很多参数 `--version` 之类的，都需要对 `args` 传参实现
