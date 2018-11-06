## 安装
sudo apt-get install git

git config --global user.name "Your Name"

git config --global user.email "email@example.com"


## 基本操作
- `git init`

    初始化一个本地空Git仓库；

- `git add {file}|{dir}`

    添加文件到Git仓库，可反复多次使用，添加多个文件；

- `git rm`

    在git中删除一个文件，但你会丢失最近一次提交后你修改的内容，因此多`commit`。普通的`rm`命令不会影响到版本库的文件。

- `git commit -m {message}`

- `git status`

- `git diff <filename>`

    也可以不加文件名，显示所有diff;

    - `git diff HEAD -- {filename}`

        显示与版本库的不同，HEAD可换做其他;

- `git log`

    查看提交历史，以便确定要回退到哪个版本。可以使用自定义的git lg，更好看;

- `git reflog`

    重返未来，查看命令历史，以便确定要回到未来的哪个版本。


## 版本回退
- `git reset --hard HEAD~{number}|{commit_id} {filename}`

    在版本的历史之间穿梭。**HEAD** 指向当前版本，**HEAD^**,**HEAD^^**,**HEAD^^^** 可以指向之前的版本。不添加 **{filename}** 时，指定所有文件。
    - `--mixed`（默认）更改**HEAD**、**INDEX**
    - `--soft`只更改**HEAD**
    - `--hard`更改**HEAD**、**INDEX** 、**WORKING COPY** 所有


## 撤销修改
- 场景1：`git checkout -- {filename}`

    改乱了工作区某个文件的内容，想直接丢弃工作区的修改；

- 场景2：`git reset HEAD {filename}`

    当你不但改乱了工作区某个文件的内容，还`git add`添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD {filename}`，就回到了场景1，第二步按场景1操作；

- 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考 **[版本回退](#版本回退)** ，不过前提是没有推送到远程库。


## 远程库
- `git remote add origin git@server:path/repo.git`

    关联本地文件夹到一个远程库 ；

- `git push -u origin master`

    **第一次** 推送master分支的所有内容。此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；
    - `-u`指令指定默认主机，以后久可以使用`git push`推送；

- `git clone git@server:path/repo.git`

    复制一个仓库的 **master-分支** 到本地，首先必须知道仓库的地址。

- `git checkout -b {new-branch} origin/{dev}`

    将本地的 **{new-branch}** 分支对应远程的 **{dev}** 分支


## 分支：
- `git branch`

    查看分支；

- `git branch {new-branch}`

    创建分支；

- `git checkout {old-branch}`

    切换分支；

- `git checkout -b {new-branch}`

    创建+切换分支；

- `git branch -d {branch}`

    删除分支；


## 分支合并
-  `git merge {branch-to-be-merged}`
    - **Fast forward** （默认）

        删除分支之后，丢失了分支信息；
    - **-no-ff**

        保留分支的每一次commit历史；

    - **--squash**

        把多次分支commit历史压缩为一次，保留；

- 无法自动合并时，先编辑冲突文件为我们希望的内容。再提交，合并完成，删除分支；

    用git log --graph命令可以看到分支合并图；


## stash
- `git stash`

    当手头工作没有完成时，先把工作现场—— **WORKING COPY** `git stash`一下，然后去修复bug。修复后，再回到工作现场。可多次使用；

- `git stash list`

    显示被stash的工作现场；

- `git stash [apply | drop | pop]`

    - **apply stash@{NUM}**

        恢复至指定stash，默认0；

    - **drop**

        删除指定stash，默认所有；

    - **pop**

        恢复至指定stash，并删除；

## 开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。

## 多人协作

查看远程库信息，使用git remote -v；

本地新建的分支如果不推送到远程，对其他人就是不可见的；

从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

删除远程分支：git push origin --delete branch_need_to_be_deleted

## rebase操作可以把本地未push的分叉提交历史整理成直线；

rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

## 命令git tag <tagname>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

命令git tag -a <tagname> -m "blablabla..."可以指定标签信息；

命令git tag可以查看所有标签。

## tag 远程操作

命令git push origin <tagname>可以推送一个本地标签；

命令git push origin --tags可以推送全部未推送过的本地标签；

命令git tag -d <tagname>可以删除一个本地标签；

命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

## 忽略某些文件时，需要编写.gitignore；

.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

## git config --global alias.st status
