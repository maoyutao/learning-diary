##git reset 的三个参数
1.soft

--soft参数告诉Git重置HEAD到另外一个commit，但也到此为止。如果你指定--soft参数，Git将停止在那里而什么也不会根本变化。这意味着index,working copy都不会做任何变化，所有的在original HEAD和你重置到的那个commit之间的所有变更集都放在stage(index)区域中。



 

2.hard

--hard参数将会blow out everything.它将重置HEAD返回到另外一个commit(取决于~12的参数），重置index以便反映HEAD的变化，并且重置working copy也使得其完全匹配起来。这是一个比较危险的动作，具有破坏性，数据因此可能会丢失！如果真是发生了数据丢失又希望找回来，那么只有使用：git reflog命令了。makes everything match the commit you have reset to.你的所有本地修改将丢失。**如果我们希望彻底丢掉本地修改但是又不希望更改branch所指向的commit，则执行git reset --hard = git reset --hard HEAD.** i.e. don't change the branch but get rid of all local changes.另外一个场景是简单地移动branch从一个到另一个commit而保持index/work区域同步。这将确实令你丢失你的工作，因为它将修改你的work tree！


3.mixed(default）

--mixed是reset的默认参数，也就是当你不指定任何参数时的参数。它将重置HEAD到另外一个commit,并且重置index以便和HEAD相匹配，但是也到此为止。working copy不会被更改。所有该branch上从original HEAD（commit）到你重置到的那个commit之间的所有变更将作为local modifications保存在working area中，（被标示为local modification or untracked via git status)，但是并未staged的状态，你可以重新检视然后再做修改和commit


##git reset、git checkout和git revert
git reset、git checkout和git revert都用来撤销代码仓库中的某些更改，而前两个命令不仅可以作用于提交，还可以作用于特定文件。


###提交层面的操作

你传给git reset和git checkout的参数决定了它们的作用域。如果你没有包含文件路径，这些操作对所有提交生效。我们这一节要探讨的就是提交层面的操作。注意，git revert没有文件层面的操作。

####Reset

在提交层面上，reset将一个分支的末端指向另一个提交。这可以用来移除当前分支的一些提交。比如，下面这两条命令让hotfix分支向后回退了两个提交。

git checkout hotfix
git reset HEAD~2
hotfix分支末端的两个提交现在变成了悬挂提交。也就是说，下次Git执行垃圾回收的时候，这两个提交会被删除。换句话说，如果你想扔掉这两个提交，你可以这么做。reset操作如下图所示：

![](https://camo.githubusercontent.com/3d55df040cb530482894661b35212b83ff4e5e14/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f726573657474696e672d636865636b696e672d6f75742d616e642d726576657274696e672f30322e737667?_=5890748)


如果你的更改还没有共享给别人，git reset是撤销这些更改的简单方法。当你开发一个功能的时候发现『糟糕，我做了什么？我应该重新来过！』时，reset就像是go-to命令一样。


当你传入HEAD以外的其他提交的时候要格外小心，因为reset操作会重写当前分支的历史。

####Checkout

你应该已经非常熟悉提交层面的git checkout。当传入分支名时，可以切换到那个分支。

git checkout hotfix
上面这个命令做的不过是将HEAD移到一个新的分支，然后更新工作目录。因为这可能会覆盖本地的修改，Git强制你提交或者缓存工作目录中的所有更改，不然在checkout的时候这些更改都会丢失。和git reset不一样的是，git checkout没有移动这些分支。



除了分支之外，你还可以传入提交的引用来checkout到任意的提交。这和checkout到另一个分支是完全一样的：把HEAD移动到特定的提交。比如，下面这个命令会checkout到当前提交的祖父提交。

git checkout HEAD~2


这对于快速查看项目旧版本来说非常有用。但如果你当前的HEAD没有任何分支引用，那么这会造成HEAD分离。这是非常危险的，如果你接着添加新的提交，然后切换到别的分支之后就没办法回到之前添加的这些提交。因此，在为分离的HEAD添加新的提交的时候你应该创建一个新的分支。

####Revert

Revert撤销一个提交的同时会创建一个新的提交。这是一个安全的方法，因为它不会重写提交历史。比如，下面的命令会找出倒数第二个提交，然后创建一个新的提交来撤销这些更改，然后把这个提交加入项目中。

git checkout hotfix
git revert HEAD~2
如下图所示：

![](https://camo.githubusercontent.com/ca3c454935277b49e1c75e04644d979e796c50e8/68747470733a2f2f7777772e61746c61737369616e2e636f6d2f6769742f696d616765732f7475746f7269616c732f616476616e6365642f726573657474696e672d636865636b696e672d6f75742d616e642d726576657274696e672f30362e737667?_=5890748)


相比git reset，它不会改变现在的提交历史。因此，git revert可以用在公共分支上，git reset应该用在私有分支上。

你也可以把git revert当作撤销已经提交的更改，而git reset HEAD用来撤销没有提交的更改。

就像git checkout 一样，git revert 也有可能会重写文件。所以，Git会在你执行revert之前要求你提交或者缓存你工作目录中的更改。

###文件层面的操作
git reset和git checkout 命令也接受文件路径作为参数。这时它的行为就大为不同了。它不会作用于整份提交，参数将它限制于特定文件。

####Reset

当检测到文件路径时，git reset 将缓存区同步到你指定的那个提交。比如，下面这个命令会将倒数**第二个提交中的foo.py加入到缓存区**中，供下一个提交使用。

git reset HEAD~2 foo.py


和提交层面的git reset一样，通常我们使用HEAD而不是某个特定的提交。运行git reset HEAD foo.py 会将当前的foo.py从缓存区中移除出去(因为相当于是把head的提交的foo.py加入缓存区，而这俩个是一样的，就相当于移出去），而不会影响工作目录中对foo.py的更改。



--soft、--mixed和--hard对文件层面的git reset毫无作用，因为缓存区中的文件一定会变化，而工作目录中的文件一定不变。

####Checkout

Checkout一个文件和带文件路径git reset 非常像，除了它更改的是工作目录而不是缓存区。不像提交层面的checkout命令，它不会移动HEAD引用，也就是你不会切换到别的分支上去。



比如，下面这个命令将工作目录中的foo.py同步到了倒数第二个提交中的foo.py。

git checkout HEAD~2 foo.py
和提交层面相同的是，它可以用来检查项目的旧版本，但作用域被限制到了特定文件。

如果你缓存并且提交了checkout的文件，它具备将某个文件回撤到之前版本的效果。注意它撤销了这个文件后面所有的更改，而git revert 命令只撤销某个特定提交的更改。

和git reset 一样，这个命令通常和HEAD一起使用。比如git checkout HEAD foo.py等同于舍弃foo.py没有缓存的更改。这个行为和git reset HEAD --hard很像，但只影响特定文件。

####总结

命令	作用域	常用情景<br/>
git reset	提交层面	在私有分支上舍弃一些没有提交的更改<br/>
git reset	文件层面	将文件从缓存区中移除<br/>
git checkout	提交层面	切换分支或查看旧版本<br/>
git checkout	文件层面	舍弃工作目录中的更改<br/>
git revert	提交层面	在公共分支上回滚更改<br/>
git revert	文件层面	（然而并没有）<br/>
