# Intro and Interfaces

本节将引入 Java 中一个极其重要的编程思想：继承



### The Problem

假设我们现在想实现一个类 `WordUtils`，以链表的数据结构来处理单词，包含 `SLList` 统计最长字符串的功能

这个功能只在 `SLList` 类中实现了，`AList` 没有，那用户在使用的时候可能会使用任意一个

尽管 Java 可以在编译的时候自动找到对应的方法，但是这种实现方法太丑陋



### Hypernyms, Hyponyms ponyms and Interface Inheritance

> Hyper means high, Hypo means low...

说明一下虽然主体不一致但是操作流程一致的情况	dog is a Hypernym of poodle...

回到程序世界中也是一样的



### Interface

声明一个类（更多情况下是一个父类，抽象类）全部可以进行的行为

```java
public interface List61B<Item> {
    // 只能声明 public 方法
    public void addFirst(Item x);
    ...
}

// 就像你玩缺氧 规定了这个小人只能去刷厕所一样
```



#### Define a Referance type for Hypernym

```java
public interface List61B<Item> {
	// 以接口的方式声明
}
```



#### Specify the Hyponym of that type

继承了该接口的类实际上是做出了一个承诺，我保证我拥有并且声明在 `List61B` 中所有的变量和所有的方法

> 呜呜呜呜是真爱欸

```java
public class AList<Item> implements List61B<Item> {
    // 注意泛型也要声明在后面
    ...
}
```



### Overriding

基于接口定义的方法接口的实现，比如在子类中实现的具体方法都是重载



#### Overloading & Overriding

核心区别是：方法签名是否相同

overloading 只是作用在方法中，在没有继承的场景下，只要签名不同方法名相同都可以认为是 Overloading

而 overriding 只作用在继承场景下，IDEA 会在左边画上一个绿色的小圆圈还有一个红色的小箭头

```java
public interface Animal {
	public void makeNoise();
}

public class Pig implements Animal {
    // Pig overrides makeNoise()	类对方法的作用
    public void makeNoise() {
        ...
    }
}

public class Dog implements Animal {
    // makeNoise() is overloaded	并不涉及类 只关于方法
    public void makeNoise(Dog x) {
        ...
    }
}

// 注意英语的语序与被动关系
```



### Interface Inheritance

目前的继承都可以认为是来自接口的继承，子类对接口的传参要满足 `is-a` 关系

Java 支持多继承，进行多继承的子类需要需要满足所有父类的抽象方法要求



### Implementation Inheritance

相比于刚刚的抽象实现，Java 也支持在接口中给出真正运行的方法内容

```java
public interface List61B<Item> {
	default public void print() {
        ...
    }
}
// 继承该接口的子类可以使用该方法
```

当然，统一继承一个方法并不适合所有的子类，所以要追求高效实现的话，子类也要重写

```java
public class SLList implements List61B<Item> {
    @override
    public void print() {
        // 注意这种情况必须添加 @override 标签
        ...
    }
}
```



### Dynamic Method Selection (only for override)

还记得最开始的静态 Java 类型吗，`static type`，同时 Java 中每个变量还会有一个 `runtime type` 

在实际运行才得到的变量的动态类型，而对象的方法执行也是根据动态类型决定

最经典的就是 父类指针指向子类对象，静态是父类类型，而**动态类型是子类** 

```java
List61B<String> lst = new SLList<String>();
```

好处呢，就是后续 `lst` 也可以指向 `AList` 不受 Java 强类型限制的影响



### Overloaded Methods

注意这里是 `overloaded` 而不是 `override` 

`overloading` 根据静态类型选择要执行的方法，不然签名的不同就没有意义了

```java
// 传参要求的类型不同
public static void peek(List61B<String> list) {
    // peekA
	System.out.println(list.getLast());
}

public static void peek(SLList<String> list) {
    // peekB
	System.out.println(list.getFirst());
}
```



#### Puzzle

```java
SLList<String> SP = new SLList<String>();
List61B<String> LP = SP;

SP.addLast("elk");
SP.addLast("are");
SP.addLast("cool");

peek(SP);	// peekB
peek(LP);	// peekA
```



### Interface Inheritance vs Implementation Inheritance

如何区别接口实现和实现继承：其实很简单就是看接口有没有具体实现内容



继承这种编程方式很直观但是也有很多问题，这点新锐语言 rust 的组合思想就很不错 
