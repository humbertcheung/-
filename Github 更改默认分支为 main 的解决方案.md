# Github 默认分支为 main 的解决方案

Github默认的主分支从2020年10月1日起已经由master改为了main(在我们创建仓库时，如果勾选添加 README file 文件时就会采用main为默认分支)，然而Git工具默认推送的还是master分支，这就很蛋疼的导致了推送的代码在Github上面的 main 主分支看不到，想要看到代码还需要切换到 master 分支 才可以。

<img src="https://s2.loli.net/2022/07/31/JGTtE6Kwn9pBlZO.png" alt="image-20220731174941989" style="zoom:60%;" />

## 解决办法1

在创建好仓库后，可以进入仓库的设置页面中，将仓库的默认分支设置回 master：

<img src="https://s2.loli.net/2022/07/31/wJXsHiEQPxRoG3V.png" alt="image-20220731214645792" style="zoom: 50%;" />

## 解决办法2

在初始化本地仓库：

`git init`

和关联远程仓库后：

`git remote add origin git@github.com:humbertcheung/login-demo.git`

### （1）拉取 main 分支的内容

通过 rebase 参数来拉取 main 分支上的内容：

`git pull --rebase origin main`

### （2）将内容添加到本地仓库

通过 add 将内容写入暂存区：

`git add .`

通过 commit 将内容提交到本地仓库中：

`git commit -m "init"`

### （3）创建 main 分支切换到main 分支

我们通过 git checkout 命令后面跟一个 `-b` 参数，这样就不用使用 git branch命令新建分支了。可以自动创建一个新分支 main 并立即切换到这个分支

`git checkout -b main`

### （4）将本地仓库的内容提交到远程仓库

通过 push 命令进行提交：

`git push -u origin main`

如果当前分支与多个主机存在追踪关系，则可以使用 -u 选项指定一个默认主机，这样后面就可以不加任何参数使用git push。 上面命令将本地的 main 分支推送到 origin 主机，同时指定 origin 为默认主机，后面就可以不加任何参数使用git push了。

### （5）删除 master 分支

最后，我们删除 master 分支即可，后续我们就直接使用 main 为主分支了

`git branch -D master`

**附上完整的操作日志：**

```ASN.1
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git init
Initialized empty Git repository in /Users/Humbert/Documents/login-demo/.git/
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git remote add origin git@github.com:humbertcheung/login-demo.git
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git pull --rebase origin main
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (4/4), done.
Unpacking objects: 100% (4/4), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
From github.com:humbertcheung/login-demo
 * branch            main       -> FETCH_HEAD
 * [new branch]      main       -> origin/main
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git add .
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git commit -m "init"
[master ff31735] init
 622 files changed, 98295 insertions(+)
 create mode 100644 app.js
 create mode 100644 contacts.sql
 create mode 120000 node_modules/.bin/mime
 create mode 100644 node_modules/.package-lock.json
 create mode 100644 node_modules/accepts/HISTORY.md
 create mode 100644 node_modules/accepts/LICENSE
 ......(文件过多，此处省略)
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git checkout -b main
Switched to a new branch 'main'
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git push -u origin main
Enumerating objects: 739, done.
Counting objects: 100% (739/739), done.
Delta compression using up to 16 threads
Compressing objects: 100% (690/690), done.
Writing objects: 100% (738/738), 2.75 MiB | 835.00 KiB/s, done.
Total 738 (delta 103), reused 0 (delta 0)
remote: Resolving deltas: 100% (103/103), done.
To github.com:humbertcheung/login-demo.git
   7ebe440..ff31735  main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git branch -D master
Deleted branch master (was ff31735).
(base) Humbert@HumbertCheungs-Pro-16 login-demo % git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

