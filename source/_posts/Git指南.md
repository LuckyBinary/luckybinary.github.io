---
title: Git指南
date: 2018-05-19 15:05:03
tags: ['Git']
---

## Git 的工作流程

Git 的工作流程一般如下：

1. 在工作目录中添加、修改文件
2. 将需要进行版本管理的文件放入暂存区域
3. 将暂存区域的文件提交到 Git 仓库

Git 管理的文件有三种状态：

- 已修改（modified）
- 已暂存（staged）
- 已提交（committed）

## 实践

### 将工作目录的文件放到 Git 仓库

- git init
- git add 文件名
- git commit -m "提交的注释"

### 查看状态

`git status`
当工作目录中文件有增加修改等新状态时，`git status`有红色提示。

![查看Git状态](/img/notes/2018-5-19_1_2.png)

`git reset HEAD "文件名"` 将最近一次提交到暂存区的文件移除

![移除暂存区](/img/notes/2018-5-19_1_3.png)

重新提交**LICENSE**到暂存区并提交到 Git 仓库

![加入暂存区并提交](/img/notes/2018-5-19_1_4.png)

修改**LICENSE**文件中的内容后，用`git status`查看状态

![修改文件后的状态](/img/notes/2018-5-19_1_5.png)
此时的状态为上述提到的*modified*状态

该状态下有两种选项
撤销修改：
`git checkout -- <filename>`该选项有两种情况：

1、LICENSE 自修改后还没有被放到暂存区，现在，撤销修改就是将上次存到 Git 仓库的文件*还原*到当前工作目录文件；

2、LICENSE 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。

保存修改：
`git add <filename>`该选项将本次修改后的文件*更新*到暂存区，覆盖上次提交的内容

用撤销修改命令还原上次储存到仓库中的内容（此时没有放入暂存区）后，并重新修改 LICENSE 文件，并用第二条命令添加到暂存区，并提交到仓库

![修改文件后的选项](/img/notes/2018-5-19_1_6.png)

### 查看历史提交

`git log`可以加上`--pretty=oneline`参数`git log --pretty=oneline`

![提交记录](/img/notes/2018-5-19_1_7.png)

通过上述操作，一共对 Git 仓库进行了三次提交。

### 版本回退

版本回退分三种情况：
1、当你修改了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- <file>`，上述所讲的撤销修改。

2、当你修改工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了情况 1，第二步按情况 1 操作。

3、已经提交了不合适的修改到 Git 仓库时，想要撤销本次提交，前提是没有推送到远程库时，用`git reset --hard HEAD^`命令。Git 必须知道当前版本是哪个版本，在 Git 中，用 HEAD 表示当前版本，也就是最新的提交提交 ID，上一个版本就是 HEAD^，上上一个版本就是 HEAD^^，当然往上 100 个版本写 100 个^比较容易数不过来，所以写成 HEAD~100。

当前工作目录、暂存区、Git 仓库状态如下：

![三个区域状态](/img/notes/2018-5-19_1_8.png)

当使用命令`git reset HEAD~`后：

![三个区域回退版本后状态](/img/notes/2018-5-19_1_9.png)

此时使用`git status`命令查看：

![三个区域回退版本后状态](/img/notes/2018-5-19_1_10.png)

### reset命令的选项

`git reset --mixed HEAD~`（默认）
1、移动HEAD的指向，将其指向上一个版本
2、将HEAD移动后指向的版本回滚到暂存区域

`git reset --soft HEAD~`
1、移动HEAD的指向，将其指向上一个版本
  
`git reset --hard HEAD~`
1、移动HEAD的指向，将其指向上一个版本
2、将HEAD移动后指向的版本回滚到暂存区域
3、将暂存区域的文件还原到工作目录

### 版本前滚

`git reset --<> 版本ID号` 版本号没必要写全，前几位就可以了。

#### 版本号获取

1. 命令行窗口还没有被关掉，可以顺着往上找，就可以指定回到未来的某个版本。
2. Git提供了一个命令`git reflog`用来记录你的每一次命令：
  ![命令记录](/img/notes/2018-5-19_1_11.png)

## 版本对比

### 比较暂存区与工作区

`git diff`

### 比较两个历史快照

`git diff 版本ID1 版本ID2`

### 比较当前工作区和仓库版本

`git diff 版本ID`

### 比较暂存区和仓库版本

`git diff --cached 版本ID`

![版本对比总结](/img/notes/2018-5-19_1_12.png)
  
## 修改最后一次提交

实际开发过程中，可能遇到两种情况：
1、版本刚提交（commit）到仓库，突然想起漏掉两个文件没有添加（add）
2、版本提交（commit）到仓库后，发现版本说明写的不全面。
  
解决方法：`git commit --amend` Git会更正”最近“一次的提交。
第一种情况先将漏掉的文件添加到暂存区，然后再执行命令：
![修改最后提交](/img/notes/2018-5-19_1_13.png)

进入后按i进入INSERT，按ESC退出INSERT。`SHIFT+ZZ`保存退出，`:q!`不保存退出

## 删除文件和文件误删

### 文件误删

![恢复误删文件](/img/notes/2018-5-19_1_14.png)

### 删除文件

`git rm 文件名`
该命令删除的只是工作目录和暂存区域的文件，也就是取消跟踪，在下次提交时不纳入版本管理。
若此时工作目录与暂存区域文件不同，工作区文件还未重新添加（add）：`git rm -f <filename>`可以同时删除两个文件。
如果只删除暂存区域的文件（保留工作目录）：`git rm --cached <filename>`可以删除暂存区文件

## 重命名文件

`git mv 旧文件名 新文件名`

## 创建和切换分支

### 创建分支

`git branch 分支名`

### 切换分支

`git checkout 分支名`

> 创建并切换到分支：`git checkout -b 分支名`

### 分支操作

然后，用`git branch`命令查看当前分支：
`git branch`命令会列出所有分支，当前分支前面会标一个\*号。

#### 合并分支

1、“Fast-forward”快进模式
新分支提交后主分支无新提交。
合并某分支到当前分支：`git merge 分支名`
2、冲突合并
master 主分支和新分支各自都分别有新的提交，这种情况下，`git merge 分支名`无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，必须手动在冲突文件中解决冲突后再提交。
`git log --graph`命令可以看到分支合并图。
合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而 fast forward 合并就看不出来曾经做过合并。

#### 删除分支

`git branch -d 分支名`
如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <分支名`强行删除。

#### Bug 分支

修复 bug 时，我们会通过创建新的 bug 分支进行修复，然后合并，最后删除；
当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复 bug，修复后，再`git stash pop`，回到工作现场。

#### 多人合作

`git push origin 分支名`推送分支到远程仓库

## 远程仓库

### 添加远程库

登陆 GitHub，然后，在右上角找到“Create a new repo”按钮创建一个新的仓库，在 Repository name 填入仓库名，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的 Git 仓库。
要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；
关联后，使用命令`git push -u origin master`第一次推送 master 分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改。如果推送失败,先用`git pull`抓取远程的新提交。

### 从远程库克隆

假设从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。首先，登陆 GitHub，创建一个新的仓库
`git clone git@github.com:WangCheng96/webStudy.git`

### 小结

1、查看远程库信息，使用`git remote -v`。
2、在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致。
3、建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`。
4、从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

## 标签管理

### 创建标签

`git tag <tagname>`用于新建一个标签，默认为 HEAD，也可以指定一个 commit id；
`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
`git tag`可以查看所有标签。

### 操作标签

`git push origin <tagname>`可以推送一个本地标签；
`git push origin --tags`可以推送全部未推送过的本地标签；
`git tag -d <tagname>`可以删除一个本地标签；
`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

## 用 Git Subtree 在多个 Git 项目间双向同步子项目

### 什么时候需要 Subtree

- 当多个项目共用同一坨代码，而这坨代码跟着项目在快速更新的时候
- 把一部分代码迁移出去独立为一个新的 git 仓库，但又希望能够保留这部分代码的历史提交记录。
