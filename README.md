#创建版本库
第一步：创建一个版本库,选择一个合适的地方，创建一个空目录：


> $mkdir learngit  
$ cd learngit  
$ pwd  
/Users/michael/learngit


第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：
> git init

Git就把仓库建好了

#添加文件
编写了一个`readme.txt文件。内容如下:
>git is a version control system.  
>git is free software.

第一步，用命令`git add`,告诉Git,把文件添加到仓库:
>$ git add readme.txt

第二步，用命令`git commit`告诉Git，把文件提交到仓库：
>$ git commit -m "wrote a readme file"

`-m`后面输入的是本次提交的说明

#时光机穿梭

###显示从最近到最远的提交日志
`git log`命令显示从最近到最远的提交日志.
如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数：
>git log
>git log --pretty=oneline

Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`3628164...882e1e0`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。
>git reset --hard HEAD^  
>git reset --hard HEAD~100

Git提供了一个命令`git reflog`用来记录你的每一次命令：
>git reflog


##工作区和暂存区

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。
###查看工作区目前的状态
>git status
###查看修改
用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：
>git diff HEAD -- readme.txt
##撤销修改
`git checkout -- file`可以丢弃工作区的修改：
>git checkout --readme.txt

就是让这个文件回到最近一次`git commit`或`git add`时的状态。

ps:`git checkout -- file`命令中的`--`很重要，没有`--`，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到`git checkout`命令。

##删除文件
一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：
>$ rm test.txt

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：
>$ git rm test.txt

现在，文件就从版本库中被删除了。

另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

小结

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。

#远程仓库

第1步：创建SSH Key。
在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
> $ ssh-keygen -t rsa -C "772863869@qq.com"

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

##添加远程库
现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：  
>$ git remote add origin git@git.yonyou.com:liuwq/project.git

请千万注意，把上面的`liuwq`替换成你自己的GitHub账户名，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：
>$ git push -u origin master

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：
>$ git push origin master  
把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！ 

小结

要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`it push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；
>$ git push origin master

分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作，而SVN在没有联网的时候是拒绝干活的！当有网络的时候，再把本地提交推送一下就完成了同步，真是太方便了！

##从远程库克隆

用命令`git clone`克隆一个本地库：
>$ git clone git@github.com:LiuwqGit/Jquery-mobile.git

在远程库中，包含一个`readme.md`文件。

这样，本地库就存在了一个新的文件夹，此文件夹内也包含一个相同内容的文件`readme.md`文件夹。

小结

要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

##分支管理
分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。
![image](http://www.liaoxuefeng.com/files/attachments/001384908633976bb65b57548e64bf9be7253aebebd49af000/0)

###创建与合并分支
在版本回退里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![image](http://www.liaoxuefeng.com/files/attachments/001384908811773187a597e2d844eefb11f5cf5d56135ca000/0)

 你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![](http://www.liaoxuefeng.com/files/attachments/0013849088235627813efe7649b4f008900e5365bb72323000/0)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：
![](http://www.liaoxuefeng.com/files/attachments/00138490883510324231a837e5d4aee844d3e4692ba50f5000/0)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：
>$ git checkout -b dev  
>Switched to a new branch 'dev'

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
>$ git branch dev  
>$ git checkout dev  
>Switched to branch 'dev'

然后，用`git branch`命令查看当前分支：
>$ git branch  
>* dev  
>  master  

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交.
比如，修改某个文件，之后使用`git add` ,`git commit`两个命令提交。

现在，dev分支的工作完成，我们就可以切换回master分支：
>$ git checkout master  
>Switched to branch 'master'

切换回`master`分支后，再查看一个readme.txt文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：
![image](http://www.liaoxuefeng.com/files/attachments/001384908892295909f96758654469cad60dc50edfa9abd000/0)

现在，我们把`dev`分支的工作成果合并到`master`分支上：
>$ git merge dev

合并完成后，就可以放心地删除`dev`分支了：
>$ git branch -d dev

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

##解决冲突
创建+切换分支，命令格式为：
>$ git checkout -b feature1

切换分支
>$ git checkout master





# Git 阮一峰教程 #
## git clone ##
>$ git clone http[s]://example.com/path/to/repo.git/  
>$ git clone ssh://example.com/path/to/repo.git/  
>$ git clone git://example.com/path/to/repo.git/  
>$ git clone /opt/git/project.git   
>$ git clone file:///opt/git/project.git  
>$ git clone ftp[s]://example.com/path/to/repo.git/  
>$ git clone rsync://example.com/path/to/repo.git/  
>$ git clone git@github.com:LiuwqGit/learngit.git  

## git remote ##

为了便于管理，Git要求每个远程主机都必须指定一个主机名。`git remote`命令就用于管理主机名。  
不带选项的时候，`git remote`命令列出所有远程主机。
>$ git remote
origin

使用`-v`选项，可以参看远程主机的网址。
>$ git remote -v
origin git@github.com:LiuwqGit/learngit.git (fetch)
origin git@github.com:LiuwqGit/learngit.git (push)


# **廖雪峰 Git 教程学习记录** #


mkdir learngit    
cd learngit   
git init  #初始化Git仓库  
** create readme.txt  ** echo "line 1 ">> readme.txt   
git add readme.txt  #添加到暂存空间  
git commit -m "first commit , 'line1'"  #提交加备注  
** echo "error line 2" >> readme.txt   
git add readme.txt   
git status  #查看状态  
** have a change.  
git reset HEAD readme.txt  #将暂存空间内容撤销  
git checkout readme.txt  #使用版本库里的内容覆盖工作空间内容,同时达到撤销工作空间内容的目的  
** echo "line2" >> readme.txt   
git add readme.txt    
git commit -m "commit line 2 "   
git rest --hard HEAD^ #回退为上个版本  
git rest --hard (gitID) #回退为指定版本  
git log #查看当前版本及之前版本的id +--pretty=oneline简化显示  
git ref log #查看所有版本的id  

  
远程仓库:  
github.com 注册  
打开Git bash   
$ ssh-keygen -t rsa -C "937203399@qq.com" #创建本机的key  
打开github.com 登陆,添加SSH KEYS  
切换到本地库  
$ git remote add origin git@github.com:ooran/learngit.git   #关联远程仓库  
$ git push -u origin master #将本地库push到远程仓库 ,第一次加-u 关联 之后可以不加  
 
克隆：  
$ git clone git@github.com:ooran/gitkills.git  

分支： 未提交的内容在工作区,所有分支都可以看到,commit后的内容在对应的分支内,切换只能在对应的分支内看到.  
git branch #查看分支 *为当前分支  
git branch <name> #创建分支  
git checkout <name> #切换分支  
git checnout -b <name>  #创建并切换分支  
git merge <name>  #合并某分支到当前分支  
git branch -d <name> #删除分支  
git branch -D <name> #强行删除分支  

  
分支冲突：合并时显示分支冲突先merge 后修改<<<<分支1 <<<<分支2 之间的冲突内容,然后在提交. 当前分支会比被合并的分支多commit一次  
git log --graph --pretty=oneline --abbrev-commit  #查看分支情况  
git merge --no-ff <name>  #禁用Fast forward,不删除分支,合并后  保留分支  



BUG分支：正在dev分支工作,需要修改bug并提交,应该先把dev当前工作区stash储藏起来.修改完bug提交后在使用 git stash pop恢复dev的内容到工作区.  
git stash #储藏当前工作区  
git stash list #查看  
git stash apply <stashid> #恢复指定内容到工作区,不在stash内删除..stashid通过git stash list查询
git stash drop <stashid>  #删除
git stash pop  #恢复stash的内容到工作区,并在stash内删除


标签：
git tag <name>  #新建标签,默认为HEAD，也可以指定一个commit id.  
git tag -a <tagname> -m "blablablabla"  #指定标签信息  
git tag -s <tagname> -m "blablablabla"  #使用PGP签名标签  
git tag #查看所有标签   
git show <tagname> #查看指定tag的详细内容  
git tag -d <tagname> #删除tag  
git push origin <tagname> #推送指定标签到远程  
git push origin --tags #推送所有标签到远程  
git tag -d <tagname>  
git push origin :refs/tags/<tagname> #从远程删除标签  

>