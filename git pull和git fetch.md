Git中从远程的分支获取最新的版本到本地有这样2个命令：

1. git fetch：相当于是从远程获取最新版本到本地，不会自动merge


 ```    
 Git fetch origin master

 git log -p master..origin/master

 git merge origin/master

 ```

   以上命令的含义：
  
   首先从远程的origin的master主分支下载最新的版本到origin/master分支上
   
   然后比较本地的master分支和origin/master分支的差别
   
   最后进行合并
   
   上述过程其实可以用以下更清晰的方式来进行：
   
  ``` 
  git fetch origin master:tmp
 
 git diff tmp 

 git merge tmp
  ```

    
    
 从远程获取最新的版本到本地的test分支上
 
   之后再进行比较合并
   
2. git pull：相当于是从远程获取最新版本并merge到本地
`
 git pull origin master`

 上述命令其实相当于git fetch 和 git merge
 
 在实际使用中，git fetch更安全一些
因为在merge前，我们可以查看更新情况，然后再决定是否合并
结束


***

看左边

一旦远程主机的版本库有了更新(Git术语叫做commit)，需要将这些更新取回本地，这时就要用到git fetch命令。

$ git fetch <远程主机名>
上面命令将某个远程主机的更新，全部取回本地。

默认情况下，git fetch取回所有分支(branch)的更新。如果只想取回特定分支的更新，可以指定分支名。


 
$ git fetch <远程主机名> <分支名>
比如，取回origin主机的master分支。

$ git fetch origin master
所取回的更新，在本地主机上要用”远程主机名/分支名”的形式读取。比如origin主机的master，就要用origin/master读取。

git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。

$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
上面命令表示，本地主机的当前分支是master，远程分支是origin/master。

取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。


  

 


$ git checkout -b newBrach origin/master
上面命令表示，在origin/master的基础上，创建一个新分支。

此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。

$ git merge origin/master
 或者
$ git rebase origin/master
上面命令表示在当前分支上，合并origin/master。


