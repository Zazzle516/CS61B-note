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



## Guitar Hero