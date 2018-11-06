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

    复制一个仓库的 **master-分支** 到本地，首先必须知道仓库的地址；

- `git remote -v`

    显示更详细的远程库信息；

- `git checkout -b {new-branch} origin/{dev}`

    将本地的 **{new-branch}** 分支对应远程的 **{dev}** 分支；

- `git push origin {branch}`
    向远程库推送 **{branch}** 分支，简单的`git push`为推送当前分支；
    - **-f** 强制更新远程端与本地同步

- `git push origin --delete {branch_need_to_be_deleted}`

    删除远程分支；

- `git pull origin {remote-branch}:{local-branch}`

    取回远程主机origin某个分支的更新，再与本地的指定分支合并。本质是`git fech` + `git  merge`；

## 分支：
- `git branch`

    查看分支；

- `git branch {new-branch}`

    创建分支；

- `git checkout {old-branch}`

    切换分支；

- `git checkout -b {new-branch}`

    创建+切换分支；

- `git branch -[d | D] {branch}`

    删除分支；

    - **-d** 常用命令；
    - **-D** 若分支有改动未commit，-d无法删除；


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

    当手头工作没有完成时，先把工作现场—— **WORKING COPY** `git stash`一下，然后去修复bug。修复后，再回到工作现场。可多次使用，为栈式结构；

- `git stash list`

    显示被stash的工作现场；

- `git stash [apply | drop | pop | clear]`

    - **apply stash@{NUM}**

        恢复至指定stash，默认栈顶；

    - **drop**

        删除指定stash，默认栈顶；

    - **pop**

        恢复至指定stash，并删除该存储，默认栈顶；

    - **clear**

        删除stash所有缓存；


## 变基？？

变基命令在于将分支上的合并操作简化为线性操作。git将分支的修改结合master的修改，使分支上的操作插入到合并后的那一刻，项目历史会非常整洁；

**绝不要在公共的分支上使用它！** 可在自己的私有小分支上使用，方便其他人查看修改。

- `git rebase`

    将当前分支变直；

- `git rebase -i {branch} | HEAD~NUM`

    - **{branch}**

    将当前分支和{branch}分支合并，打开交互式rebase；

    - **HEAD~NUM**

    rebase当前分支最后NUM次提交；


## 标签
- `git tag`

    查看所有标签

- `git tag {tagname} {commit_id}`
    - **commit_id**
        （可省略，默认最新一次提交）上打上标签{tagname}；

    - **-a {tagname}**
        指定标签名；

    - **-m {comment}**
        指定说明文字；

- `git show {tagname}`

    查看标签为{tagname}的提交信息；

- `git tag -d {tagname}`

    删除名为{tagname}的标签；

- `git push origin {tagname}`

    推送某个标签到远程；

- `git push origin --tags`

    一次性推送全部尚未推送到远程的本地标签；

- `git push origin :refs/tags/{tagname}`

    删除远程标签，需要先删除本地tag；


## git配置
本地的global配置文件：**~/.gitconfig**，也可自行按喜好手动设置如下：
```
git config --global alias.st status
git config --global alias.ck checkout
git config --global alias.cm "commit -m"
git config --global alias.br branch
git config --global alias.unstage 'reset HEAD'
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

## 忽略特殊文件
在项目根目录下，编写**.gitignore**，注释行为“#”。**.gitignore** 文件本身要放到版本库里，并且可以对.gitignore做版本管理！；

- `git add -f {filename}`

    强制添加被被忽略的文件；
