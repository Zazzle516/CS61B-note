# Scope, Pass-by-Value, Static

### Give'em the ’Ol Switcheroo

主要讨论了 Java 传递形参的问题，Java 的对象传递（数组，类，接口）是引用传递，原始数据类型是值传递

```java
// 引用传递
switcheroo(foobar, baz);	// not switch	Error
fliperoo(foobar, baz);		// switch
swaperoo(foobar, baz);		// 对比 fliperoo() 此处并没有新构建 Foo Class
							// 所以这里其实是把 a 同化为 b		(这个地方答案出错了)

public class Foo {
    public int x, y;

    public Foo(int x, int y) {
        this.x = x;
        this.y = y;
    }


    public static void switcheroo(Foo a, Foo b) {
        // a, b 其实是指向对应 object 的指针 修改的是指针的指向
        // 在 main() 里面再去访问的时候并没有被改变
        // 对比修改 a.x a.y 真的可以修改内容	可以看到差别
        Foo temp = new Foo(a.x, a.y);
        a = b;
        b = temp;
        System.out.println(a.x);
    }

    public static void swaperoo (Foo a, Foo b) {
        Foo temp = a;
        a.x = b.x;
        a.y = b.y;

        b.x = temp.x;
        b.y = temp.y;
    }

    public static void main(String[] args) {
        Foo foobar = new Foo(10, 20);
        Foo baz = new Foo(30, 40);
        // foobar.x: ___ foobar.y: ___ baz.x: ___ baz.y: ___
        switcheroo(foobar, baz);
        swaperoo(foobar, baz);
        System.out.println(foobar.x + foobar.y + baz.x + baz.y);
    }
}
```



### Quick Maths

针对数组的传参方式的判断

```java
public class QuickMath {
    public static void mulitplyBy3(int[] A) {
        for (int x: A) {
            // 其实这个增强 for 循环本质是先声明了 x 一个变量
            // 每次迭代器调用 next() 方法把下一个元素传递给 变量x
            // 对 变量x 的处理已经脱离 数组A 所以无法修改原数组
            x *= 3;
        }
    }
    
    public static void mulitplyBy2(int[] A) {
        int[] B = A;
        for (int i = 0; i < B.length; i ++) {
            B[i] *= 2;
        }
    }
    
    public static void main(String[] args) {
        int[] arr;
        arr = new int[]{1, 2, 3, 4};
        mulitplyBy3(arr);	// {1, 2, 3, 4}	注意这里未改变

        arr = new int[]{2, 3, 3, 4};
        mulitplyBy2(arr);	// {4， 6， 6， 8}
        
        // 有一个 swap 的例子没有敲进来 根本没必要 因为不会被交换
    }
    
}
```



### Static Books

能看出这是一个互相调用的构造函数了（一直很好奇这个互相调用是怎么实现的）

通过 cmd 执行 `javac XX.java` 不能包含中文字符

```java
class Library {
    public Book[] books;
    public int index;
    public static int totalBooks = 0;

    public Library(int size) {
        books = new Book[size];
        index = 0;
    }

    public void addBook(Book book) {
        books[index] = book;
        index++;
        totalBooks++;
        book.library = this;
    }
}

class Book {
    public String title;
    public Library library;    // 声明这本书
    public static Book last = null;

    public Book(String name) {
        title = name;
        last = this;
        library = null;
    }

    public static String lastBookTitle() {
        return last.title;
    }

    public String getTitle() {
        return title;
    }
}
```



#### Modification

判断下面哪些修改会报错

1. Change the totalBooks variable to non static
   1. 在 `addBook` 方法里面确实 OK 只是说从设计逻辑的角度是错误的
2. Change the `lastBookTitle` method to non static
   1. Possible：Non-pointer Error	但是可以通过编译 因为是通过静态的类变量访问的
3. Change the `addBook` method to static
   1. Error：涉及访问 index 所以报错
4. Change the last variable to non static
   1. Error：`lastBookTitle()` 无法正常执行
5. Change the library variable to static
   1. Correct：没啥影响



#### Output

```
1 public class Main {
2 public static void main(String[] args) {
3 System.out.println(Library.totalBooks); _______0______________
4 System.out.println(Book.lastBookTitle()); ______Error: NUllPointerException_______
5 System.out.println(Book.getTitle()); ________Error_____________

Book goneGirl = new Book("Gone Girl");
Book fightClub = new Book("Fight Club");

10 System.out.println(goneGirl.title); ________"Gone Girl"_____________
11 System.out.println(Book.lastBookTitle()); _________"Fight Club"____________
12 System.out.println(fightClub.lastBookTitle()); __________"Fight Club"___________
13 System.out.println(goneGirl.last.title); _________"Fight Club"____________

15 Library libraryA = new Library(1);	// 声明当前 Lib 容纳的 Book 容量 初始化 index = 0
16 Library libraryB = new Library(2);
17 libraryA.addBook(goneGirl);

19 System.out.println(libraryA.index); __________1___________
20 System.out.println(libraryA.totalBooks); _________1____________
21
22 libraryA.totalBooks = 0;

23 libraryB.addBook(fightClub);
24 libraryB.addBook(goneGirl);
25
26 System.out.println(libraryB.index); __________2___________
27 System.out.println(LibraryB.totalBooks); __________2___________
28 System.out.println(goneGirl.library.books[0].title); _________"Fight Club"_________
29 }
30 }
```

