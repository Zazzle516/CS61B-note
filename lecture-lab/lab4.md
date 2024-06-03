# Git and Debugging

### Intro to Git

看完 SPEC 提供的一系列视频后，至少能回答下面的问题

- git 工作流 `git add` 和 `git commit` 
- 在不同的提交之间切换并且更新文件 `git checkout` 
- 分离 `Head` 状态
- 远程仓库
- 本地仓库与远程仓库的合并交互 `origin/master` 和 `skeleton/master` 
- 如何解决合并冲突

[Using Git | CS 61B Spring 2019 (datastructur.es)](https://sp19.datastructur.es/materials/guides/using-git) 



### Git Exercise

当时老师的代码是依次下发下来的，那我在做的时候实际代码都已经更新完毕了，是无法完成 lab4 与 lab1 的冲突复现的，那只能自己手动的在 lab1 造成一个冲突，然后完成合并练习

还是比较简单的，这里贴一个手动完成的方式	[CS 61B 攻略 | 朴素的博客 (mordecaimaic.github.io)](https://mordecaimaic.github.io/posts/cs61b-Labs-and-Projects-Challenges-Journal/) 



#### 补救措施

```shell
# 即使全完了 也可以用这行代码回到最初始的状态 重新开始
git checkout skeleton/master -- <file>

# 回到上一次提交的状态
git checkout master -- proj0/game2048/Model.java
```



### A Debugging Mystery

简单的使用 `sout` 就能发现问题，是 Java 的 Integer 的范围缓存并没有更新

Java 中 Integer 取值范围本身与 int 类型一致，但是为了提高效率，Java 缓存了 `[-128 ~ 127]` 的整数对象

改就很简单了，我懒得修改 IDEA 缓存池刷新，直接改 `int` 完事了	

[Java--Integer的常量缓存池（默认-128~127数值范围）_integer缓存范围-CSDN博客](https://blog.csdn.net/MinggeQingchun/article/details/120704300) 

