# HW 0: A Java Crash Course

这个 homework 的文档主要是介绍一些 Java 的基本语法，针对没有什么 Java 基础的（比如我



### A Basic Program

[Java Visualizer (uwaterloo.ca)](https://cscircles.cemc.uwaterloo.ca//java_visualize/#mode=display) https://pythontutor.com/（好耶，是 Java 的版本

（好好笑，可以试试错误代码，但是会安慰你电脑不会爆炸

```java
// 是第二个偏复杂的版本
class Person {
  private String name;
  public Person(String theName) {
    this.name = theName;
  }
}

public class Student extends Person {
  private int id;
  public static int nextId = 1;
  public Student(String theName) {
    super(theName);
    id = nextId++;
  }

  public static void main(String[] args) {
    Person e1 = new Student("Alice");
    Person e2 = new Student("Bob");
    Person e3 = new Student("Carol");
    Person arrayOfPeople[] = {e1, e2, e3};
  }
}
```



### Conditionals

介绍了一下 `if-statement` 和 `switch-statement` 的语法（感觉 14 年的老师不是很友好，好严厉啊... 

强调了一下花括号的作用范围对 Java 的重要性



### Curly Brace Standards

从编程规范上，虽然只有一行的执行语句不需要花括号，但还是要加上



### Creative Exercise 1

打印五行数量递增的符号（经典题目了其实

```java
public class PrintSymbol {
    public static void main(String[] args) {
        int line = 5;
        int symbol = 1;
        for(int i = 0; i < line; i++) {
            for(int j = 0; j < symbol; j++) {
                System.out.print("*");
            }
            symbol += 1;
            System.out.println();
        }
    }
}
```



### Defining Functions(Methods)

Java 语法要求必须明确的定义返回值的类型

**Signature**：`public static int max(int x, int y)` 包含参数，返回值类型，方法名称和修饰词



### Creative Exercise 2

写一个新方法 `drawTriangle()` 并且定义返回值为 `void` （不返回任何内容

接收一个形参为 N 的参数，联系 Creative Exercise 1 的代码内容，定义执行行数

```java
public class ClassNameHere {
   public static void drawTriangle(int N) {
        int symbol = 1;
        for(int i = 0; i < N; i++) {
            for(int j = 0; j < symbol; j++) {
                System.out.print("*");
            }
            symbol += 1;
            System.out.println();
   		}
   }
   
   public static void main(String[] args) {
      drawTriangle(10);
   }
}
```



### Arrays

Java 的数组定义我一直都不太会...

```java
// 定义长度已知的数组
int[] list_name = new int[3];

// 根据元素数量定义数组
int[] list_name = new int[]{a, b, c,...};

// 获取列表长度
list_name.length;
```



### Exercise 3

注意列表传参的方式 `type[] list_name_formal` 

```java
public class ClassNameHere {
    /** return the max number from m */
    public static int max(int[] m) {
        int max_num = 0;
        for(int i = 0; i < m.length; i++) {
            if(m[i] > max_num){
                max_num = m[i];
            }
        }
        return max_num;
    }
    
    public static void main(String[] args) {
        int[] numbers = new int[]{9, 2, 15, 2, 22, 10, 6};
        int result = max(numbers);
        System.out.print(result);
    }
}
```



### Break & Continue

Continue 会跳过循环体中的剩余部分，而 Break 会直接跳出循环体

```java
public class ContinueDemo {
    public static void main(String[] args) {
        // 注意字符串的定义和初始化方式
        String[] a = {"cat", "dog", "laser horse", "horbse", "horse"};
        
        for(int i = 0; i < a; i ++) {
            if(a[i].contains("horse")) {
                continue;
            }
            for(int j = 0; j < 3; j++) {
                system.out.println(a[i]);
            }
        }
    }
}

// break 和 continue 同样可以在 while-loop 和 do-while 中作用
```



### Exercise4

题目提示说这道题有点挑战性（让我来瞧瞧.jpg

完成一个函数 `WindowPosSum(int[] a, int n)` ，求 `a[i]` 到 `a[i + n]` 的和来替换每一个` a[i]` 的值，但是这个只作用在 `a[i]` 为正数的时候（如果求到最后的元素不足以满足 n 的条件，这个时候尽可能的累加就可以

```java
windowPosSum({1, 2, -3, 4, 5, 4}, n = 3);
a[0] = a[0] + a[1] + a[2] + a[3];
a[1] = a[1] + a[2] + a[3] + a[4];
a[2] = a[2];
...
```

题目给了一些提示：

- 使用两层 `for-statement` 循环
- 使用 `continue` 来跳过负数值
- 使用 `break` 来防止访问越界



```java
public class BreakContinue {
  // 类似于滑动窗口的取值 这个窗口的大小是 4
  public static void windowPosSum(int[] a, int n) {
      // 修改发生在数组内部 因为并没有返回任何内容
      int list_len = a.length;
      for(int i = 0; i < list_len; i++) {
          if(a[i] > 0) {
              for(int j = 1; j <= n; j++) {
                  if((i + j) >= list_len) {
                      break;
                  }
                  a[i] += a[i + j];
              }
          }
          else {
              continue;
          }
      }
  }

  public static void main(String[] args) {
    int[] a = {1, 2, -3, 4, 5, 4};
    int n = 3;
    windowPosSum(a, n);

    // Should print 4, 8, -3, 13, 9, 4
    System.out.println(java.util.Arrays.toString(a));
  }
}
```



### Enhanced Loop

Java 同样支持 Iteration（现在感觉 Iteration 真的是很强大的特性），主要是针对一些不关心 Index 的循环

```java
public class EnhancedForBreakDemo {
    public static void main(String[] args) {
        String[] a = {"cat", "dog", "laser horse", "ketchup", "horse", "horbse"};

        for (String s : a) {
            for (int j = 0; j < 3; j += 1) {
                System.out.println(s);
                if (s.contains("horse")) {
                    break;
                }                
            }
        }
    }
}
```



# Done!

emm.. 这节 hw00 没什么好说的，就是最基础的 Jaa 语法









