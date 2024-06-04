# Inheritance

### Raining Cats and Dogs

```java
public class TestAnimal {
    public static void main(String[] args) {
        Animal a = new Animal("Pluto", 10);
        Cat c = new Cat("Huh", 9);
        Dog d = new Dog("Fido", 4);

        a.greet();  // Animal Pluto says: Sound...
        c.greet();  // Cat Huh says: Huh?
        d.greet();  // Dog Fido says: Woof!

        c.play();   // Woo it is so much fun being an animal!
        c.play(":)");   // Woo it is so much fun being an animal! :)

        a = c;      // Compile Pass

        ((Cat) a).greet();           // Cast Pass: Cat Huh says: Huh?
        ((Cat) a).play(":D");   // Woo it is so much fun being an animal! :D

        a.play(14); // Compile Error

        ((Dog) a).play(12);     // Runtime Error: can't convert to Dog

        a.greet();      // Cat Huh says: Huh?

        c = (Cat) a;      // Compile Error: !is-a     在 line_14 a 已经指向 Cat 可以强制转换
    }
}
```



### An Exercise in Inheritance Misery

说是要划掉会造成编译错误的语句（但是我看他好像已经划掉了...，包括后续的运行错误，并看看最终的运行结果

我错了，我看的是 sol 文件，我还在想为什么把答案写出来了，我有罪2333

- 没有 `super.super` 的使用语法，跨越抽象是禁止的



```java
public class D {
    public static void main(String[] args) {
        A b0 = new B();
        System.out.println(b0.x);   // 5
        b0.m1();    // Am1-> 5
        b0.m2();    // Bm2-> 5

        ((B)b0).m2(61);  // Compile Error: Static Type Error    可以强制类型转换

        B b1 = new B();
        b1.m2(61);      // Bm2y-> 61
        b1.m3();           // Bm3-> called

        A c0 = new C();     // pass... Dynamic Type of c0 is C
        c0.m2();            // Cm2-> 5

        // C c1 = (A) new C(); // Compile Error: 子类指针指向父类对象
        A a1 = (A)c0;
        C c2 = (C)a1;       // 改回去了 肯定也 ok
		
        // C c2 = new C();
        c2.m3();            // Bm3-> called 调用父类方法
        c2.m5();            // Cm5-> 6

        ((C) c0).m3();      // Bm3-> called     ????
        ((C)c0).m2();        // 没有有效进行强制类型转换    Cm2-> 5

        b0.update();        // x = 99
        b0.m1();            // Am1-> 99
    }
}
```

