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
