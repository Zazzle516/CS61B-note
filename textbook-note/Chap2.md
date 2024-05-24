# Lists

在 Proj0 里有大量使用数组的代码，数组在 Java 中都是固定长度对的，无法被改变

Java 中有提供 Build-in 数组类型，本章的介绍重点



## Mystery of the Walrus

```java
Walrus a = new Walrus(1000, 8.3);
Walrus b;

b = a;
b.weight = 5;		// change b will affect a
System.out.println(a);
System.out.println(b);
```

这里面蕴含的运行逻辑可以帮助我们构建更强大的代码，更有效率的数据结构



### Bits

为什么会有上面不同影响的结果呢

因为 Java 有 8 大基本类型

```
[byte] [short] [int] [long] [float] [double] [boolean] [char]
```

当你在 Java 中声明一个特定变量之后，计算机会根据这个类型分配特定的存储空间，来存储这个变量

Java 会在内部维护一个表，完成每个变量名称对地址的映射

如果只是声明的话：Java 并不会在预留的空间内写入任何数据（安全起见，Java 不允许未初始化的变量被访问

> Tip：一个变量在内存的存储地址其实是超过了 `Java level of abstration accessible` 
>
> 在 Java 中无论使用何种方式都不可能访问到某个变量的具体地址（Java 有点废物啊...
>
> 这个 PDF 在狡辩这个 Java 特性是为了更好的性价比
>
> Donald Knuth：premature optimization is the root of all evil	过早优化是地狱的开始



#### GRoE/Golden Rule of Equals

赋值语句会把 `box(x)` 内的每一个 bit 都复制到 `box(y)` 中



### Reference Types

上面是针对一等数据类型的处理，其他的所有数据类型都是通过引用去处理的，包括数组



#### Object Instantiation

在对象的实例化过程中，Java 会先为这个类的每个实例变量分配出空间，然后给出默认值

`new` 关键字其实更像是声明在内存的哪里储存该变量，返回内存中的某一个地址



#### Reference Variable Declaration

引用类型不会关心内部具体需要多少内存空间，只会返回一个 64bit 的变量

```java
Walrus someWalrus;
someWalrus = null;		// 并没有进行地址分配 或者说本应该用来指向存储位置的指针目前是 null

Walrus someWalrus;
someWalrus = new Walrus(1000, 8.3);		// 通过 new 进行了空间分配
```

对引用进行拷贝复制只是拷贝了指向，共享同一片内存的内容



#### Box and Pointer Notation

在 Java 的可视化执行中，使用小格子表示内存分配的空间（指针可视化）



#### Resolving the Mystery of the Walrus

引用的拷贝只是指向同一个内存空间



#### Parameter Passing

函数的传参与引用如何结合起来，本质都是**值传递**，只不过引用传递的是地址，通过地址直接修改

函数有自己的局部函数帧，参数储存在局部函数帧中，相对于主函数的运行环境是独立的



#### Test My Understanding

```java
public class PassByValueFigure {
    
	public static void main(String[] args) {
		Walrus walrus = new Walrus(3500, 10.5);
		int x = 9;
		doStuff(walrus, x);	// walrus 引用	x 常量
		System.out.println(walrus);	// 改变
		System.out.println(x);		// 不变
	}
	
    public static void doStuff(Walrus W, int x) {
		W.weight = W.weight - 100;
		x = x - 5;
	}
}
```



### Instantiation of Arrays

实例化数组和其他类的实例化其实也没啥区别，都要使用 `new` 关键字

```java
int[] x;					// 单纯声明
new int[]{0, 1, 2};			// 单纯的分配了空间 然后立刻释放掉..(一瞬的生命)
int[] x = new int[]{0, 1};	// 包含空间分配

Walrus[] wal_list = new Walrus[2];	// 分配指定数量的 Walrus 数组
```

和其他类型不太一样的是，Java 只允许数组变量储存真实的内存地址（Java 本来是无法访问变量地址的）



### Law of the Broken Futon

[The Math Ceiling: Where’s your cognitive breaking point? – Math with Bad Drawings](https://mathwithbaddrawings.com/2015/04/08/the-math-ceiling-wheres-your-cognitive-breaking-point/) 

有一说一，这个小故事讲的挺好的，我自己也有这种经历，而且还就是指针学习，当时没理解虽然也能敲代码，但实际上是残缺的，我到后面慢慢又补起来才完全理解，那一刻真的是恍然大悟的感觉

上面讲的指针或者说引用我其实早就学了233（或者说自己悟了），但这种基础时不时复习一下还是挺好的



### Int Lists

链表，可变长度（其实就像是在 Python 里面做的那样）

```java
public class IntList {
    public int first;
    public IntList rest;	// type(next) = self
    
    public IntList(int f, IntList r) {
        // 声明构造方法
        first = f;
        rest = r;
    }
    
    public static void main(String[] args) {
        // 这样只能在前面添加元素
        IntList L = new IntList(15, null);
        L = new IntList(10, L);
        L = new IntList(5, L);
    }
}
```



#### Size and IterativeSize

为 `IntList` 添加一个 `size()` 方法，返回链表的长度

- `size()`：使用递归的方法返回长度
- `IterativeSize`：使用迭代的方法返回长度

```java
// my code here
public class IntList {
    public int first;
    public IntList rest;	// type(next) = self
    
    public IntList(int f, IntList r) {
        // 声明构造方法
        first = f;
        rest = r;
    }
    
    public static int Size(IntList L) {
        // 注意不能执行 L.rest 否则会有 NullPointerError
        if (L != null) {
            return 1 + L.Size(L.rest);
        }
        return 0;
    }
    
    public static int IterableSize(IntList L) {
        int length = 0;
        while (L != null) {
            length += 1;
            L = L.rest;
        }
        return length;
    }
}

// 有一说一 客观上我这么写可以运行 但是没有体现出 Java OOP 编程的思想
// 单纯的写了一个方法函数 和 Object IntList 都没关系了
// 只能说是 Python 的思想遗毒
```



##### 修订版

通过对象实例来调用 `L.size()` 而不是 `size(L)` 

```java
// my code here
public class IntList {
    public int first;
    public IntList rest;	// type(next) = self
    
    public IntList(int f, IntList r) {
        // 声明构造方法
        first = f;
        rest = r;
    }
    
    public static int size() {
        if (this.rest == null) {
            return 1;
        }
        return 1 + L.rest.size();
    }
    
    public static int IterableSize() {
        int length = 0;
        IntList ptr = this;
        while (ptr != null) {
            length += 1;
            ptr = ptr.rest;
        }
        return length;
    }
}
```



#### Get

获取特定位置的元素

```java
// my code here
public class IntList {
    public int first;
    public IntList rest;	// type(next) = self
    
    public IntList(int f, IntList r) {
        // 声明构造方法
        first = f;
        rest = r;
    }
    
    public int get(int index) {
        // 首先是针对特殊情况的处理
        if (index > this.size()) {
            return null;
        }
        IntList ptr = this;
        while (index) {
            ptr = ptr.rest;
            index -= 1;
        }
        return ptr.first;
    }
}
```



#### Other Exercise

##### incrList（IntList L, int x）

对 L 的每个元素增加 x 的固定值，返回一个新列表

```

```



##### DincrList（IntList L, int x）



### Declaring a Variable (Simplified)

P48



















