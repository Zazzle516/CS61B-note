# Extra Credit: Autograding

### Task I

新建一个 `TestArrayDequeEC.java` 文件，随机调用 `StudentArrayDeque` 和 `ArrayDequeSolution` 方法，直到报错（提示使用 `StdRandom` 方法生成随机数

在这个测试文件中不需要测试 `equals(Object o)` 和 `iterator()` 方法

并且 SPEC 要求泛型特化为整数类型，调用 `addFirst()`，`addLast()`，`removeFirst()` 和 `removeLast()` 方法就能找到一个错误（但是 SPEC 声明这个错误不能是 `NullPointerException` 错误

确保自己写的测试用例不会从一个空数列中移除元素，并且用 `Integer` 类型作为接收类型

（因为只有引用类型 `Integer` 可以为空，直接用 `int` 接收是不能为空的）



#### Example

死活过不去了艹，有点点绝望，不知道是哪里的问题，已经用 `Integer` 接收了，但是本地显示的显示的报错和线上评测的报错不同，我也没有办法，看来看别人怎么写的

- 用随机数的方法去测试每一种方法，我用 3 个方法检测到一个错误之后就没有继续
- 打印错误信息不完整，答案是累加的形式，我只打印了一行（虽然这个不是主要错误...