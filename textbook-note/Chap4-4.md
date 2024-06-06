# Abstract Data Types

总结一下之前的学习，尤其是接口的应用

- 为不同的类定义一些通用功能接口
- 通过接口继承来对用户伪造一个相同但本质不同类实现可能不同的世界
- 通过内部类实现接口构造类的不同功能
- ......

然后，视频引入了 栈 这个抽象数据结构，每次执行 `push()` 都会有一个新元素放到栈顶，执行 `pop()` 弹出

通过让链表的指针指向栈顶，链表的实现方式是会比数组好一些的，因为没有空间分配的损耗



假设实现 `GrabBag` 这个数据结构，支持的行为基本是插入元素，移除元素，选取元素，都是随机的

这种情况下呢，用数组实现就会更好，因为长度已知的时候就可以轻松获得一个随机数，常数复杂度



在 Java 中，`Deque` 实际上是一个接口，定义实现了 `Deque` 所有方法的都可以称为 `Deque`，数据结构只和行为绑定，所以是抽象的



# Java libraries

Java 已经有一套官方的，非常好用的抽象数据结构类型，以包的形式存储在 Java 的库中

### Java.util Library:

List：一组有序的元素集合，比如 `ArrayList` 

Set：一组无序的不重复的元素集合，比如 `HashSet` 

Map：一组键值对关系集合，比如 `HashMap` 

这三个算是最重要的三个了，一定要好好掌握



### Exercise

#### MyVersion

```java
import java.util.*;
import java.io.File;
import java.io.FileNotFoundException;

public class FileToList {
    public static void getWord(String fileName) {
        ArrayList<String> lst = new ArrayList<>();
        int index = 0;
        try {
            File myFile = new File("src/" + fileName);
            Scanner myReader = new Scanner(myFile);
            while (myReader.hasNext()) {
                lst.add(myReader.next());
                System.out.println(lst.get(index));
                index += 1;
            }
            myReader.close();
            int unique = countUniqueWords(lst);
            System.out.println(unique);

        } catch (FileNotFoundException e) {
            System.out.println("FileNotFoundException");
            e.printStackTrace();
        }
    }

    public static int countUniqueWords(List<String> lst) {
        // 统计不重复字符串 遍历一遍 添加所有元素
        HashSet<String> mySet = new HashSet<>(lst);
        return mySet.size();
    }
    
    private static int getNum(String target, List<String> words) {
        int count = 0;
        for (String word : words) {
            if (target.equals(word)) {
                count ++;
            }
        }
        return count;
    }

    public static Map<String, Integer> collectWordCount(List<String> targets, List<String> words) {
        // 查看 targets 中每个单词在 words 出现的次数    可以用 HashMap 来实现
        Map<String, Integer> myMap = new HashMap<>();
        for (String target : targets) {
            int count = getNum(target, words);
            myMap.put(target, count);
        }
        return myMap;
    }

    public static void main(String[] args) {
        Scanner inputFileName = new Scanner(System.in);
        String fileName = inputFileName.nextLine();
        getWord(fileName);
    }
}
```



#### Ans Version

相比我的来说，在指针的声明上更系统化一些，我写的代码版本有点像拼凑起来的（确实），没有系统性

而且这个版本的 API 调用比我强多了呜呜呜呜呜呜（统一用父类指针去指向子类对象）

```java
public static List<String> getWords(String inputFileName) {
    List<String> lst = new ArrayList<String>();
    In in = new In();
    while (!in.isEmpty()) {
        // optionally, define a cleanString() method that cleans the string first.
        lst.add(in.readString());
    }
    return lst;
}

public static int countUniqueWords(List<String> words) {
    Set<String> ss = new HashSet<>();
    for (String s : words) {
        ss.add(s);
    }
    return ss.size();
}

public static Map<String, Integer> collectWordCount(List<String> words) {
    Map<String, Integer> counts = new HashMap<String, Integer>();
    
    for (String t: target) {
        // 初始化
        counts.put(s, 0);
    }
    for (String s: words) {
        // 这里的统计是利用 counts.hash 实现的 比我的巧妙多了
        if (counts.containsKey(s)) {
            // 防止可能没有对应 key 值
            counts.put(word, counts.get(s)+1);
        }
    }
    return counts;
}
```



### Java v.s. Python

> 在 Python 里实现上面的功能要简洁太多
>
> Plus：I don't like Java

这里主要讲述一下 Java 与 Python 基本的不同，或者说造成了使用两种语言底层思考差异的原因

但是因为 Java 要求程序员指明类型之类的繁琐语法，不同的结构会有些许的差异，比如 `HashMap` 数据结构本身并没有任何顺序，但是如果声明使用 `TreeMap`，它就是根据字母排序的（所以取决于程序员的功力了

如果说对各种类型很熟练的话，Java 写起来也是很快的，程序结构也更清晰

```python
def find_word_count(words, targets):
    word_counts = {}
    
    # 初始化
    for s in targets:
        word_counts[s] = 0;
        
    for s in words:
        word_counts[s] += 1;
        
    return word_counts;
```



### Abstarct Interfaces

抽象只是声明该方法的存在，子类必须要对该抽象方法重写，之前在接口中定义的以 `;` 结尾的方法都是抽象方法

在接口中实际也可以写变量，`public static final` 权限

> 除非使用了 `default` 关键字可以写具体内容

一个类可以继承多个接口，实现多个接口的特点



### Abstract Classes

有点像类和接口的中间产物（intermediate），注意 Java 只允许单继承，所以一个类撑死了写一个抽象类

- **抽象类不需要实现抽象接口的所有方法** 
- 不可被实例化，因为是抽象的

- 可以同时提供抽象和具体方法

  - 针对抽象方法需要使用关键字 `abstract` 

  - 具体方法不需要任何关键字（这一点和接口相反）

- 可以提供任何类型的方法和变量



```java
public abstract class GraphicObject {
    public int x, y;
    
    public void moveTo(int newX, int newY) {
        ...
    }
    
    public abstract void draw();
    public abstract void reSize();
}
```



从接口继承的抽象类的抽象方法是可以累计的

```java
// PaperShredder <- DeluxeModel <- DCX9000
// DCX9000 最后有 2 个抽象方法需要实现

public interface PaperShredder {
    void shred(Document d);
    void shredAll(Document[] d);
}

public abstract class DeluxeModel implements PaperShredder {
    // 抽象类不需要实现抽象接口的所有方法
    public int count = 0;
    public void count() {
        return count;
    }
    
    public shredAll(Document[] d) {
        ...
    }
    
    public abstract void connectToWifi();
    
    // 总计 3 个抽象方法 shredAll() 实现还剩 2 个
}
```



#### Summary

推荐用接口而不是抽象类，主要是降低复杂度

抽象类相对于接口来说有一个好处就是提供了一个默认实现，子类当然可以修改，但是不改也能用默认方法



# Packages

在 Java 中通过 `web` 命名的方式对同类名文件进行区分

导入包的时候不要一次性全导入，因为模糊不清的化可能会造成冲突



# Done!

第四章终于学完了o(*￣▽￣*)ブ

