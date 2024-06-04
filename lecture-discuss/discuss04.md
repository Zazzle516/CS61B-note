# Inheritance

### JUnit Tests

```java
public class IntListTree {
    
    @Test
    public void testLisst() {
        IntList one = new IntList(1, null);
        IntList TwoOne = new IntList(2, one);
        IntList ThreeTwoOne = new IntList(3, TwoOne);
        
        IntList x = IntList.list(3, 2, 1);
        assertEquals(ThreeTwoOne, x);
    }
    ...
}
```

聊聊 JUnit 测试的好处与坏处

好处肯定是测试用例更直观，更集中，看起来也方便

坏处可能就是有点麻烦吧，总体还是利大于弊



### Creating Cats

> 现在明明应该是 Huh 猫2333（太有灵魂了那个动图

```

```

