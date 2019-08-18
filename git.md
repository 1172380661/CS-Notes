## git ##
git 是一个分布式的版本控制工具，所谓分布式就是指每个电脑上都会保存着完整的代码版本库。  
和集中式的 SVN 相比，git 的分布式特性使得代码更安全，即使某台电脑挂掉，其它电脑中也含有完整的代码。
### 远程操作 ###
### 常用命令 ###
    $ git init
将当前目录下的文件被 git 管理起来。

    $ git add <filename>
将某个文件添加到 git 暂存区(stage)。 

    $ git commit -m <message>
将暂存区的文件添加到分支上。

    $ git log
    $ git relog
打印版本变更记录 log 命令只能版本线，而 relog 可以看到撤销记录。

    $ git diff
    $ git diff head  
diff 比较工作区和暂存区的区别，diff head 比较工作区和版本区的区别。

    $ git reset
回退到某个版本。

    $ git checkout
用版本区的文件替换工作区的文件。

    $ git remote add <name> <url>
    $ git remote rm <name>
本地仓库和某个远程仓库建立联系。

    $ git push -u origin master
向某个远程仓库推送修改，-u 在第一次输入之后可以省略，以后默认都向这个仓库提交修改。

    $ git clone <url>
clone 某个git库到当前文件夹下

    $ git branch <name>
    $ git checkout <name>
	$ git checkout -b <name>
创建分支、检出分支，第三句是前两句的合并。注意如果在某个分支内进行了修改必须 commit 后才可以切换到其它分支。（可能是因为如果不提交就切换会丢失修改）

    $ git merge <name>
合并某分支到当前分支。

    $ git branch -d <name>
删除某个分支

 