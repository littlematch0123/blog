![git_thumb](https://pic.xiaohuochai.site/blog/git/git_thumb.jpg)

版本管理在产品级开发中是非常重要的一个部分，它涉及到团队协作，且影响到产品最终的发布、上线以及测试环节，当前最流行的版本控制系统是 git。git 内容非常多，本文尽量克制地来介绍 git 的相关内容

### 概述

#### 版本控制系统的作用


版本控制系统(Version Control System)是一种记录若干文件修订记录的系统，它有以下三个作用：


1、从当前版本回退到任意版本

![git_base](https://pic.xiaohuochai.site/blog/git/git_base_01.png)

2、查看历史版本

![git_base](https://pic.xiaohuochai.site/blog/git/git_base_02.png)

3、对比两个版本差异

![git_base](https://pic.xiaohuochai.site/blog/git/git_base_03.png)


#### git 优势

1、速度快

2、设计简单

3、轻量级的分支操作，允许上千个并行开发的分支，对非线性开发模式的强力支持

4、有能力高效管理类似 linux 内核一样的超大规模项目

5、git 已经成为事实上的标准，几乎所有优秀的前端项目都通过 git 来进行版本控制

6、社区成熟活跃，git 的流行离不开 github 的贡献

#### 重要概念

要理解 git，首先要了解 git 中的重要概念

【术语介绍】

```
repository 仓库
track 跟踪
modify 修改
stage 暂存
commit 提交
push 推送
pull 拉取
clone 克隆
amend 修改
verbose 冗长的
```
【`.git` 目录】

每个项目都有一个 git 目录(如果 `git clone` 出来的话，就是其中`.git` 的目录)，它是 git 用来保存元数据和对象数据库的地方。这个目录非常重要，每次克隆镜像仓库的时候，实际拷贝的就是这个目录里面的数据

【三种状态】

对于任何一个文件，在 git 中都只有三种状态：已提交(committed)，已修改(modified)和已暂存(staged)
```
已提交：该文件已经被安全地保存在本地数据库中了
已修改：修改了某个文件，但还没有提交保存
已暂存：把已修改的文件放在下次提交时要保存的清单中
```
文化的三种状态正好对应文件流转的三个工作区域：git 的工作目录，暂存区域，以及本地仓库

![git_fileStatus](https://pic.xiaohuochai.site/blog/git_base3.png)

下面来分别解释下，这三个工作区域

工作目录是对项目的某个版本独立提取出来的内容

暂存区域是一个简单的文件，一般都放在 `.git` 目录中。有时候人们会把这个文件叫做索引文件

本地仓库就是指的 `.git` 目录

基本的 git 工作流程如下：

1、在工作目录中修改某些文件

2、对修改后的文件进行快照，然后保存到暂存区域

3、提交更新，将保存在暂存区域的文件快照永久转储到Git目录中

【commit 哈希值】

在保存到 git 之前，所有数据都要进行内容的校验和(checksum)计算，并将此结果作为数据的唯一标识和索引，而不是文件名

git 使用 `SHA-1` 算法计算数据的校验和，通过对文件的内容或目录的结构计算出一个SHA-1哈希值，作为指纹字符串。该字符串由40个十六进制字符(0-9及a-f)组成，看起来就像是：
```
23b9da6552252987aa493b52f8696cd6d3b00372
```

### git 配置

#### 配置级别

　　git 共有三个配置级别
```
　　--local【默认，高优先级】：只影响本仓库，文件为.git/config

　　--global【中优先级】：影响到所有当前用户的git仓库，文件为~/.gitconfig

　　--system【低优先级】：影响到全系统的git仓库，文件为/etc/gitconfig
```
#### 基础配置

一般在新的系统上，需要先配置下自己的 git 工作环境。配置工作只需一次，以后升级时还会沿用现在的配置。当然，如果需要随时可以用相同的命令修改已有的配置

1、用户名
```
git config --global user.name "xiaohuochai"
```
2、邮箱
```
git config --global user.email "121631835@qq.com"
```
3、文本编辑器
```
git config --global core.editor "code --wait"
```
4、更改 git 处理行结束条符的方式

Windows 使用回车（CR）和换行（LF）两个字符来结束一行，而 Mac 和 Linux 只使用换行（LF）一个字符。下面的代码告诉 git 在提交时把回车和换行转换成换行，检出时不转换。这样在 Windows 上的检出文件中会保留回车和换行，而在 Mac 和 Linux 上，以及版本库中会保留换行

```
git config --global core.autocrlf input
```

5、取消对中文的转义

使用 git 时，经常会碰到有一些中文文件名或者路径被转义成\xx\xx\xx的情况，通过下面的配置可以改变默认转义
```
git config --global core.quotepath false
```
#### 查看配置

```
git config --list # 查看所有配置
git config --list --global # 查看全局配置
git config user.name # 查看某个配置项
```
如果要删除或修改配置，更简单的办法是直接打开`~/.gitconfig`文件，或者`.git/config`文件修改即可

#### 关于忽略的配置

一般总会有些文件无需纳入 git 的管理，也不希望它们总出现在未跟踪文件列表

可以在项目根目录创建一个名为 `.gitignore` 的文件，列出要忽略的文件模式

文件 `.gitignore` 的格式规范如下：

1、所有空行或者以注释符号 ＃ 开头的行都会被 git 忽略

2、可以使用标准的glob模式匹配

3、匹配模式以反斜杠(/)开头防止递归

4、匹配模式最后跟反斜杠(/)说明要忽略的是目录

5、要忽略指定模式以外的文件或目录，可以在模式前加上叹号(!)取反

`.gitignore` 文件常见设置如下

```
node_modules/
ecosystem.json
.DS_Store
.idea
.vscode
```
#### SSH 配置


### git 基础操作

#### 初始化新仓库

要对现有的某个项目开始用 git 管理，只需到此项目所在的目录，执行
```
$ git init
```

初始化后，在当前目录下会出现一个名为 `.git` 的目录，所有 git 需要的数据和资源都存放在这个目录中。不过目前，仅仅是按照既有的结构框架初始化好了里边所有的文件和目录，但还没有开始跟踪管理项目中的任何一个文件

#### 检查文件状态

要确定哪些文件当前处于什么状态，可以用 `git status` 命令

如果在取得仓库之后立即执行此命令，会看到类似这样的输出
```
$ git status
On branch master
Initial commit
nothing to commit(create/copy files and use "git add" to track)
```
这说明现在的工作目录相当干净。换句话说，所有已跟踪文件在上次提交后都未被更改过，或者没有任何文件

现在创建一个新文件README，保存退出后运行 `git status` 会看到该文件出现在未跟踪文件列表中

```
$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	README.txt

nothing added to commit but untracked files present (use "git add" to track)
```

在状态报告中可以看到新建的README文件出现在“Untracked files”下面。未跟踪的文件意味着 git 在之前的快照(提交)中没有这些文件

#### 跟踪新文件

使用命令 `git add` 开始跟踪一个新文件。所以，要跟踪README文件，运行

```
$ git add README.txt
```

使用命令 `git add .` 会批量跟踪所有工作目录下未被跟踪的文件
```
$ git add .
```

此时再运行 `git status` 命令，会看到README文件已被跟踪，并处于暂存状态
```
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.txt
```

只要在“Changes to be committed”这行下面的，就说明是已暂存状态

#### 暂存已修改文件

　　现在修改下之前已跟踪过的文件README.txt，将其内容修改为hello world

　　然后再次运行status命令，会看到这样的状态报告：
```
$ echo hello world > README.txt
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   README.txt
```

文件README.txt出现在 “Changes not staged for commit” 这行下面，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。要暂存这次更新，需要运行git add命令

　　git add 命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等

因此，将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适

现在运行 `git add` 将README.txt放到暂存区，然后再看看git status的输出

```
$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   README.txt
```

#### 提交更新

每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了，然后再运行提交命令`git commit`
```
$ git commit
```
这种方式会启动文本编辑器以便输入本次提交的说明，编辑器会显示类似下面的文本信息

```

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#	new file:   README.txt
#
# Changes not staged for commit:
#	modified:   README.txt
#
```

可以看到，默认的提交消息包含最后一次运行 `git status` 的输出，放在注释行里，另外开头还有一空行，需要输入提交说明

另外也可以用 `-m` 参数后跟提交说明的方式，在一行命令中提交更新
```
$ git commit -m '更新 README 内容'
[master 34c5aa0] 更新 README 内容
 1 file changed, 1 insertion(+), 1 deletion(-)
```
提交后它会提示，当前是在哪个分支(master)提交的，本次提交的完整SHA-1校验和是什么(34c5aa0)，以及在本次提交中，有多少文件修订过，多少行添改和删改过

在提交的时候，给 `git commit` 加上 `-a` 选项，git 会自动把所有已经跟踪过的文件暂存起来一并提交

```
$ git commit -am '更新 README'
[master daa40d0] 更新 README
 1 file changed, 1 insertion(+), 1 deletion(-)
```

但是，跳过 `git add` 步骤，不等于完全不使用 `git add`。因为 `git commit -a` 是将所有跟踪过的文件暂存起来并提交，只是省略了暂存这一步。但一个未跟踪状态的文件需要使用 `git add` 命令来使其变成已跟踪状态

还有一种提交方式是使用 `-v` 或`--verbose`选项，翻译成中文是冗余的，它不能能回顾刚刚修改的内容，而且会迫使把提交理由写得更详细些
```
将 README 内容中的 12345 去掉
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Changes to be committed:
#	modified:   README.txt
#
# ------------------------ >8 ------------------------
# Do not modify or remove the line above.
# Everything below it will be ignored.
diff --git a/README.txt b/README.txt
index 5c1d8ad..95d09f2 100644
--- a/README.txt
+++ b/README.txt
@@ -1 +1 @@
-hello world12345
\ No newline at end of file
+hello world
\ No newline at end of file
```
输出结果如下：
```
$ git commit --verbose
[master 2494a62] 将 README 内容中的 12345 去掉
 1 file changed, 1 insertion(+), 1 deletion(-)
```


### git 查看

#### 状态简览

`git status` 命令的输出十分详细，但其用语有些繁琐。 如果使用 `git status -s` 命令或 `git status --short` 命令，将得到一种更为紧凑的格式输出

```
$ git status -s
 M README   # 文件被修改，但还没有放入暂存区
MM Rakefile # 在工作区被修改并提交到暂存区后又在工作区中被修改了
A  lib/git.rb # 新添加到暂存区中的文件
M  lib/simplegit.rb # 文件被修改，且放入了暂存区
?? LICENSE.txt # 新添加的未跟踪的文件
```

#### 状态详览

如果在知道具体哪行发生了改变，要使用 `git diff` 命令

`git diff` 命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容，如果暂存了所有更新过的文件后，则运行 `git diff` 后会什么都没有

下面的代码中，README.txt 文件的内容从 'hello world1' 变化到 'hello world123'

```
$ git diff
diff --git a/README.txt b/README.txt
index 62b372b..6d7f756 100644
--- a/README.txt
+++ b/README.txt
@@ -1 +1 @@
-hello world1
\ No newline at end of file
+hello world123
\ No newline at end of file
```

如果要看已经暂存起来的文件和上次提交时的快照之间的差异，可以用 `git diff--cached` 命令  

下面的代码中，README.txt 文件的内容从空内容变化到 'hello world1'
```
$ git diff --cached
diff --git a/README.txt b/README.txt
new file mode 100644
index 0000000..62b372b
--- /dev/null
+++ b/README.txt
@@ -0,0 +1 @@
+hello world1
\ No newline at end of file
```

#### 查看提交历史

使用 `git log` 命令可以查看提交历史
```
$ git log
commit 3f7b9ed403e6d624651014a5d15c481463572c15 (HEAD -> master)
Author: xiaohuochai <121631835@qq.com>
Date:   Sun Dec 29 23:19:44 2019 +0800

    add b

commit ee5ae6f1dd5f620f4d2ac4a3702eb4814a062fce
Author: xiaohuochai <121631835@qq.com>
Date:   Sun Dec 29 23:15:10 2019 +0800

    delete c
```
默认不用任何参数的话，`git log` 会按提交时间列出所有的更新，最近的更新排在最上面，每次更新都有一个SHA-1校验和、作者的名字和电子邮件地址、提交时间，最后缩进一个段落显示提交说明

我们常用 `-p` 选项展开显示每次提交的内容差异，用 `-2` 则仅显示最近的两次更新
```
$ git log -p -2
commit 3f7b9ed403e6d624651014a5d15c481463572c15 (HEAD -> master)
Author: xiaohuochai <121631835@qq.com>
Date:   Sun Dec 29 23:19:44 2019 +0800

    add b

diff --git a/b1 b/b1
new file mode 100644
index 0000000..e69de29

commit ee5ae6f1dd5f620f4d2ac4a3702eb4814a062fce
Author: xiaohuochai <121631835@qq.com>
Date:   Sun Dec 29 23:15:10 2019 +0800

    delete c

diff --git a/c b/c
deleted file mode 100644
index e69de29..0000000
```

该选项除了显示基本信息之外，还在附带了每次 commit 的变化。当进行代码审查，或者快速浏览某个搭档提交的 commit 的变化时，这个参数就非常有用了

可以用 `--oneline` 选项将每个提交放在一行显示，这在提交数很大时非常有用
```
$ git log --oneline
3f7b9ed (HEAD -> master) add b
ee5ae6f delete c
```

### git 远程仓库

要参与任何一个 git 项目的协作，必须要了解该如何管理远程仓库。远程仓库是指托管在网络上的项目仓库，同他人协作开发某个项目时，需要管理这些远程仓库，以便推送或拉取数据，分享各自的工作进展。管理远程仓库的工作，包括添加远程库，移除远程库，管理远程库分支，定义是否跟踪这些分支等


#### 克隆远程仓库

克隆仓库的命令格式为 `git clone [url]`。比如，要克隆代码仓库 git_learn，可以用下面的命令：

```
$ git clone git@github.com:littlematch0123/git_learn.git
```
这会在当前目录下创建一个名为 `git_learn` 的目录，其中包含一个.git的目录，用于保存下载下来的所有版本记录，然后从中取出最新版本的文件拷贝。如果进入这个新建的 `git_learn` 目录，会看到项目中的所有文件已经在里边了，准备好后续开发和使用。如果希望在克隆的时候，自己定义要新建的项目目录名称，可以在上面的命令末尾指定新的名字

```
$ git clone git@github.com:littlematch0123/git_learn.git learnGit
```

#### 查看远程仓库

要查看当前配置有哪些远程仓库，可以用 `git remote` 命令，它会列出每个远程库的简短名字。在克隆完某个项目后，至少可以看到一个名为 `origin` 的远程库，git 默认使用这个名字来标识所克隆的原始仓库

```
$ git remote
origin
```
也可以加上 -v 选项(v为--verbose的简写，中文意思是冗长的)，显示对应的克隆地址

```
$ git remote -v
origin	git@github.com:littlematch0123/git_learn.git (fetch)
origin	git@github.com:littlematch0123/git_learn.git (push)
```

#### 添加远程仓库

通常情况下，一个本地 git 仓库对应一个远程仓库；然而，在一些情况下，一个本地仓库需要同时关联多个远程仓库，比如同时将一个项目发布在 github 和 coding上

添加一个新的远程仓库，可以指定一个名字，以便将来引用，运行 `git remote add [shortname] [url]`
```
$ git remote add coding git@git.coding.net:ehuo0123/git_learn.git
$ git remote -v
coding	git@git.coding.net:ehuo0123/git_learn.git (fetch)
coding	git@git.coding.net:ehuo0123/git_learn.git (push)
origin	git@github.com:littlematch0123/git_learn.git (fetch)
origin	git@github.com:littlematch0123/git_learn.git (push)
```

#### 抓取和推送数据

现在可以用字符串 coding 指代对应的仓库地址了。比如说，要抓取所有 coding 有的，但本地仓库没有的信息，可以运行 `git fetch coding`

从远程仓库中获得数据，可以执行：
```
$ git fetch [remote-name]
```


使用命令 `git push [remote-name] [branch-name]`，来推送数据到远程仓库

```
$ git push [remote-name] [branch-name]
```

如果要把本地的 master 分支推送到 origin 服务器上(克隆操作会自动使用默认的master和origin名字)，可以运行下面的命令

```
$ git push origin master
```

同理，pull操作也需要指定从哪个远程仓库拉取，`git pull [remote-name] [branch-name]`


#### 远程分支删除和重命名

```
$ git remote rename coding cd # 重命名
```

```
$ git remote rm coding # 删除
```

#### 不区分远程仓库

由于添加了多个远程仓库，在 push 和 pull 时便面临了仓库的选择问题。诚然如此较为严谨，但是在许多情况下，只需要保持远程仓库完全一致，而不需要进行区分，因而这样的区分便显得有些“多余”

先查看当前的 `git remote` 情况

```
$ git remote -v
origin	git@github.com:littlematch0123/git_learn.git (fetch)
origin	git@github.com:littlematch0123/git_learn.git (push)
```

接下来，不额外添加远程仓库，而是给现有的远程仓库添加额外的URL

使用 `git remote set-url --add <name> <url>`，给已有的远程仓库添加一个远程地址

```
$ git remote set-url --add origin git@git.coding.net:ehuo0123/git_learn.git
```

再次查看所关联的远程仓库：

```
$ git remote -v
origin	git@github.com:littlematch0123/git_learn.git (fetch)
origin	git@github.com:littlematch0123/git_learn.git (push)
origin	git@git.coding.net:ehuo0123/git_learn.git (push)
```

这样设置后的push 和pull操作与最初的操作完全一致，不需要进行调整

如果不再需要多个仓库，可以使用`git remote set-url --delete <name> <url>`，将其删除
```
$ git remote set-url --delete origin git@git.coding.net:ehuo0123/git_learn.git
```

### git 分支管理

### git 标签管理

### git 其他操作

#### 删除文件

1、从工作目录中删除文件，直接使用 `rm` 命令删除即可，因为其没有纳入 git 版本库中，git 并不知道

```
touch a # 新建 a
rm a # 删除 a
```
如果画蛇添足地使用 `git rm a`，反而会提示错误
```
$ git rm a
fatal: pathspec 'a' did not match any files
```

2、从暂存区中删除文件，需要使用 `git rm -f` 命令来强制删除
```
touch b # 新建 b
git add b # 将 b 添加到暂存区
git rm -f b # 删除 b
```
如果使用 `git rm b`，会提示如下错误
```
$ git rm b
error: the following file has changes staged in the index:
    b
(use --cached to keep the file, or -f to force removal)
```

3、从本地仓库中删除文件，使用`git rm`命令即可
```
touch c # 新建 c
git add c # 将 c 添加到暂存区
git commit -m 'add c' # 提交到本地仓库
git rm c # 删除 c
```
4、如果仅仅是想把文件从 git 仓库中删除(亦即从暂存区域移除)，但仍然希望保留在当前工作目录中。换句话说，仅是从跟踪清单中删除。比如一些文件不小心纳入仓库后，要移除跟踪但不删除文件，以便稍后在 `.gitignore` 文件中补上，用--cached选项即可
```
$ git rm d --cached
```

#### 文件重命名

1、从工作目录中文件重命名，直接使用 `mv` 命令删除即可，因为其没有纳入 git 版本库中，git 并不知道

```
touch a # 新建 a
mv a a1 # 重命名 a 为 a1
```
如果画蛇添足地使用 `git mv a a1`，反而会提示错误
```
$ git mv a a1
fatal: not under version control, source=a, destination=a1
```

2、从暂存区，或者本地仓库中重命名文件，直接使用 `git mv` 命令就可以了
```
$ git mv b1 b2
localhost:t bailiang$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	renamed:    b1 -> b2
```

#### 撤消操作

任何时候，都有可能需要撤消刚才所做的某些操作。但要注意的是，有些撤销操作是不可逆的，所以要谨慎小心，一旦失误，就有可能丢失部分工作成果

1、修改最后一次提交

有时候提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 `--amend` 选项重新提交：

```
$ git commit --amend
```
如果刚才提交时忘了暂存某些修改，可以先补上暂存操作，然后再运行 `--amend` 提交

```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```
上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容

2、取消已暂存的文件
使用 `git reset HEAD <file>...` 命令可以取消暂存，将暂存区的文件恢复到工作目录中
```
$ git reset HEAD a.txt
```

3、取消对文件的修改

使用 `git checkout -- <file>...`  命令可以将文件恢复到上一个版本的状态。要注意的是这个命令非常危险，对文件做的任何修改都会消失，因为只是拷贝了另一个文件来覆盖它。除非确实不想要那个文件了，否则不要使用这个命令
```
$ git checkout -- a.txt
```

### git 常用命令

### 注意事项

1、版本控制系统只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等。图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从100KB改成了120KB，但到底改了啥，版本控制系统不知道，也没法知道

　　Microsoft 的 Word 格式是二进制格式，因此，版本控制系统是没法跟踪 Word 

    当然，办法也是有的，需要安装 `docx2txt` 程序，将 word 文档转换为可读的文本文件

    把下面这行文本加到 .gitattributes 文件中：

```
*.docx diff=word
```

写一个脚本把输出结果包装成 git 支持的格式。 在可执行路径下创建一个叫 docx2txt 文件，添加这些内容：

```
#!/bin/bash
docx2txt.pl $1 -
```
用 `chmod a+x` 给这个文件加上可执行权限。 最后，需要配置 git 来使用这个脚本

```
$ git config diff.word.textconv docx2txt
```
现在如果在两个快照之间进行比较，git 就会对那些以 .docx 结尾的文件应用“word”过滤器，即 docx2txt。这样，Word 文件就能被高效地转换成文本文件并进行比较了

2、不要使用 Windows 自带的记事本编辑任何文本文件。原因是 Microsoft 开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf(十六进制)的字符，会遇到很多不可思议的问题

3、`git commit -am`可以写成`git commit -a -m`，但不能写成`git commit -m -a`

4、在 git 中任何已提交的东西几乎总是可以恢复的，但任何未提交的东西丢失后很可能再也找不到了



