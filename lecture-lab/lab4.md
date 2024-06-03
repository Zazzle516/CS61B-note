# Git and Debugging

当时老师的代码是依次下发下来的，那我在做的时候实际代码都已经更新完毕了，是无法完成 lab4 与 lab1 的冲突复现的，那只能自己手动的在 lab1 造成一个冲突，然后完成合并练习



```shell
24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git add .

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git commit -m "fake a conflict and changed lab1"
[master 2595edc] fake a conflict and changed lab1
 1 file changed, 4 insertions(+), 6 deletions(-)

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git push
fatal: unable to access 'https://github.com/Berkeley-CS61B/skeleton-sp21.git/': Recv failure: Connection was reset

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git push
fatal: unable to access 'https://github.com/Berkeley-CS61B/skeleton-sp21.git/': Failed to connect to github.com port 443 after 21084 ms: Couldn't connect to server

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git push
remote: Permission to Berkeley-CS61B/skeleton-sp21.git denied to Zazzle516.
fatal: unable to access 'https://github.com/Berkeley-CS61B/skeleton-sp21.git/': The requested URL returned error: 403

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git config --global --unset http.proxy

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git config --global --unset https.proxy

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git push
remote: Permission to Berkeley-CS61B/skeleton-sp21.git denied to Zazzle516.
fatal: unable to access 'https://github.com/Berkeley-CS61B/skeleton-sp21.git/': The requested URL returned error: 403

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git config --global http.proxy 127.0.0.1:10809

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git config --global https.proxy 127.0.0.1:10809

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git config --global -l
user.email=2405677060@qq.com
user.name=Zazzle
credential.https://gitee.com.provider=generic
http.sslverify=false
http.proxy=127.0.0.1:10809
https.proxy=127.0.0.1:10809

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git commit
On branch master
Your branch is ahead of 'skeleton/master' by 22 commits.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean

24056@Zazzle-Laptop MINGW64 /d/CS61B (master)
$ git push
remote: Permission to Berkeley-CS61B/skeleton-sp21.git denied to Zazzle516.
fatal: unable to access 'https://github.com/Berkeley-CS61B/skeleton-sp21.git/': The requested URL returned error: 403

```

