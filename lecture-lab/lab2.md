# JUnit Tests and Debugging

## Poject Setup

### Import

1. 打开 IDEA 界面，`import Project` 
   1. 新版本的 IDEA 好像没有，要在 `open Project` 后选择 `Project Settings` 
   2. 选择 `Modules` 再选择 `Import Module` 导入
2. 选择对应的项目文件夹，一直选择 `next` 即可
3. 在 `Import Project` 界面选择 `Create project from existing sources` 
4. 可以看到设置 `Project name` 之类的界面，直接 `next` 
5. `next` 
6. 可能出现一个 Libraries 的对比界面，如果没有也没关系，直接 `next` 
7. 可以看到一个 `Modules` 和 `Module dependeccies` 界面，也是 `next` 
8. 这里强调如果是 JDK 11 的话，可以跳过 step_9 和 step_10 否则按步执行
9. 在左上角选择 `+` 来选择 JDK
10. 找到本机中 JDK 的存储位置，确定好后，只要界面像 step_8 就是成功了
11. 点击 `finish` 



### Getting Java Libraries

这里因为我不是 UCB 的学生，所以没有 SPEC 上说明的 `s***` 仓库，直接在当前的仓库路径下打开 `cmd` 

```shell
git submodule update --init
```

```shell
Submodule 'library-sp21' (https://github.com/Berkeley-CS61B/library-sp21) registered for path 'library-sp21'
Cloning into 'D:/CS61B/library-sp21'...
Submodule path 'library-sp21': checked out 'c4275362bc496eea32ac8142b3806ca6bd1978e2'
# 看到这些说明成功了
```

接着打开 `project structure` 选择 `SDK` 添加 `D:\CS61B\library-sp21\javalib` 注意一定是整个文件夹

不然无法运行测试函数



## Debugging

重复上面的项目导入步骤，打开 `lab2/pom.xml`，针对我的情况，记得把 `Language Level` 设成 15 不然报错

这一大段的 SPEC 就是提示要科学的使用 IDEA 的 debugger 让它发挥最强大的功能

核心思想：断点 + 执行下一步



### Breakpoints and Step_Into

说实话我这段程序看了半天，也 Debug 了，愣是没找到哪里出问题，不过是猜 `Math.round()` 有问题

去看了一下 `Math.round()` 感觉就是传参有问题，别的也看不出来了

```java
// 对双精度型进行保留两位小数的四舍五入	这个函数的四舍五入比较特别
public long Math_round(double x);

import java.io.*;
import java.lang.*;
import java.util.*;

public class Main {
    public static void main(String[] args) {
        double d = 7435.9876;
      	// Math.round(743598.76) => 743599
    	double roundDbl = Math.round(d * 100.0) / 100.0;
    	System.out.println("Rounded Double value: " + roundDbl);
  }
}

// Rounded Double value: 7435.99
```

[Java中Math.round()方法_java math.round-CSDN博客](https://blog.csdn.net/weixin_44170221/article/details/104731124) 



### Step_Over and Step_Out

在 Debug 的时候也要依赖抽象性，执行 `step_over` 可以执行一个函数而不用去看它的内部过程（体现了抽象..）

此事在 `DebugExercise2` 中亦有记载，接收两个数组作为参数，选择相同位置数值更大的拼成一个新数组

```java
{2, 0, 10, 14};
{-5, 5, 20, 30}
{2, 5, 20, 30};		sum => 57
```

SPEC 提示在提供的代码中有两个不同的 bug，我的任务是修复它，在 Debug 的过程中不能进入 `max` 或 `add` 

（因为这些基础方法往往实现起来很奇怪，如果不小心进入了，执行 `step_out` 退出）

SPEC 说如果发现这两个基础方法如果有一个有 bug，应该完全重写它，说的就是你，`max()` 



### More Debug Guidance

#### Error Stack Trace

```shell
Exception Error: Exception place
[Which thread errored] [What error occurred] [Any more information about the error]
# 其实只需要关心 top line
```



#### What does this Error mean

![](./../../../CS61B/61B_pics/Java_Eception.png)



#### Conditional Breakpoint

在代码行上打上断点之后，然后右键点击这个断点，可以看到一个断点窗口，最下行有 `Condition` 

根据这行代码的执行结果去设置



#### Top Toolbar

- **Drop Frame**：

  This allows you to reset the current frame by returning to the previous frame where it was called. This is useful if you missed the part of a function you were trying to see by essentially letting you rewind time.

- **Run to Cursor**：

  If you are running in debug mode, you can quickly jump to areas of interest in the code by clicking run to cursor. This will act as if there is a temporary breakpoint set wherever your cursor is.



#### Left Toolbar

没啥好说的其实，这栏除了重启 Debug 和继续执行，其他不怎么用



#### Debug View

主界面分成两列，左侧基本是执行的函数帧，也能看到正在执行的函数帧（这一点在写 CA61A 的 scheme 已经体会了，尤其是在 Debug OPTIONAL 问题的时候）

右侧查看当前执行的函数帧的变量

善用 `Evaluate`，因为不会改变原有的程序执行流



### JUnit and Unit Testing

现在把目光转向单元测试，是一种很严格的去检验单个函数的方法，保证你的项目最终有效运行

> Each method should only do one thing

[61B 2019 Lecture 7 - Testing - YouTube](https://www.youtube.com/playlist?list=PL8FaHk7qbOD4ZfVY8g8lo5dFrLP-ctGmT) 有点长啊.. 这么多视频

我终于看完全部的 12 个视频回来了_〆(´Д｀ )



#### JUint Syntax

```java
import static org.junit.Assert.*;
import org.junit.Test;

@Test
public void testMethod() {
    assertEquals(<expected>, <actual>);
}
```

有一些语法要求，视频里也提了，`non-static` 并且需要 `@Test` 修饰



### Running JUint in IDEA

强调一下 Java 的 JUnit 测试用例，一旦出现一个错误，同一个 JUnit 里面的测试函数就不能再往下执行了

```java
@Test
public void testSum() {

    assertEquals(11, Arithmetic.sum(5, 6));     // Error
    assertEquals(-1, Arithmetic.sum(5, -6));	// not executed
    assertEquals(-6, Arithmetic.sum(0, -6));
    assertEquals(0, Arithmetic.sum(6, -6));
}
// 独立的 @Test 函数还是可以继续执行的
```

修一下 `Arithmetic.java` 的 bug



### Application: IntLists

在 textbook 的 Chap1.2 中，讨论了 `IntList` 的实现，在 `IntListExercise.java` 中有 3 个方法，全都有 bug



#### Starter Code

在 `IntList Class` 中，添加了 `toString()` 和 `of` 方法，他这个 `IntList.of()` 的实现还挺有意思的



#### Part A: IntList Iteration

在这部分，主要处理在 `IntListExercise.java` 中的 `addConstant()` 方法，接收一个 `IntList` 作为参数，为列表中的每一个元素增加一个常量

错误的地方在于，没有处理 `IntList` 的最后一个元素，估计是循环的判断条件有问题，嗯，改好了



#### Part B: Nested Helper Methods

这部分处理 `IntListExercises.java` 中的 `setToZeroIfMaxFEL()` 方法，这个方法纯粹是为了练习设计出来的

替换这个列表中某些位置的值，每处理完一个位置会更新这个列表的有效区域

```java
[55 -> 22 -> 45 -> 44 -> 5]
[ 0 -> 22 -> 45 -> 0 ->  0]

[5 -> 535 -> 35 -> 11 -> 10 -> 0]
[0 ->   0 -> 35 ->  0 -> 10 -> 0]
// 和最大值的元素位置没有关系
```

执行顺序：

1. 读取当前列表的最大值，55，首位数字和尾位数字一致，替换
2. 更新列表有效区域
3. 读取当前列表的最大值，45，不替换
4. 更新列表有效区域
5. 读取当前列表的最大值，45，但首位数字和尾位数字不一致，不替换
6. 更新列表有效区域
7. 读取当前列表的最大值，44，首位数字和尾位数字一致，替换
8. 更新列表有效区域
9. 读取当前列表的最大值，5，首位数字和尾位数字一致，替换

项目文件本身提供了一些 `helper method` 比如 `firstDigitEqualsLastDigit()` 和 `max()` 

SPEC 指导了在这种情况，可以把函数拆分出来，分开执行，看看每步之后的执行结果

```java
int currentMax = max(p);
boolean firstEqualsLast = firstDigitEqualsLastDigit(currentMax);
if (firstEqualsLast) {
    p.first = 0;
}
// 查看到底是哪个函数执行的有问题	(这把是不是你打的有问题.jpg)
```

改还是很简单的



#### Part C: Tricky IntLists!

这个部分解决 `IntListExercise.java` 中的 `squarePrimes()` 方法，接收一个列表参数，平方所有的素数

如果列表中有至少一个素数，返回 `true` 否则 `false`，提供了 `Primes.isPrime(int x)` 这样的辅助函数

> Fermat Primality Test （在 Debug 的时候不需要关心）

SPEC 推荐的改 bug 行为：

1. 编写针对 `squarePrimes()` 的测试用例
2. 运行所有你编写的测试用例，直到发现一个发生 `fail` 的，然后开始 Debug
3. 改好 bug，找到 bug 的过程最重要

观察代码可以发现这是一个递归实现，第一个就给我试出来了我好幸运hhh，就是代码提前结束的问题

除了额外添加一个变量的参数，想不到太好的解决办法呢，但是 SPEC 又说修改不需要几行，改一下判断

```java
// 注意判断顺序也会有影响 这个涉及到 Java 对 && || 之类的运算符的计算
// 考的是蛮细节的东西
return !squarePrimes(lst.rest)  && currElemIsPrime;
```



啊，提交上去有两个测试没过，淦

最后还是用一个辅助函数实现了，有点点不爽



# Done!

看看有没有什么简洁的方法

```java
/**
 * Part C: (Buggy) mutative method that squares each prime
 * element of the IntList.
 *
 * @param lst IntList from Lecture
 * @return True if there was an update to the list
 */
public static boolean squarePrimes(IntList lst) {
    // Base Case: we have reached the end of the list
    if (lst == null) {
        return false;
    }

    boolean currElemIsPrime = Primes.isPrime(lst.first);

    if (currElemIsPrime) {
        lst.first *= lst.first;
    }
	
    // 所以说还是执行顺序的问题... 有被自己蠢到
    return squarePrimes(lst.rest) || currElemIsPrime;
}
```



