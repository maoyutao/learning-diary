##创建版本库
**此过程可用github desktop**


首先，选择一个合适的地方，创建一个空目录：

```
$ mkdir learngit

$ cd learngit

$ pwd

/Users/michael/learngit
```

pwd命令用于显示当前目录。在我的Mac上，这个仓库位于/Users/michael/learngit。

第二步，通过git init命令把这个目录变成Git可以管理的仓库：

```
$ git init
```
##把文件添加到版本库
**此过程可用github desktop,add对应勾选，commit一样**

第一步，用命令git add告诉Git，把文件添加到仓库：

```
$ git add readme.txt
```

执行上面的命令，没有任何显示，这就对了，Unix的哲学是“没有消息就是好消息”，说明添加成功。

第二步，用命令git commit告诉Git，把文件提交到仓库：

```
$ git commit -m "wrote a readme file"

[master (root-commit) cb926e7] wrote a readme file
 1 file changed, 2 insertions(+)
 create mode 100644 readme.txt 
```
 简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

输入说明对自己对别人阅读都很重要。

为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件

工作区 add到暂存区 暂存区commit到分支 如果不add，就不会被commit

同理，在版本库中删除文件也只需两步（在文件被删除后）

 
 ```
 git rm readme.txt
 
 git commit -m"balabala"
 ```
 （删除的撤销只能复原原先版本库的版本，最近一次commit之后在文件上的修改不会被复原）
 
##版本管理
#### 查看
1.查看当前状态（是否有修改，是否有add但未提交的）

```
git status
```
**github desktop中会自动检测是否有uncommitted changes**

2.查看提交日志（显示从近到远的commit日志）

```
git log
```
加上`--pretty=oneline`简化输出,只显示commit id和说明

```
git log --pretty=oneline
```
3.查看每次命令
(跟2的区别：2回退后只显示所在版本之前的，不显示所在版本之后的，要回到所在版本之后的版本需要知道commit id，一是上翻记录，而是使用reflog）

press return for more, press q to end

```
git reflog
```

**github desktop 中直接看history，在history复原相当于又做了一次commit，有记录，但在左边界面用undo撤销就没有记录了**

4.(cat 文件名 显示内容在终端，**github desktop 里就在左边那里右键-显示在finder 然后打开就好了，当前处在哪个版本，哪个分支就会显示哪个形态的文件**） 
#### 操作
1. 退回

 在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
 
 ```
 git reset --hard head ^
 ```
 或者
 
 ```
 git reset --hard 3628164
 ```
 版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了

 **github desktop 在history复原或在左边界面用undo撤销，区别见上面**
 
2. 撤销修改

  命令`git checkout -- readme.txt`意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

 一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

 一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

 总之，就是让这个文件回到最近一次git commit或git add时的状态。

##远程库
要关联一个远程库，使用命令
```git remote add origin git@server-name:path/repo-name.git；```

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改； 

克隆：` git clone git@github.com:maoyutao/gitskills.git`

(**这个在github desktop上有**）

 
##分支
基础：

查看分支：`git branch` 命令会列出所有分支，当前分支前面会标一个*号

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并指定分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

用`git log --graph`命令可以看到分支合并图。

（**除了删除分支，github desktop上都有相应操作**）

 高级：
 
1. `git stash` 
 
 Git把stash内容存在某个地方,解决bug后，一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；
另一种方式是用`git stash pop`，恢复的同时把stash内容也删了

2. 开发一个新feature，最好新建一个分支；

 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。
 
##多人协作


总结 ：

首先，可以试图用`git push origin branch-name`推送自己的修改；

如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`(当前分支自动与唯一一个追踪分支进行合并)试图合并；

如果合并有冲突，则解决冲突，并在本地提交；

没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。

##标签
`git tag <name>`用于新建一个标签，默认为HEAD，也可以指定一个commit id；

eg. `git tag v0.9 6224937`

`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；

`git tag -s <tagname> -m "blablabla...`"可以用PGP签名标签；

`git tag`可以查看所有标签。

`git show <tagname>`查看标签信息。

`git push origin <tagname>`可以推送一个本地标签；

`git push origin --tags`可以推送全部未推送过的本地标签；

`git tag -d <tagname>`可以删除一个本地标签；

`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

##其他 
1. 忽略某些文件（**github desktop 上有**）

  忽略某些文件时，需要编写.gitignore；

 .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
 
2. 使用github

 在GitHub上，可以任意Fork开源仓库；点“Fork”就在自己的账号下克隆了一个bootstrap仓库，然后，从自己的账号下clone：

 自己拥有Fork后的仓库的读写权限；

 可以推送pull request给官方仓库来贡献代码。
  一旦你从自己的主题branch（例如fix-unicode-error）推送了一条Pull Request，那么在这条Pull Request被关闭之前，再次向这个branch里push代码，所有的commits都会被自动追加到这个Pull Request后面（不需要再另开Pull Request）。
