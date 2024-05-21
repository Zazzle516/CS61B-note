# Introduction to Java

### Our first Java Prog

这个我第一眼看过去没有太明白，瞄了一眼解答，应该判断执行语句会不会报错

```java
int size = 27;
String name = "fido";
Dog myDog = new Dog(name, size);	// 假设参数数量是正确的情况下 应该传入具体的参数
...;
// 其实没啥好说的 就简单看一看吧
```



### Mystery

```java
public static int mystery(int[] inputArray, int k) {
    int x = inputArray[k];
    int answer = k;
    int index = k + 1;
    while (index < inputArray.length) {
        if (inputArray[index] < x) {
            x = inputArray[index];
            answer = index;
        }
        index += 1;
    }
    return answer;
}
// 找到在 k 后的最小元素的下标(包括 k )
```



```java
public static void mystery2(int[] inputArray) {
    int index = 0;
    while(index < inputArray.length) {
        // 获取最小元素的下标位置
        int targetIndex = mystery(indexArray, index);
        
        // 交换位置
        int temp = inputArray[targetIndex];
        inputArray[targetIndex] = inputArray[index];
        inputArray[index] = temp;
        
        // 迭代
        index += 1;
    }
}
```



### Write my First Java Prog

实现 `Fib()` 函数，使用递归和迭代方式

```java
// 递归方式
public static int fib(int n) {
    if (n <= 1) {
        return n;
    }
    return fib(n - 1) + fib(n - 2);
}
```



```java
public static int fib(int n) {
    int pre = 0;
    int current = 1;
    int sum = 0;
    while(n) {
        sum, pre = pre + current, current;
    }
    return sum;
}
```























