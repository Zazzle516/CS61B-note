# Autoboxing

#### Industrial Strength Syntax

本章主要讨论一些在实际开发中的不同的补充性话题



### Automatic Conversions

首先 Java 有 8 个基本类型，就是平时使用的 `int`，`short` 之类的，首先明确一下概念

- `wrapper`：`Integer`, `Double` 之类的大写，实际上是引用类型 `Reference Type` 

- `primitives`：`int`, `double` 之类的

注意二者之间的关系：`Integer` > `int`，也就是 `wrapper` 类型的覆盖范围要大于基本类型，所以从 `int` 转换到 `Integer` 需要的是包装，增加内容（注意这点）

```java
// 如果想使用 wrapper 类型的话
public class BasicArrayList {
    public static void main(String[] args) {
        ArrayList<Integer> L = new ArrayList<Integer>();
        
        // 理论上正常使用基本类型的流程	需要 wrap() 和 unwrap()
        L.add(new Integer(5));
        int first = L.get(0).valueOf();
    }
}
```



一直这么写真的很麻烦，所以引入 `Autoboxing` 和 `Auto Unboxing` 来简化流程

```java
public class BasicArrayList {
    public static void main(String[] args) {
        ArrayList<Integer> L = new ArrayList<Integer>();
        
        L.add(5);					// from int to Integer: Autoboxing
        int first = L.get(0);		// from Ingeter to int: Auto unboxing
    }
    // 所以 Java 一直在做隐式的转换
}
```

> 像 `Autoboxing` 和 `Un AutoBoxing` 是引用类型特有的能力，你决无可能搞出一个类似的，也是引用类型特殊的地方，本质上是一个指针 + 一片空间

注意 Java 无法对数列数组之类的进行隐式的类型转换，要手动对内容读取



### Autoboxing and Unboxing

泛型的使用，在使用的时候要给出一个具体的类型，这部分内容主要还是对上面视频的说明



### Caveats

同样，这节就是说一下在隐式类型转换的时候的注意点



### Widening and Narrowing

在基本类型之间也存在从大到小的类型转化

```shell
double <= int		# 当然反之就不行 需要手动进行强制类型转换
```



# Immutability

一般的变量都是可变的，除非是 `final` 关键字修饰之后，才会无法改变

实际上 `Integer`，`String` 和 `Date` 类都是不可变的，当然**不可变对象的诞生并不一定需要 `final` 修饰** 

```java
public class Date {
    // 一旦声明了之后 就不能再改了
    // 事实上 把 public 改成 provate 同样达成了不可修改的目标
    public fianl int year;
    ...
        public Date(int m, int d, int y) {
        this.month, day, year = m, d, y;
    }
}
```

这些不可改变的值有一些好处，迫使你优化项目结构，更不容易出错

记住即使定义了指针是 `fianl` 也不能代表该指针所指向的对象就不变了，只是该指针不能再指向其他实例对象了

注意 Java 中的 `String` 类型的数据都是**不可改变的**，每次你以为对 `String` 操作实际是返回了一个新的 `String` 



# Generics

事实上之前已经用过了，就是泛型那堆乱七八糟的，现在重新的来系统性的接触一下

目标：声明一个 `ArrayMap Class` 并且有以下方法：

- `put(key, value)`：放入一组键值对
- `contains(key)`：检查是否包含该键
- `get(key)`：假设键存在的情况下，返回该键对应的值
- `keys()`：返回所有的键
- `size()`：返回键的数量

注意在实现 `keyIndex()` 的时候会有这样的一个问题，`==` 本质只是看这两个指针是否指向同一个地址，并不是真的分析内容，如果是两个相同内容的不同实例，是没有办法判断相等的，只能用 `equal()` 方法



### ArrayMap and Autoboxing Puzzle

如何用数组实现映射其实就是课本里的举例，用两个数组通过下标一一对应去实现

在 `ArrayMap.java` 文件中的一个测试用例实际上会有一个奇怪的报错

```java
@Test
public void test() {
    ArrayMap<Integer, Integer> am = new ArrayMap<Integer, Integer>();
    am.put(2, 5);
    int expected = 5;
    assertEquals(expected, am.get(2));
}

// ... reference to assertEquals is ambiguous ...

// 为什么会报错呢 因为 Java 不清楚我们现在想调用的是哪个 assertEquals() 方法
```



Q：现在我们能做点什么能够让 `<int, Integer>` 转变为 `<long, long>` 类型

- Widen `expected` to `long` 
- Unbox `am.get(2)` 
- Widen the unboxed `am.get(2)` to `long` 



Q：现在我们能做点什么能够让 `<int, Integer>` 转变为 `<Object, Object>` 类型

这个简单，把 `expected` 改成 `Integer` 就可以了



### Generic Methods

接下来主要利用 `MapHelper.java` 文件弥补一个在 `ArrayMap` 实现中的漏洞，因为 `get()` 无法处理 `key` 不存在的情况，还有 `maxKey()` 如果 `key` 可以比较的话返回最大值

当然在实际开发中并不会在另一个文件中修改 `get()` 方法而是在原地修改，但是这里是为了学习

在写方法的时候就能发现，如果直接按下面的写法：

```java
public static get(Map61B<K, V> map, K key)	// 无法通过编译的
```



当然为什么呢，可能会想这是没有在类的声明中声明泛型，但是实际上即使添加了泛型，使用 `static` 也会报错

```java
public class MapHelper<K, V> {
    public static V get(Map61B<K, V> map, K key) {
        ...
    }
}

// 'Map61B.MapHelper.this' cannot be referenced from a static context
```

只把泛型写在方法传参也是不行的，如果不在类的定义中声明，Java 无法识别



#### Solve

不去声明 `MapHelper` 类的泛型，毕竟它也只提供方法处理，不涉及任何对象实例和变量

所以把泛型范围控制在方法范围就可以，在方法的关键字处声明泛型即可

```java
public class MapHelper {
    // 把类的泛型声明转移到方法
    public static <K, V> V get(Map61B<K, V> map, K key) {
        ...
    }
}
```



### Implementing maxKey

这个我自己最开始想的是额外传入一个比较器对象，不然编译器不知道怎么比较，但是老师不愧是老师，强太多

```java
public static <K, V> K maxKey(Map61B<K, V> map, Comparator<K> c) {
    // 把 keys 作为数组取出来
    List<K> keyList = map.keys();

    // 初始化
    K currMaxKey = keyList.get(0);

    for (int i = 1; i < map.size(); i++) {
        if (c.compare(currMaxKey, keyList.get(i)) > 0) {
            currMaxKey = keyList.get(i);
        }
    }
    return currMaxKey;
}
```



实际上这种额外要求参数的形式还有其他的解决方式，那就是**对泛型本身做出约束**，要求**泛型拥有比较能力**

```java
public static <K extends Comparable<K>, V> K maxKey(Map61B<K, V> map) {
    // 把 keys 作为数组取出来
    List<K> keyList = map.keys();

    // 初始化
    K currMaxKey = keyList.get(0);

    for (K key: keyList) {
        if (key.compareTo(currMaxKey) > 0) {
            // 使用泛型自己的比较方法
            currMaxKey = key;
        }
    }
    return currMaxKey;
}
```



### Type Upper Bounds

针对上面的 `<K extends Comparable<K>, V>` 实现其实会有一些问题



**Q：**既然 `Comparable` 是 `interface` 为什么使用 `extends` 关键字

这个其实是 Java 中对 `extends` 这个关键字的使用问题，这里**只是用来声明**并不是真的继承，并不像一般写在 `class` 后面的 `extends` 关键字需要实现对应的方法，这里的 `K` 是 `Comparable Interface` 的 `SubType` 

实际上是 Java 的开发者非要用这个关键字，如果不用这个关键字会少很多歧义



**Q：**为什么用 `Comparable` 而不是 `Comparator` 接口

可能更偏向使用场景的不同吧，`Comparator` 更偏向自定义比较器？

（因为老师没有提出这个问题，我就自己解决一下



## Summary

关于 Java 泛型的四个相关特性

- Autoboxing and auto-unboxing of primitive wrapper types
- Promotion/widening between primitive types
- Specification of generic types for methods (before return type)
- Type upper bounds in generic methods (e.g. `K extends Comparable` ).



# Done

终于，进入第六章，太多阅读材料了... 无语，虽然很有用是真的