# Subtype Polymorphism

通过继承，可以使用多态，设计出更有普遍性的数据结构和方法

多态，多种形态，在 Java 中指一个类（object）可以拥有多种类型或形式，在 OOP 编程中，多态主要和一个对象如何被判断为他自己类的实例，父类的实例之类的事情有关

比如目前有一个变量 `deque` 的静态类型是 `Deque`，实际上去调用 `deque.addFirst()` 的时候是取决于它的动态类型（在运行时决定）

Polymorphism：providing **a singe** interface to **entities of** different types

​							  你只需要知道 `deque` 的静态类型，而具体是 `Link` 还是 `Array` 是由对象自己决定的

**打印两个对象中的较大者**

##### HOF

```python
def print_larger(x, y, compare, stringify):
    # compare() 和 stringify() 函数作为指针传入	独立于对象存在
    if compare(x, y):
        return stringify(x)
    return stringify(y)
```



##### Subtype Polymorphism Approach

```python
def print_larger(x, y):
    # 让对象自己去比较	根据 x, y 具体的类型去调用
    # OOP 编程思想
    if x.largerThan(y):
        return x.str()
    return y.str()
```



### Max Function

假设我们现在写一个找到最大值的方法，可以接收任何数列，不管类型

```java
public static Object max(Object[] items) {
    int maxDex = 0;
    for (int i = 0; i < items.length; i++) {
        // Operator '>' cannot be applied to 'java.lang.Object', 'java.lang.Object'
        if (items[i] > items[maxDex]) {
            maxDex = i;
        }
    }
    return items[maxDex];
}

public static void main(String[] args) {
    Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
    Dog maxDog = (Dog) max(dogs);
    maxDog.bark();
}
```

我乍一看没看出什么问题，敲到 IDEA 里能看到在 `items[i] < items[maxDex]` 报错了，因为 Java 不知道怎么比较类型（还是不要太相信自己，什么都要敲一敲）

如果要解决这个问题，也简单，单独为 `Dog` 类写一个找最大值的方法，但是这样就失去了普遍化的意义

在 Python 或 C++ 中可以通过对 `operator` 的重载实现，但是 Java 中没有这个功能，但是可以借助接口继承实现



### Interface: CompareTo

我们可以写一个接口文件，并且确保 `Dog` 会继承该接口，包含一个比较方法 `compareTo` 

```java
public interface OurComparable {
    public int compareTo(Object o);
    // public int compareTo(OurComparable o);	写成这样也 ok
}
```

```java
public class Maximizer {
    public static OurComparable max(OurComparable[] items) {
        // 注意这里的 参数类型 和 返回值类型
        int maxDex = 0;
        for (int i = 0; i < items.length; i++) {
            int cmp = items[i].compareTo(items[maxDex]);
            if (cmp > 0) {
                maxDex = i;
            }
        }
        return items[maxDex];
    }

    public static void main(String[] args) {
        Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
        Dog maxDog = (Dog) max(dogs);
        maxDog.bark();
    }
}
```

```java
public class Dog implements OurComparable{
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void bark() {
        System.out.println("bark");
    }

    @Override
    public int compareTo(Object o) {
        // 在实际使用中通常都是做减法 判断结果的正负
        return Integer.compare(this.age, ((Dog) o).age);
    }
}
```

以接口作为参数传参确实是个比较抽象的事情，毕竟接口只是对函数接口的封装，传递这个行为本身是要传递继承了该接口（实现了接口要求的函数）的**具体类**，而不是一个抽象接口，这个只是方便去写，因为不可能对每个继承了该接口的类都写一个不同参数类型的函数

在 `max()` 中用 `OurComparable` 去接收就保证该具体类一定继承并实现了 `OurComparable` 接口要求的函数



### Interfaces Quiz

```java
public interface OurComparable {
    public int compareTo(Object o);
}
```



```java
public class DogLauncher {
    public static void main(String[] args) {
        Dog[] dogs = new Dog[]{d1, d2, d3};
        System.out.println(Maximizer.max(dogs));
    }
}
```



```java
public class Dog implements OurComparable {
    ...
    public int compareTo(Object o) {
        Dog uddaDog = (Dog) o;
        if (this.size < uddaDog.size) {
            return -1;
        } else if (this.size == uddaDog.size) {
            return 0;
        }
        return 1;
    }
    ...
}
```



```java
public class Maximizer {
    public static OurComparable max(OurComparable[] items) {
        int maxDex = 0;
        for (int i = 0; i < items.length; i++) {
        	int cmp = items[i].compareTo(items[maxDex]);
            if (cmp > 0) {
                maxDex = i;
            }
        }
		return items[maxDex];
    }
}
```



#### Q1

给出 `Dog`，`DogLauncher`，`OurComparable` 接口和 `Maximizer`，如果我们省略在 `Dog` 中写的 `compareTo()` 方法，哪个文件会编译失败

`Dog` 给不出具体实现的话，首先是 `Dog` 文件无法通过编译，因为没能做到 `implements` 的承诺，如果只是改成了一个空函数，那么在 `Maximizer` 的具体执行就会出错（吧



#### Q2

如果只是把 `implements` 这行声明删掉，而不去删除函数，这时哪个文件会编译错误

此时的 `Dog` 没有继承 `OurComparable` 接口，也就和接口毫无关系，那么在 `Doglauncher` 的函数传参就会错误

> 记住编译器一定是在代码运行之前检查错误的，所以并不是 `Maximizer` 报错



### Comparable Interface

尽管我们通过接口实现了对 `max` 的普遍应用，但实际使用起来还是有一些问题，比如不得不进行的强制类型转换，没有任何 Java 内部类来实现 `OurComparable` 接口，也没有任何 Java 内部类来调用该接口（写给自己玩的）

为什么没人用呢，因为 Java 已经提供了一个 `Comparable` 接口，并且被无数的 Java 库使用

```java
public interface Comparable<T> {
    // 注意这里使用了泛型
	public int compareTo(T obj);
}
```

> 官方的 `Comparable` 接口使用了泛型，避免了后续的强制类型转换



使用官方库重写 `Dog` :)

```java
public class Dog implements Comparable<Dog> {
    public int compareTo(Dog thatDog) {
        return this.size - thatDog.size;
    }
}

// 同时记得修改 Maximizer 的接口传参类型
```

但是在返回值的时候仍然需要进行强制类型转换，所以仍然留下了一个小尾巴

```java
public class Dog implements Comparable<Dog>{
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void bark() {
        System.out.println("bark");
    }

    public int compareTo(Dog thatDog) {
        return this.age - thatDog.age;
    }
}

// ------------------- //

public class Maximizer {
    public static Comparable max(Comparable[] items) {
        int maxDex = 0;
        for (int i = 0; i < items.length; i++) {
            int cmp = items[i].compareTo(items[maxDex]);
            if (cmp > 0) {
                maxDex = i;
            }
        }
        return items[maxDex];
    }

    public static void main(String[] args) {
        Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
        Dog maxDog = (Dog) max(dogs);
        maxDog.bark();
    }
}
```



### Comparator Interface

Q：自定义比较方式（ `comparable` 做不到吗？，应该也能啊，因为可以自定义 `compareTo` 方法，如果 Comparator 类只是取代 compareTo 的函数，有什么意义呢

A：为**同一个类**同时提供**不同的比较方式**，比如需要一个不同的排序方式之类的

```java
public interface Comparator<T> {
	int compare(T o1, T o2);
}
```

这种需求 HOF 倒是可以轻易解决，因为传入不同的 `compare` 函数就可以了

```python
def print_larger(T x, T y):
    # 这种情况如何支持多个比较函数
    if x.largetThan(y):
        return x.str();
    return y.str()
```

```python
def print_larger(T x, T y, comparator<T> c):
    if c.compare(x, y):
        # 让函数自己去决定 ???
        return x.str()
    return y.str();
```



#### Use Comparator

Q：为什么使用内部类实现呢

通过内部类可以多个不同的比较逻辑，而不影响主类的实现，每个 `comparator` 内部类对应一个比较逻辑

```java
import java.util.Comparator;    // 因为没有 Comparable 那么常用

public class Dog implements Comparable<Dog>{
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() {
        return this.name;
    }

    public void bark() {
        System.out.println("bark");
    }

    public int compareTo(Dog thatDog) {
        return this.age - thatDog.age;
    }

    // 不属于任何一个具体实例方法 所以可以用 static 修饰
    public static class NameComparator implements Comparator<Dog> {
        @Override
        public int compare(Dog o1, Dog o2) {
            // 这里使用的是 Java 中 String 中自己实现 compareTo 方法
            // public int compareTo(String anotherString) {...}
            return o1.name.compareTo(o2.name);  
        }
    }
}
```



```java
public class Maximizer {
    public static Comparable max(Comparable[] items) {
        int maxDex = 0;
        for (int i = 0; i < items.length; i++) {
            int cmp = items[i].compareTo(items[maxDex]);
            if (cmp > 0) {
                maxDex = i;
            }
        }
        return items[maxDex];
    }

    public static void main(String[] args) {
        Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
        Dog maxDog = (Dog) max(dogs);
        maxDog.bark();

        Dog.NameComparator nameComparator = new Dog.NameComparator();
        if (nameComparator.compare(dogs[0], dogs[1]) < 0) {
            System.out.println(dogs[0].getName());
        }
    }
}
```



#### Update Comparator

在自定义的 `comparator` 外面套一层，生成新的比较器对象返回

```java
public class Dog implements Comparable<Dog>{
    private String name;
    private int age;
    
    ...

    // 私有化该比较方法
    private static class NameComparator implements Comparator<Dog> {
        @Override
        public int compare(Do g o1, Dog o2) {
            return o1.name.compareTo(o2.name);  
        }
    }
    
    public static Comparator<Dog> getNameComparator() {
        return new NameComparator();
    }
}
```



```java
import java.util.Comparator;

public class Maximizer {
    public static Comparable max(Comparable[] items) {
        int maxDex = 0;
        for (int i = 0; i < items.length; i++) {
            int cmp = items[i].compareTo(items[maxDex]);
            if (cmp > 0) {
                maxDex = i;
            }
        }
        return items[maxDex];
    }

    public static void main(String[] args) {
        Dog[] dogs = {new Dog("Elyse", 3), new Dog("Sture", 9), new Dog("Benjamin", 15)};
        Dog maxDog = (Dog) max(dogs);
        maxDog.bark();
		
        // 直接使用 Comparator 作为容器接收
        // 在上面那个 public 的例子中直接用 Comparator 接收也 ok 的
        Comparator<Dog> nameComparator = Dog.getNameComparator();
        if (nameComparator.compare(dogs[0], dogs[1]) < 0) {
            System.out.println(dogs[0].getName());
        }
    }
}
```

实际上就是通过接口实现了函数回调功能

这两个接口其实区别还是蛮大的，`comparable` 针对**自己**和另一个对象的，而 `comparator` 是针对**两个**对象

> 因为 Java 只允许实现一个自定义 `compareTo()` 方法
