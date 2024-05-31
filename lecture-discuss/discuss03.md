# Scope, Static and Linked Lists

### Static Electricity

```java
public class Pokemon {
    public String name;
    public int level;
    public static String trainer = "Ash";
    public static int partySize = 0;

    public Pokemon(String name, int level) {
        this.name = name;
        this.level = level;
        this.partySize += 1;
    }

    // 注意这里并不是一共成员函数 没有 this 的概念
    public static void change(Pokemon poke, int level) {
        poke.level = level;
        level = 50;         // 这里修改的是传参的 level
        poke = new Pokemon("Voltorb", 1);
        poke.trainer = "Team Rocket";
    }

    // 成员函数 通过 this 表明当前的调用对象
    public void printStats() {
        System.out.println(name + " " + level + " " + trainer);
    }

    public static void main(String[] args) {
        Pokemon p = new Pokemon("Pikachu", 17);
        Pokemon j = new Pokemon("Jolteon", 99);
        System.out.println("Party size: " + Pokemon.partySize);     // 1

        p.printStats(); // Pikachu 17 1

        int level = 18;
        Pokemon.change(p, level);

        p.printStats(); // Voltorb 1 1

        Pokemon.trainer = "Ash";
        j.trainer = "Brock";

        p.printStats(); // Voltorb 1 1
    }
}
```



这个输出有点出乎我的意料了... 四个输出后有三个完全理解错误(‾◡◝)

##### Q1: 对类变量不敏感

这里用类变量修饰的无论是通过实例还是类名去访问，都是在访问同一个变量

```java
public static int partySize = 0;        // 注意是 类变量
instanceX.partySize += 1;
instanceY.partySize += 1;

System.out.println("Party size: " + Pokemon.partySize);	// 2
```



##### Q2: 同理，类变量的处理

无论通过哪个实例去修改类变量，所有实例对应的类变量都会被修改，因为访问的都是同一个内存地址



##### Answer

```java
public class Pokemon {
    public String name;
    public int level;
    public static String trainer = "Ash";   // 声明的宝可梦的统一训练师
    public static int partySize = 0;        // 注意是 类变量 统计声明过的宝可梦的数量

    public Pokemon(String name, int level) {
        this.name = name;
        this.level = level;
        this.partySize += 1;
    }

    // 类函数 没有 this 的概念
    public static void change(Pokemon poke, int level) {
        poke.level = level;
        level = 50;         // 这里修改的是传参的 level

        // 修改的其实是指向 Pikachu 的指针副本
        poke = new Pokemon("Voltorb", 1);
        poke.trainer = "Team Rocket";   // 把类中的 trainer 全部修改为 Team Rocket
    }

    // 成员函数 通过 this 表明当前的调用对象
    public void printStats() {
        System.out.println(name + " " + level + " " + trainer);
    }

    public static void main(String[] args) {
        Pokemon p = new Pokemon("Pikachu", 17);
        Pokemon j = new Pokemon("Jolteon", 99);
        System.out.println("Party size: " + Pokemon.partySize);     // 类变量 static 无论是通过实例或者类去访问都是同一个变量

        p.printStats(); // Pikachu 17 Ash

        int level = 18;
        Pokemon.change(p, level);   // 因为 change() 是非成员函数 传递的 poke 是值传递 只是复制了一份指向 Pikachu 的指针

        // 修改了传参的 p 的副本 poke 指向 但是原本的 p 没有变化 所以这里输出的 name 仍然是 pikachu
        p.printStats(); // pikachu 18 Team_Rocket
        System.out.println(Pokemon.partySize);  // 但是因为新声明了 pokemon 数量统计还是会变化
        
        Pokemon.trainer = "Ash";
        j.trainer = "Brock";

        p.printStats(); // Voltorb 1 1
    }
}
```



### To Do List

画指针盒子图

```java
StringList L = new StringList("eat", null);
L = new StringList("should", L);
L = new StringList("you", L);
L = new StringList("sometimes", L);
StringList M = L.rest;
StringList R = new StringList("many", null);
R = new StringList("potatoes", R);
R.rest.rest = R;
M.rest.rest.rest = R.rest;
L.rest.rest = L.rest.rest.rest;
L = M.rest;

// L: eat many potatoes many potatoes many...
```

![](./LMR.png)



### Helping Hand Extra

注意对**哨兵的处理：**第一个元素不能访问

```java
public class SLList {
    private static class Node {
        int item;
        Node next;
    }

    Node sentinel;

    public SLList() {
        this.sentinel = new Node();
    }

    private int findFirstHelper(int n, int index, Node curr) {
        // Q: 为什么需要一个 Helper Function
        // A: 因为外面套着那层 SLList 并不是递归结构 必须通过内部 Node 来得到
        if (curr == null) {
            return -1;
        }
        if (curr.item == n) {
            return index;
        } else {
            return findFirstHelper(n, index + 1, curr.next);
        }
    }

    public int findFirst(int n) {
        // return the index of the first node equal n
        // return findFirstHelper(n,0,this.sentinel);
        return findFirstHelper(n,0,this.sentinel.next);
    }
}
```



# Done!

这个比较简单，就是有一些地方仍然出错了，哨兵，值传递还有类变量的地方，不能马虎dui'dai