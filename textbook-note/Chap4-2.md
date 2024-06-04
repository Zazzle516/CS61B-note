# Extends

上面的内容是针对接口的层级关系，如果我们想实现类之间的层级关系该怎么办呢

比如目前想写一个新类 `RotatingSLList`，和 `SLList` 有着相同的方法但是多了一共特殊方法，一种方式是完全拷贝一遍然后新增该特殊功能，但是这样就完全没有用到继承的特性

和接口继承不同的是，可以自由添加私有的方法，父类的方法，静态变量，内部子类都会继承

> 注意构造函数不会继承，私有成员也不会直接被子类继承

```java
// 使用 extends 关键字
public class RotatingSLList<Item> extends SLList<Item>
```



### VengefulSLList

记住那些被 `removeLast()` 删除的元素，防止它们集结起来形成反叛军，于是我们新建一个类用来记住那些被移除的元素 `VengefulSLList` 

```java
public class VengefulSLList<Item> extends SLList<Item> {
    SLList<Item> deletedItem;	// 记录所有被删除的元素
    
    // 构造函数
    public VengefulSLList() {
        deletedItems = new SLList<Item>();
    }
    
    // SLList<Item> deletedItem = new SLList<Item>();	也可以选择不使用构造函数 直接初始化
    
    public void printLostItems() {
        deletedItem.print();
    }
    
    @Override
    public Item removeLast() {
        // 让用户在无知的状态下达成效果
        Item x = super.removeLast();	// 使用 SLList 中的私有变量
        deletedItems.addLast(x);
        return x;
    }
}
```



### Constructors Are Not Inherited

子类可以继承父类的实例，静态变量，成员方法和嵌套子类，但是不包含构造函数

尽管构造函数不继承，Java 要求所有的子构造函数必须从调用父类构造函数开始

在子类继承父类的时候，是存在父类构建函数执行再到子类构建函数执行的先后顺序的

```java
public VengefulSLList() {
	super();	// 可省略	自动调用无参构造函数
    
    // 如果不想使用默认的无参构造 则必须指定父类构造函数
    deletedItems = new SLList<Item>();
}
```



### The Object Class

Java 中的任意一共类都是父类 `Object` 的子类，默认继承自该祖先，所以大家从祖先那里继承了什么呢

主要是定义了一些通用功能，比如 `.toString()`，`.hashCode()` 和 `.equals()` 之类的



### "Is-a" and "Has-a"

强调！！！！！



### Encapsulation / 封装

封装思想是 OOP 编程的基石之一，也是程序员对抗复杂度的最强大的工具

什么是封装呢，就是内部的具体实现不可见，只能通过公开的接口交互的方式

- Hierarchical abstraction

Create layers of abstraction, with clear abstraction barriers

- Design for change

一开始就预设在构建一共大型系统，并且这个系统中的每个子模块都有可能随时消失，而主系统不会因为这些子模块的消失而出现运行问题

> 视频里提到在后续 Project1B 的完成中，也会有这种问题，因为 `private resize()` 方法



### Abstraction Barriers

Java 可以通过编译器强制实现这层抽象功能，使用 `private` 关键字



### How Inheritance Breaks Encapsulation

#### Version1

```java
public void bark() {
	sout("bark");
}

public void brakMany(int N) {
    for (int i = 0; i < N; i++) {
        bark();
    }
}
```



##### 应用

假设我自己写了一个 `VerboseDog` 继承 `Dog` 的 `bark()` 方法

```java
@Override
public void barkMany(int N) {
	sout("As a dog, I say: ");
	for (int i = 0; i < N; i++) {
		bark();		// 调用父类的 bark()
	}
}

vd.brakMany(3);		// 调用 override_barkMany()
```



#### Version2

现在修改 `Dog` 类中方法的实现，交换了实现顺序，理论上从这里来看也没有任何差别

```java
public void bark() {
	barkMany(1);	// 根据 caller 的动态类型选择
}

public void brakMany(int N) {
    for (int i = 0; i < N; i++) {
        sout("bark");
    }
}
```



##### 应用 Error

```java
@Override
public void barkMany(int N) {
	sout("As a dog, I say: ");
	for (int i = 0; i < N; i++) {
		bark();		// 调用父类的 bark()
	}
}

vd.barkMany(3);		// 调用 Dog.bark() -> vd.barkMany() -> Dog.bark() ->... 无限循环
```

还是很能说明继承思想的一些坏处，很容易在开发者认为没问题的情况打破了这层抽象



# Casting

### Type Checking and Casting

重点是首先要通过编译器的静态检查，毕竟编译器没办法提前运行代码，不知道动态类型是什么

```java
public static void main(String[] args) {
    VengefulSLList<Integer> vsl = new VengefulSLList<Integer>(9);
    
    SLList<Integer> sl = vsl;	// 父类指针可以指向子类对象
    
    sl.addLast(50);
    sl.removeLast();		// Dynamic Selection 首先静态类型也存在这个方法
    
    // 注意编译器判断一个方法是否对某个对象有效使用静态类型
    // 通过 SLList 指针调用 VengefulSLList 是不被编译器认可的
    sl.printLostItems();	'Compile-Time Error'
    
    VengefulSLList<Integer> vsl_2 = sl;		// Compile Error
}
```



### Expression

首先使用 `new` 声明对象可以看成两个独立的部分，左部分 + 右部分，左边单纯的声明了一个名称，右边单纯的分配了一片空间，通过赋值把它们连接起来，所以**左右两个部分都有自己独立的类型**，而判断这两个独立类型能否一起作用，就是静态类型（父类子类）的关系决定的

使用 `new` 关键字创建的对象同样有编译类型（静态类型），只允许 `is-a` 关系的赋值，注意左右关系

```java
// VengefulSLList is-a SLList
// 此时右侧的 VengefulSLList<Integer>() 就是编译类型	然后判断该编译类型是否是 SLList
SLList<Integer> sl = new VengefulSLList<Integer>();
```

如果反过来，则不是

```java
// SLList !is-a VengefulSLList
VengefulSLList<Integer> vsl = new SLList<Integer>();
```

在函数传参中也是一样的效果，所以在参数中最好直接写父类类型

```java
Poodle frank = new Poodle("Frank", 5);
Poodle frankJr = new Poodle("Frank Jr.", 15);

public static Dog maxDog(Dog d1, Dog d2) { ... }

// pass
Dog largerDog = maxDog(frank, frankJr);

// Error: does not compile! RHS has compile-time type Dog
Poodle largerPoodle = maxDog(frank, frankJr); 
```



### Casting

Java 有一个特殊语法，通过强制类型转换，告诉编译器一个特别的表达式有一个特别静态类型

```java
// 在返回值前面通过 (Poodle) 完成强制类型转换
Poodle largerPoodle = (Poodle) maxDog(frank, frankJr);
```

不过要小心使用，因为你不让编译器行使它的职责，你就要检查是否合法，否则会有 `ClassCastException` 错误



# Higher Order Function

就是 CS61A 学的那一套，把函数作为数据处理

在 Java7 及以前，没有指向函数的指针的，所以 HOF 也根本无从谈起，实际上呢，也可以用接口继承实现

什么奇技淫巧，反正我不认可，强扭的瓜不甜_〆(´Д｀ )

```java
public interface IntUnaryFunction {
	public int apply(int x);
}

public class TenX implements IntUnaryFunction {
	/* Returns ten times the argument. */
	public int apply(int x) {
		return 10 * x;
	}
}

public class HoFDemo {
    public static int do_twice(IntUnaryFunction f, int x) {
		return f.apply(f.apply(x));
	}
    
    public static void main(String[] args) {
        System.out.println(do_twice(new TenX(), 2));
    }
}
```



### Inheritance Cheatsheet

就是对上面内容的一些总结











