# Lists, Sets and ArraySet

这里就是带着回忆一下之前自己写 `List` 数据结构的内容，方便和后面真实开发场景对比

```java
java.util.List<Integer> L = new java.util.ArrayList<>();
```



#### Set

介绍一下 Java 版本的

```java
Set<String> S = new HashSet<>();
S.add("Tokyo");
...
```



也有 Python 的，会更简洁一些

```python
s = set()
s.add("Tokyo")
print("Tokyo" in s)
```

接下来打算自己实现一个 `Set` 被称为 `ArraySet`，就是使用数组作为实现结构

- `add(value)`：如果不存在则添加
- `contains(value)`：判断是否存在
- `size()`：返回数量



### ArraySet

就是用数组实现以下 Set 的特性，其实没什么好说的，就是注意自己用数组的时候没有 `Array.add()` 成员方法给你用，它就是个单纯的数据结构，用下标，也就是记录 `size()` 的方式去新增元素

注意为空的元素无法参与比较，`NullPointerException` !!!

```java
public class ArraySet<T> {

    private T[] myList;
    private int size;       // record effective elements

    public ArraySet() {
        this.myList = (T[]) new Object[100];     // 不考虑扩容之类的问题
        this.size = 0;
    }

    public boolean contains(T x) {
        for (T value : this.myList) {
            if (value != null && value.equals(x)) {
                // 注意空元素无法参与比较	同时要求该类型必须有 equals() 方法
                return true;
            }
        }
        return false;
    }

    public void add(T x) {
        if (x == null) {
            return;
        } else {
            if (contains(x)) {
                return;
            }
            // 数组本身并没有 add() 方法 所以用下标的方法
            this.myList[size] = x;

            this.size += 1;
        }
    }

    /* Returns the number of key-value mappings in this map. */
    public int size() {
        return this.size;
    }

    public static void main(String[] args) {
        ArraySet<String> s = new ArraySet<>();
//        s.add(null);
        s.add("horse");
        s.add("fish");
        s.add("house");
        s.add("fish");
        System.out.println(s.contains("horse"));
        System.out.println(s.size());
    }

    /* Also to do:
    1. Make ArraySet implement the Iterable<T> interface.
    2. Implement a toString method.
    3. Implement an equals() method.
    */
}
```



# Throw Exceptions

### Syntax

```java
throw new Exceptions();
```

单纯的抛出错误会导致程序中止，在实际的使用环境中我们肯定不希望程序动不动就崩溃

所以我们可以捕获这些错误去处理，除了捕获错误还有一些其他的解决办法就是修改原有逻辑让程序更健壮一些

Java 的官方库也是采用了相同的逻辑，可以放入空元素也能对存在空元素判断为 `true` 

无论哪种处理方法都可以，就是保持一致性并且写好文档



# Iteration

让 `ArraySet` 支持迭代器功能（好漫长的视频，好长的英文....

```java
Set<Integer> javaSet = new HashSet<>();
for (int i : javaSet) {
    sout(i);
}
```



### How Iteration works

在短短的一行代码中到底经历了什么可以支持 `enhance-for` 循环

```java
import java.util.Iterator;	// IDEA 会自动帮你导入的

Set<Integer> javaSet = new HashSet<>();

Iterator<Integer> seer = javaSet.iterator();

while (seer.hasNext()) {
    sout(seer.next());	// iterator 给出当前元素同时后移一个元素
}
```

如果想要在自实现的 `ArraySet` 中实现，需要添加一个 `iterator()` 方法并返回一个 `Iterator<T>` 类，在该类中有效实现了 `hasNext()` 和 `next()` 方法

可以在 `ArraySet` 写一个内部类 `ArrayIterator` 实现

> 当我写一个类继承接口的时候，这个接口是泛型的，那我的内部类还用写泛型吗
>
> GPT：当内部类已经处于一个泛型外部类的上下文中时，它可以直接访问和使用外部类的泛型类型参数。因此，不需要在内部类中重新声明泛型类型参数。这样设计的好处是减少了冗余和混乱，使得代码更加简洁和易读。



### Support Iteration

仅仅是实现 `Iterator` 内部类和成员方法是不够的，实际上在外部调用增强 `for` 循环仍然会报错，因为 Java 并不知道你实现了 `Iterator` 子类，所以需要在外部类完成对全世界的宣言，自己已经实现了 `Iterator` 

声明中使用的是 `Iterable` 注意这一点，可能会比较混乱

```java
// 注意这里是外部类所以要写两个泛型
public class ArraySet<T> implements Iterable<T>
```

迭代器的强大之处在于即使是链表，只要你实现了 `Iterator` 内部类，也可以完成增强 `for` 循环的遍历

> 下面这个是 GPT 生成的，我也得自己写一份

```java
import java.util.Iterator;
import java.util.NoSuchElementException;

public class MyLinkedList<T> implements Iterable<T> {

    private class Node {
        T data;
        Node next;

        Node(T data) {
            this.data = data;
            this.next = null;
        }
    }

    private Node head;

    public MyLinkedList() {
        this.head = null;
    }

    public void add(T data) {
        if (head == null) {
            head = new Node(data);
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = new Node(data);
        }
    }

    @Override
    public Iterator<T> iterator() {
        return new LinkedListIterator();
    }

    private class LinkedListIterator implements Iterator<T> {
        private Node current = head;

        @Override
        public boolean hasNext() {
            return current != null;
        }

        @Override
        public T next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            T data = current.data;
            current = current.next;
            return data;
        }
    }

    public static void main(String[] args) {
        MyLinkedList<Integer> list = new MyLinkedList<>();
        list.add(1);
        list.add(2);
        list.add(3);

        // 使用增强 for 循环遍历链表
        for (Integer value : list) {
            System.out.println(value);
        }
    }
}
```



# Object Methods

但凡是 Java 中的类，都会默认继承 `Object`，只要继承了 `Object` 就会继承 `Object` 的成员方法

```java
String toString();
boolean equals(Object obj);
Class <?> getClass();
int hashCode();
protected Objectclone();
protected void finalize();

void notify();
void notifyAll();

void wait();
void wait(long timeout);
void wait(long timeout, int nanos);
```

里面有一些成员方法看着很奇怪，但是目前我们只关心最上面两个 `toString()` 和 `equals()` 

> 后面还会再讨论以下 `hashcode()`，其他的就涉及不到了



### toString

声明一个对象的输出 / 打印方式

```java
System.out.println(Object x) == x.toString()
```

如果不去重写这个方法而是直接使用

```java
System.out.println(obj);	// 只能得到一串 Java 对象编号

Obj_name + @ + Mem_addr
```

重写 `toString()` 方法

```java
@Override
public String toString() {
    ...;
}
```

这里视频使用了递归的方式，很优雅啊，在 CS61A 也常见的其实（主要是我好久没见有点忘了

```java
@Override
public String toString() {
    String returnStr = "{";
    
    for (T item : this) {
        returnStr += item.toString();
        // returnStr += item;	事实上不写 toString() 也 ok 的 因为 Java 是在帮你隐式调用
        returnStr += ", ";
    }
    returnStr += "}";
    return returnStr;
}
```



### toString Optimize

在上面的 `toString()` 虽然可以实现，但是这个方法其实是相当慢的，因为 Java 的 String 变量的不可修改性，每次添加一个新的 `item` 到 `returnStr` 中其实都是创建了一个新的字符串

因为创建新字符串并且拷贝的关系，时间复杂度并不只是线性的，`(1 + 2 + 3 + ... + (N - 1))` 的复杂度

所以 Java 提供了 `StringBuilder` 来优化这个问题



### StringBuilder

提供一个可变的字符串类型

```java
@Override
public String toString() {
    StringBuilder returnSB = new StringBuilder("{");
    
    for (int i = 0; i < this.size; i++) {
        returnSB.append(items[i].toString());
        returnSB.append(", ");
    }
    
    returnSB.append("}");
    return returnSB.toString();
}
```



### Equals

虽然之前也提到过，但是视频里又着重强调了一遍，`(==) != equals()` 

针对 `==` 比较，比较的的是两个变量格子内的比特位，针对基本类型是可以的，因为值都是直接存储在格子里，然而针对引用类型就不是这样了，`==` 比较的是两个格子的引用是否指向了同一片空间

所以即使两个对象的内容完全相同但是地址不同也会被判定为不相等，需要重写 `equals()` 方法

`equals()` 的默认实现就是 `==` 所以如果不重写的话是一定会有问题的



### ArraySet Equals

函数签名必须和 `Object.equals()` 保持一致

```java
public boolean equals(Object xxx){
    ...;
}
```

注意 Set 的比较不能涉及顺序

```java
public boolean equals(Object other){
    if (other == null) {
        // 判空防止程序崩溃
        return false;
    }
    if (other.getClass() == this.getClass()) {
        ArraySet<T> otherSet = (ArraySet<T>) other;
        if (otherSet.size() ==  this.size()) {
            // 在长度相等的情况下 单方向判断集合 A 中元素集合 B 是否存在就可以
            for (T item : this) {
                if (!otherSet.contains(item)) {
                    return false;
                }
            }
        } else {
            return false;
        }
        return true;
    } else {
        return false;
    }
}
```



### Equals Optimize

根据 `==` 的判断进行优化，避免一些无意义的比较

```java
public boolean equals(Object other){
	...;
    if (other == this) {
        // 针对自己比较自己的情况
        return true;
    }
    ...;
}
```



### Equals Rule

1.) equals must be an equivalence relation 

- reflexive: `x.equals(x)` is true
- symmetric: `x.equals(y)` if and only if `y.equals(x)` 
- transitive: `x.equals(y)` and `y.equals(z)` implies `x.equals(z)` 

2.) It must take an `Object` argument, in order to override the `original .equals()` method

3.) It must be consistent if `x.equals(y)` , then as long as x and y remain unchanged: x must continue to equal y

4.) It is never true for null `x.equals(null)` must be false



### String Join()

利用数组的 `join()` 方法拼接字符串

但是这里在最后的返回也涉及到字符串的拷贝了，所以效率也是一般般

```java
@Override
public String toString() {
    List<T> returnLst = new ArrayList<>();
    for (T item : this) {
        returnLst.add(item.toString());
    }
    return "{" + String.join(", ", returnLst) + "}";
}
```



### Better toString: ArraySet.of

见识到了很新的东西，其实也说不上新，只是没见过 Java 中多参数的语法(￣_,￣ )

> Java 中类的静态方法无法访问到类的泛型，所以需要在静态方法中重定义一个泛型名称



```java
public static <IrrelevantWithT> ArraySet<IrrelevantWithT> of(IrrelevantWithT... stuff) {
    // 给出多个参数直接构造
    ArraySet<IrrelevantWithT> newSet = new ArraySet<>();
    
    for (IrrelevantWithT item : stuff) {
        newSet.add(item);
    }
    
    return newSet;
}
```



# Done!

第六章的待学内容就是这些，后续的 Legacy 章节应该不影响我先写完 proj1，然后再去看
