# The Deque API

两个都还差 `public boolean equals(Object o)` 和 `public Iterator<T> iterator()` 没有实现



### Q3: MaxArrayDeque

这部分是作为对 `comparable` 和 `comparator` 的单独练习

在 `ArrayDeque` 的基础之上添加两个新的方法，一个构造函数

- `public MaxArrayDeque(Comparator<T> c)`：根据给出的 `Comparator` 构造新的 `MaxArrayDeeque` 

- `public T max()`：根据 `Comparator` 返回最大值元素，如果为空返回 `null` 
- `public T max(Comparator<T> c)`：根据 `c` 返回最大值元素

在可能存在多个正确值的情况下，返回任何一个都可以，因为这个测试只检查正确性



### Q4: Deque Interface

这里要求在 `Deque` 接口实现一个默认的 `isEmpty()` 但是我的这个 `ArrayDeque()` 不太好改，因为涉及到空间分配了，除非这个 autograder 不会插入超过 8 个元素，否则会出错的



## Guitar Hero

使用 `deque` 数据结构来实现吉他的弦音



#### 声音算法

使用 `Karplus-Strong` 算法，因为只用到了接口，实际上使用哪个具体的数据结构都可以

1. 把 `deque` 中的每个元素用 `[-0.5 - 0.5]` 替换，双精度浮点数
2. 移除 `deque` 中的第一个元素，与下一个数求平均值 `0.996 * (get(first) + get(second)) / 2` 
   1. 提示使用 `removeFirst()` 和 `get()` 方法
3. 将计算得到的新值称为 `newDouble` 然后把该值添加到 `deque` 的末尾
4. 演奏第一个值 `StdAudio.play(oldFirst)` 然后无限循环上面的流程



所以能从执行流程中看到这个数列的空间使用其实是固定的，本质上是一个**流计算的处理** 

注意在完成 `GuitarString.java` 文件的时候对 `GuitarString` 类用 `0` 初始化

写完之后可以用 `testTic()` 简单测试一下



### Q5: GuitarString

数据结构分析：因为 `ArrayDeque` 的实现稍微复杂一点，所以还是用 `LinkedListDeque` 结构



Q：为什么生成随机数这一步和初始化要分开呢，直接生成含有随机数的数据结构不好吗

不知道为什么，估计只是为了测试，通过测试还是很简单的



运行 `testPluckTheAString` 真的听到声音了，真的好有意思啊hhhhhh



### Q6: GuitarHeroLite

提供一个图形化界面和用户（也就是我）去交互，接下来这部分并不会纳入评测，就是给你玩的

参考 `GuitarHeroLite` 写一个完整的 `GuitarHero` 程序来支持完整的声部

```java
// 模拟钢琴键盘
String keyboard = "q2we4r5ty7u8i9op-[=zxdcfvgbnjmk,.;/' "
```

因为之前实现的 `GuitarString` 对应一个音，所以实际上需要一个由 37 个 `GuitarString` 构成的数组



## Autograder

检查有点点严格的... 泛型只能写 `T` 甚至 `Type` 都不行，格式检查也出了好多问题

之前的迭代器忘了实现了.. （这个项目的分布有点点不合理的感觉呢）应该把第六章看完再回来测试