本文将介绍 git 的进阶及相关设置，从而更优雅地使用 git




#### 分支变基


把一个分支中的修改整合到另一个分支的办法有两种：merge 和 rebase(rebase的翻译为衍合或变基)

分支变基(rebase)其实就是把在一个分支里提交的改变移到另一个分支里重放一遍

当前有两个分支：master 和 x 分支，两个分支的文件 a 内容分别是 'master' 和 'x'
```
$ git log --graph --oneline --all
* db45879 (HEAD -> master) add master to a
| * 64da989 (x) add a to a
|/
* 04b40da add a
```
现在，进行分支变基，将 x 分支提交的改变移动到 master 分支。先切换到 x 分支，然后再 `rebase master`
```
$ git checkout x
$ git rebase master
First, rewinding head to replay your work on top of it...
Applying: add a to a
Using index info to reconstruct a base tree...
M	a
Falling back to patching base and 3-way merge...
Auto-merging a
CONFLICT (content): Merge conflict in a
error: Failed to merge in the changes.
Patch failed at 0001 add a to a
Use 'git am --show-current-patch' to see the failed patch

Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```
解决冲突后，添加到暂存区，然后提交到本地仓库
```
$ git add a
$ git commit -m 'rebase'
```
再次使用 `git log`，结果如下
```
$ git log --graph --oneline --all
* 0e5b749 (HEAD) rebase
* db45879 (master) add master to a
| * 64da989 (x) add a to a
|/
* 04b40da add a
```

### git 标签管理

