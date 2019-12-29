![git_thumb](https://pic.xiaohuochai.site/blog/git/git_thumb.jpg)

版本管理在产品级开发中是非常重要的一个部分，它涉及到团队协作，且影响到产品最终的发布、上线以及测试环节，当前最流行的版本控制系统是 git。git 内容非常多，本文尽量克制地来介绍 git 的相关内容

### 概述

#### 版本控制系统的作用

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

【`.git` 目录】

每个项目都有一个 git 目录(如果 `git clone` 出来的话，就是其中`.git` 的目录)，它是 git 用来保存元数据和对象数据库的地方。这个目录非常重要，每次克隆镜像仓库的时候，实际拷贝的就是这个目录里面的数据

【三种状态】

对于任何一个文件，在 git 中都只有三种状态：已提交(committed)，已修改(modified)和已暂存(staged)
```
已提交：该文件已经被安全地保存在本地数据库中了
已修改：修改了某个文件，但还没有提交保存
已暂存：把已修改的文件放在下次提交时要保存的清单中
```
文化的三种状态正好对应文件流转的三个工作区域：git 的工作目录，暂存区域，以及本地仓库

![git_fileStatus](https://pic.xiaohuochai.site/blog/git_base3.png)

下面来分别解释下，这三个工作区域

工作目录是对项目的某个版本独立提取出来的内容

暂存区域是一个简单的文件，一般都放在 `.git` 目录中。有时候人们会把这个文件叫做索引文件

本地仓库就是指的 `.git` 目录

基本的 git 工作流程如下：

1、在工作目录中修改某些文件

2、对修改后的文件进行快照，然后保存到暂存区域

3、提交更新，将保存在暂存区域的文件快照永久转储到Git目录中

【commit 哈希值】

在保存到 git 之前，所有数据都要进行内容的校验和(checksum)计算，并将此结果作为数据的唯一标识和索引，而不是文件名

git 使用 `SHA-1` 算法计算数据的校验和，通过对文件的内容或目录的结构计算出一个SHA-1哈希值，作为指纹字符串。该字符串由40个十六进制字符(0-9及a-f)组成，看起来就像是：
```
23b9da6552252987aa493b52f8696cd6d3b00372
```

### git 配置

【配置级别】

　　git 共有三个配置级别
```
　　--local【默认，高优先级】：只影响本仓库，文件为.git/config

　　--global【中优先级】：影响到所有当前用户的git仓库，文件为~/.gitconfig

　　--system【低优先级】：影响到全系统的git仓库，文件为/etc/gitconfig
```
【基础配置】

一般在新的系统上，需要先配置下自己的 git 工作环境。配置工作只需一次，以后升级时还会沿用现在的配置。当然，如果需要随时可以用相同的命令修改已有的配置

1、用户名
```
git config --global user.name "xiaohuochai"
```
2、邮箱
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
【查看配置】

```
git config --list # 查看所有配置
git config --list --global # 查看全局配置
git config user.name # 查看某个配置项
```
如果要删除或修改配置，更简单的办法是直接打开`~/.gitconfig`文件，或者`.git/config`文件修改即可

