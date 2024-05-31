## A: Intro to Version Control System

Git 是一个分布式版本控制系统，每个开发者的电脑上都保存了整个项目全部的历史记录（包括旧版本）

#### Repository

整个项目的整个历史被称为一个 Repository，因为是本地存储的，所以可以在本地使用 Git，甚至不需要联网

（所以 GitHub 和 Git 其实没关系，只是一个服务器，所有在线仓库的一个用户罢了



## B: Local Repositories

注意 **Github != Git** （使用 Git 本身并不会用到 `git push` 命令



首先是先创建一个文件夹，然后把该文件夹作为本地仓库，进入该文件夹内部，创建一些文件

```shell
cd repo_file
touch Asuka.txt
touch ShinJi.txt
```



总之在创建一些文件后，声明该文件夹作为 Git 仓库

```shell
git init
```



创建本地仓库，但是此时还没有在 Git 仓库中添加任何文件

```shell
git add Asuka.txt	# 实际上是把 Asuka.txt 文件添加到 <track file> 中
```

单独添加一个文件其实是有的时候不需要跟踪所有文件，通过 `add command` 命令可以告诉 Git 它该追踪哪些文件



此时检查 Git 的状态

```shell
git status		# 能看到 new file: Asuka.txt 等待 commit(同时还会显示未追踪的文件 ShiJi.txt)
```



上传文件（把文件放入保险箱中

```shell
git commit -m "Eva"
```

只要不是世界毁灭，行星爆炸（，硬盘损坏），就可以认为是完全安全的



再次查看状态

```shell
git status	# 此时会显示 clean tree
git log		# 此时会显示出曾经的提交记录	可以看到上一次的 commit 号和时间
```



更激进一点的，可以查看内容，这行命令没什么实际意义，反正也看不懂，只是看看 Git 的快照功能

```shell
git show	# 不是人类能看懂的语言
```



修改已经放入保险箱的文件

```shell
vim Asuka.txt

# 尝试提交修改
git commit -m "changed Asuka.txt"		# 事实上 Git 无法进行提交
# Changes not staged for commit
```

为什么会无法提交呢，因为虽然这个文件之前通过 `git add` 添加到了追踪列表中，但是并没有改变它的状态，导致 Git 不知道这个文件被修改了

```shell
git add Asuka.txt
git commit -m "changed Asuka.txt"	# sucess!
git log		# 此时可以发现有 2 个提交了
```



在修改文件后想回到上一次提交再修改

```shell
git log		# 通过查看提交的版本号 找到目标提交号 commit_num
git checkout commit_num ./Asuka.txt

... file changing...

# 二次提交	提交记录并不会覆盖
git commit -m "Reverse Change"		# 并不需要使用 git add 因为 checkout 自动进行了一次 add

# 查看第三次的提交
git log
```



## C: Local Repositories

[Git - Recording Changes to the Repository (git-scm.com)](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) 

















# 附录

## Staged

[Git三大特色之Stage(暂存区)_git stage-CSDN博客](https://blog.csdn.net/qq_32452623/article/details/78417609) 

理论上，即使你在提交的时候，不去进行 `git add` 处理，也可以把修改提交上去，但是 Git 无法检测到你的修改

也就是，`git commit` 检测的文件和实际存储的文件并不是一个文件，`git commit` 检测的文件存储在 **暂存区** 



### Git Section

Git 的本地数据管理可以分为 3 个区：工作区，暂存区，版本库

#### Working Directory

打开文件的地方，文本啊，VS 啊之类的



#### Stage

数据暂时存放的地方，可以在工作区和版本库之间进行数据交流



#### Commit History

存放已经提交的数据，`git push` 的执行内容



#### Work Flow

第一次 `git add` 操作同步 Working_Directory 和 Stage 的内容，第二次 `git commit` 操作同步 Commit History

通过一个暂存区给挽回错误操作带来了可能性

```
1. developer
2. Working_Directory -> add -> Stage -> commit -> Commit_History
3. RemoteRepository
```



## Recording Change

对 C 部分的 Git 官方链接进行一些说明，关于 Git 如何判断文件改变 [Git - Recording Changes to the Repository (git-scm.com)](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) 

在工作环境（repo）中每个文件只有两个状态，`tracked` 或者 `untracked` 状态

- Tracked File：在上一个快照中的文件，还包括新添加的文件，可能是未修改的，修改的，暂存的
- Untracked File：不在上一个快照并且不在暂存区的文件（可以是曾经的快照中未修改的文件）

当你拷贝了一个仓库了下来，此时仓库中的所有文件都属于 `Tracked Files` 并且是未修改状态，因为 Git 刚刚检查过，并且你也没有修改任何文件

```shell
# 查看 master 分支下正在跟踪的文件
git ls-tree -r master --name-only
```



#### Tracked Files

只要曾经对该文件使用过 `git add` 该文件**永远也回不到** `Untracked` 状态

注意图中的 `remove` 操作只能从 `Unmodified` 状态开始

<img src="./../textbook-note/pics/git_status.svg" style="zoom:80%;" />

```shell
mkdir GitTest
cd GitTest
git init
touch a.txt
touch b.txt

git ls-tree -r master --name-only	# 无结果 此时还没有任何文件添加进 Git
git add .
git commit -m "First"

git ls-tree -r master --name-only	# 此时查看可以看到  a.txt  b.txt

echo "Hello">>a.txt		# 命令不一定对 反正就是修改 a.txt

git add a.txt
git commit -m "Test A file"
git ls-tree -r master --name-only	# 此时仍然可以看到  a.txt  b.txt

# 再次修改 a.txt
echo "World">>a.txt
git add a.txt
git commit -m "Second A file change"

git ls-tree -r master --name-only	# b.txt 是一直存在的
```



### Staging Modified Files

`git add` 除了声明文件追踪还有很多其他的功能，还可以把文件放到缓存区，标记合并（解决文件冲突），其实更准确的描述是 `add precisely this content to the next commit` 

如果在执行 `git add` 后又修改了文件，必须重新执行 `git add` 因为新修改的内容没有放入缓冲区，无法识别



### Short Status

```shell
git status -s	# git status 的简化版本
```

文件状态标识：

- Untracked File：有个 `??` 在左边
- new added File：有个 `A` 在左边     （如此类比其他标识



### Ignoring Files

便于项目管理而产生的 `.gitignore` 文件，一般自己从零开始创建 Git 仓库的话没有，需要自己新建，但是 IDEA 之类的集成环境会自动帮你配置好（一般是日志文件，临时文件，工具生成文件之类的）

> 不过注意：`gitignore` 文件只能适用于还没有被加入追踪的文件，一旦已经追踪了，即使添加到 `gitignore` 中也没用了（这点我有深刻教训

```shell
*.[oa]	# 告诉 Git 忽视以 .o 和 .a 结尾的文件
*~		# 忽视所有以 ~ 结尾的文件		比如 Word 用来标识缓存文件
```

关于 `gitignore` 文件书写的一些规则：

- 注释通过 `#` 标识
- `gitignore` 文件的每个配置项单独占一行，最简单的方式就是直接把文件名写进去
- 稍微复杂一点到正则表达式匹配
  - 如果 `gitignore` 文件放在项目根目录（绝大多数情况），则开头的 `/` 表示只能匹配根目录的，如果开头没有 `/` 则会在任何目录开始匹配
  - 如果 `/` 放在末尾，则表示只匹配目录
  - 使用 `!` 表示该文件会被重新包含（但同时如果它有上级目录的话，上级目录也需要是被包含的状态
  - `**` 用于匹配多级目录，比如 `a/**/b` 可以表示 `a/b` 或者 `a/x/../b` 这样



#### Ignore Case

- 忽略所有内容

  ```shell
  *
  ```

- 忽略所有目录

  ```shell
  */		# 注意表示目录的 / 是在末尾
  ```

- 忽略某路径下的所有文件，除了某个特定文件

  ```shell
  file_Directory/*		# 声明并不是文件夹本身
  !file_Directory/special_file_name
  ```

- 只保留特定目录下某个特定名称的所有文件

  ```shell
  /*						# 忽略所有目录
  !/file_Directory/		# 保留根目录下的特定目录	(根目录本身的信息一定会被保留
  
  /file_Directory/*
  !/file_Directory/a?z.*	# 保留 a 开头 z 结尾的任意后缀文件
  ```

  

#### Ignore Check

如何判断自己写的 `ignore` 文件是否正确生效

```shell
git check-ignore -v {文件或目录路径}

# -v 查看该文件是通过哪条文件忽略掉的
# 如果有相应的输出 说明被忽略了
```



#### Add then Ignore

遇到了最不幸的情况，先加入了版本管理但是现在想忽略

```shell
fix .gitignore file

git rm -r --cached .	# 清除本地缓存
git add .				# 重置跟踪文件
```



#### Ignore then Add

相对不幸的情况，先前忽略了现在想加入（但是解决办法是相同的

```shell
fix .gitignore file

git rm -r --cached .	# 清除本地缓存
git add .				# 重置跟踪文件
```

