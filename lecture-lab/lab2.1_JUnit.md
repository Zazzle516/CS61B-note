# Testing Lecture Video

对这一系列视频做个笔记

## Video 1: A new Way

#### How does a programmer Know their Code Work

显然，在课程里通过 MVP：Autograder 可以判断我们的代码是否能通过，当然现实中没有

你在现实中写了一个项目是没有办法知道它能不能正常运行的

首先，核心，最重要的：没有能完美运行的程序，只能尽可能去完善



#### Write a Sorting Method

写代码的新方式：自己写测试例子（yue~）

有的时候可以先写测试代码然后再写具体的代码内容，反而会让思路更清晰



## Video 2: Ad Hoc Testing

在视频中他写测试用例的时候，用到 `input != expected` 这样的比较方式，这就涉及到 Java 的比较

因为 `input` 和 `expected` 都是字符串数组指针，指向某个地址，即使指向的内容相同也会认为不同

```java
String[] input = {"a"};
String[] expected = {"a"};

if (input != expected) {
    System.out.println("I'm here!");	// 进入该分支
}

if (!java.util.Arrays.equals(input, expected)) {
    System.out.println("Correct!");
}
```

> 所以说实话，测试用例本身也可能写 bug，这事就很无解
>
> 测试用例的报错也要写的有一定程度的说明性（通过 `System.out.println()` 的方式写挺烦的）



## Video 3: JUnit

> Ad Hoc Testing is reallyyyyyyy Tedious!!

通过 `JUnit` 提供的 `org.junit.Assert.assertEquals()` 方法可以写非常简洁的测试代码，大大避免了错误



## Video 4: How Sort Works

这里重点介绍的是 Selection Sort （选择排序）



#### Vedio 5: 选择排序

每次找到未排序部分的最小值，移动到未排序部分的最前端，更新未排序部分

通过交换元素，至少比移动元素的效率高

```java
public class Sort {
    // 哪怕是这个简单的,小小的,交换函数 也能拆解出多个辅助函数实现抽象
    public static void sort(String[] x, int index) {
        if (index == x.length) {
            return;
        }
        // Find the smallest item
        int smallest = findSmallest(x, index);
        
        // Swap item
        swap(x, index, smallest);
        
        // 目前只支持 single step 想办法扩大到整个列表
        // 如何在不修改 method signature 的情况下完成递归(我是只想到改 signature 的方法)
        // 切 果然还是需要第二个参数 不然 Java 实现不了
        sort(x, index + 1);
    }
    
    public static int findSmallest(String[] x, int index) {
        // 理论上 这个函数应该是 private 但是在 private 权限下无法满足测试类的访问
        int indexSmallest = index;	// 记录最小值下标
        for (int i = index; i < x.length; i++) {
            // 实际上是不能这么写的 java 无法通过符号比较字符串
            // if (x[i] < x[indexSmallest]) {  
            if (x[i].CompareTo(x[indexSmallest]) < 0) {
                // str1.CompareTo(str2) 如果 str1 < str2: -1	str1 > str2: +1
                indexSmallest = i;
            }
        }
        // 不测试完一个小步骤不进入下一步
        return indexSmallest;
    }
    
    public static void swap(String[] x, int a, int b) {
    	String temp = x[b];
        x[b] = x[a];
        x[a] = temp;
    }
}
```



## Video 10: Reflections on Process

现实世界中也会遇到反复重构代码的事情（比如我在 Proj0）

针对某个函数提供的小测试用例可以放松大脑，不用关心那么宏大的项目结构，只要确保这个函数 OK 就可以

确定每个函数都可以正常运行之后，再去关心整个项目上下文结构

但是自己写测试类，然后在测试类中针对某个特定函数测试反复注释查看是否正确也挺烦的



## Video 11: Better JUnit

视频里提到在 lab3 中能看到更详细的 JUint 的使用

使用 `Annotate` 用 `@org.junit.Test` 进行标识，写在函数签名上面的一行，就不需要在测试类中写 `main` 函数



#### JUint Syntax

- `Non-static` 方法
- 用 `@org.junit.Test` 标识（当然，如果提前执行 `import` 就不需要声明包名）



## Video 12: Testing Philosophy

#### Autograder Driven Development

一定不要尝试这种方法！！！，完成项目的最糟方式：

1. 阅读并尽量搞懂 SPEC
2. 写整个项目
3. 得到一大堆语法错误
4. 修改后使用 Autograder，获得很多执行错误（hhhh，就简直就是我呜呜呜呜呜呜
5. 重复上个步骤... ￣へ￣

视频这里应该是推荐自己去写测试用例试一试



#### Test Driven Development

在现实世界这个想法还是很流行的

- Identify a new feature
- Write a unit test first for that feature
- Run the test (which should fail) RED
- Write code until it become GREEN
  - Make it better



#### Integration Testing

整合测试，包含多个 units 测试

参考在 Proj0 的测试过程，先测试单个函数，再测试整个项目