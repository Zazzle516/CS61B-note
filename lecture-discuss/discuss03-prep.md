# Arrays, Linked Lists

### Fill Grid

给出两个一维数组，分别对应左下角和右上角元素，不修改主对角线的元素，两个数组可以提供足够数量的元素，如果超出了规定数量，忽视即可

```java
public class Grid {

    public static void fillGrid(int[] LL, int[] UR, int[][] S) {
        int N = S.length;
        int kL, kR;
        kL = kR = 0;

        // 提供的代码框架要求在一个循环里解决
        // LL 数组和 UR 数组的目标元素下标相反
        for (int i = 0; i < N; i++) {
            for (int j = i + 1; j < N; j++) {
                S[i][j] = UR[kR];
                S[j][i] = LL[kL];
                kR += 1;
                kL += 1;
            }
        }
    }

    public static void main(String[] args) {
        int[] LL = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 0, 0};
        int[] UR = { 11, 12, 13, 14, 15, 16, 17, 18, 19, 20 };
        int[][] S = {
                { 0, 0, 0, 0, 0},
                { 0, 0, 0, 0, 0},
                { 0, 0, 0, 0, 0},
                { 0, 0, 0, 0, 0},
                { 0, 0, 0, 0, 0}
        };

        fillGrid(LL, UR, S);
    }
}
```



### Even Odd

直接修改给定的链表的顺序让偶数奇数交替，即使是偶数索引链接也要在奇数索引链接之前

```java
IntList.list(0, 3, 1, 4, 2, 5);
evenOdd(lst);
IntList.list(0, 1, 2, 3, 4, 5);		// 注意无论是奇数长度或者偶数长度都需要正确运行
```

我对题目的理解应该就是说，永远从偶数开始，完成递增的排序？（我没看太懂

我看懂了，是根据元素内容进行元素的插入（感觉也不太对吧... 这样不能破坏性的修改

我看了答案也看不懂他想干什么... 什么玩意





### Partition

对给定的数列 `lst` 和整数 `k` 破坏性的分组，拆分 `lst` 到 `k` 个子列，每个子列要拥有以下属性

- 每个子列的长度应该相同，如果做不到的化，后一个子列的长度要比当前子列长度 - 1
- 元素的顺序保持不变

分组的策略不统一，可能有很多种分组方式，选择一种即可，要求返回一个一维数组

我不知道他想让我干什么....	罢了罢了，这个 Disc 也不是很重要



# Done...?

全程只看懂了一道题，无语，答案也看不懂他想干什么



















