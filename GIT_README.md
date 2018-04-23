# Git学习笔记 #
# 1.Git简介 #
## 1.1安装Git ##
1. 在Linux上安装Git
	>`首先，你可以试着输入git，看看系统有没有安装Git`
	`$git`
	`The program 'git' is currently not installed. You can install it by typing:
	sudo apt-get install git
	sudo apt-get install git`
2. 在Mac OS X上安装Git 
	>`直接从AppStore安装Xcode，Xcode集成了Git，不过默认没有安装`
3. 在Windows上安装Git
	>`https://git-scm.com/downloads`
4. 安装后的配置
	>`git config --global user.name "LiangziKing"`
	>`git config --global user.email"1990181918@qq.com"`
	>`git config --global core.autocrlf false  //禁用自动转换 `
	>`注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。`
## 1.2.创建版本库 ##
    1. 创建一个版本库
	>mkdir learngit
	>cd learngit
	>pwd
	2. 通过git init命令把这个目录变成Git可以管理的仓库
	>git init
	3. 添加文件到Git仓库，分两步：
	>git add <file> 
	>git commit -m "XXX"

# 2.时光机穿梭 #
	>1. git status 要随时掌握工作区的状态，使用git status命令。
	>2. git diff 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
## 2.1版本回退 ##
	>1. 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本
	>2. HEAD指向的版本就是当前版本 git reset --hard commit_id
	>3. 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
## 2.2工作区和暂存区 ##
	>1. 工作区（Working Directory）就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：
	>2. 版本库（Repository）工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
## 2.3管理修改 ##
	>1. 如果不add到暂存区，那就不会加入到commit中。
	>2. 提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别
## 2.4撤销修改 ##
	>1. `git checkout -- file`可以丢弃工作区的修改
	>2. `git checkout -- file`命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令，我们在后面的分支管理中会再次遇到git checkout命令。
	>3. `git reset HEAD file` 在commit之前，git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。
## 2.5删除文件 ##
	>你通常直接在文件管理器中把没用的文件删了，或者用rm命令删了：
	>git status
	>要从版本库中删除该文件，那就用命令git rm删掉，并且git commit
	>git checkout -- <file>
	>命令git rm用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
	>git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
# 3.远程仓库 #
	1. 创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
	>ssh-keygen -t rsa -C "1990818918@qq.com"
	2. 登陆GitHub，打开“Account settings”，“SSH Keys”页面然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
## 3.1添加远程库 ##
	1.登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：
	2.根据GitHub的提示，在本地的learngit仓库下运行命令：
	>remote add origin git@github.com:LiangziKing/learngit.git
	3.要关联一个远程库，使用命令git remote add origin git@server-name:path/repo-name.git；
	4.关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
	5.此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
## 3.2从远程库克隆 ##
	>git clone git@github.com:LiangziKing/gitskills.git

# 4分支管理 #
	分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。
	![](https://i.imgur.com/08gepYJ.png)
	如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！
## 4.1创建与合并分支 ##
	首先，我们创建dev分支，然后切换到dev分支
	>git checkout -b dev
	>|| git checkout命令加上-b参数表示创建并切换，相当于以下两条命令
	>git branch dev
	>git checkout dev
	>然后，用git branch命令查看当前分支.

	Git鼓励大量使用分支：

	查看分支：git branch

	创建分支：git branch <name>

	切换分支：git checkout <name>

	创建+切换分支：git checkout -b <name>
	
	合并某分支到当前分支：git merge <name>

	删除分支：git branch -d <name>
## 4.2解决冲突 ##
	git log --graph --pretty=oneline --abbrev-commit
## 4.3分支管理策略 ##
	git merge --no-ff -m "merge with no-ff" dev
	合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
	git log --graph --pretty=oneline --abbrev-commit
## 4.4Bug分支 ##
	stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
	>git stash
	>修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
	>当手头工作没有完成时，先把工作现场git stash一下，然后去修复bug，修复后，再git stash pop，回到工作现场。
	>工作区是干净的，刚才的工作现场存到哪去了？
	>git stash list
	>git stash apply恢复
	>git stash pop，恢复的同时把stash内容也删了
## 4.5Feature分支 ##
添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

	git checkout -b feature-vulcan
	Switched to a new branch 'feature-vulcan'
就在此时，接到上级命令，因经费不足，新功能必须取消！

虽然白干了，但是这个分支还是必须就地销毁：

	git branch -d feature-vulcan
	error: The branch 'feature-vulcan' is not fully merged.
	If you are sure you want to delete it, run 'git branch -D feature-vulcan'.
销毁失败。Git友情提醒，feature-vulcan分支还没有被合并，如果删除，将丢失掉修改，如果要强行删除，需要使用命令git branch -D feature-vulcan。

现在我们强行删除：

	$ git branch -D feature-vulcan
	Deleted branch feature-vulcan (was 756d4af).
终于删除成功！
## 4.6多人协作 ##
要查看远程库的信息，用git remote：

	$ git remote
	origin

或者，用git remote -v显示更详细的信息：

	$ git remote -v
	origin  git@github.com:michaelliao/learngit.git (fetch)
	origin  git@github.com:michaelliao/learngit.git (push)
推送分支

	$ git push origin master

如果要推送其他分支，比如dev，就改成：

	$ git push origin dev

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- master分支是主分支，因此要时刻与远程同步；

- dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；

- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；

- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

抓取分支

因此，多人协作的工作模式通常是这样：



1. 首先，可以试图用git push origin branch-name推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！


	如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。
	这就是多人协作的工作模式，一旦熟悉了，就非常简单。


小结


 - 查看远程库信息，使用git remote -v；

- 本地新建的分支如果不推送到远程，对其他人就是不可见的；

- 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

- 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

- 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

- 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


# 5标签管理 #

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起

## 5.1创建标签 ##

在Git中打标签非常简单，首先，切换到需要打标签的分支上：
    
	$ git branch
	* dev
	  master
	$ git checkout master
	Switched to branch 'master'

然后，敲命令git tag <name>就可以打一个新标签：

	$ git tag v1.0

可以用命令git tag查看所有标签：

	$ git tag
	v1.0

默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？

方法是找到历史提交的commit id，然后打上就可以了：

	$ git log --pretty=oneline --abbrev-commit
	6a5819e merged bug fix 101
	cc17032 fix bug 101
	7825a50 merge with no-ff
	6224937 add merge
	59bc1cb conflict fixed
	400b400 & simple
	75a857c AND simple
	fec145a branch test
	d17efd8 remove test.txt
比方说要对add merge这次提交打标签，它对应的commit id是6224937，敲入命令：

	$ git tag v0.9 6224937
再用命令git tag查看标签：

	$ git tag
	v0.9
	v1.0
注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

	$ git show v0.9
	commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Thu Aug 22 11:22:08 2013 +0800

    add merge
	...
可以看到，v0.9确实打在add merge这次提交上。

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
	
	$ git tag -a v0.1 -m "version 0.1 released" 3628164
用命令git show <tagname>可以看到说明文字：

	$ git show v0.1
	tag v0.1
	Tagger: Michael Liao <askxuefeng@gmail.com>
	Date:   Mon Aug 26 07:28:11 2013 +0800
	
	version 0.1 released
	
	commit 3628164fb26d48395383f8f31179f24e0882e1e0
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Tue Aug 20 15:11:49 2013 +0800

    append GPL

还可以通过-s用私钥签名一个标签：
	
	$ git tag -s v0.2 -m "signed version 0.2 released" fec145a

小结


- 命令git tag <name>用于新建一个标签，默认为HEAD，也可以指定一个commit id；

- git tag -a <tagname> -m "blablabla..."可以指定标签信息；

- git tag -s <tagname> -m "blablabla..."可以用PGP签名标签；

- 命令git tag可以查看所有标签。


## 5.2操作标签 ##
如果标签打错了，也可以删除：

	$ git tag -d v0.1
	Deleted tag 'v0.1' (was e078af9)

因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

如果要推送某个标签到远程，使用命令git push origin <tagname>：

	$ git push origin v1.0
	Total 0 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v1.0 -> v1.0

或者，一次性推送全部尚未推送到远程的本地标签：

	$ git push origin --tags
	Counting objects: 1, done.
	Writing objects: 100% (1/1), 554 bytes, done.
	Total 1 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v0.2 -> v0.2
	 * [new tag]         v0.9 -> v0.9

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	$ git tag -d v0.9
	Deleted tag 'v0.9' (was 6224937)

然后，从远程删除。删除命令也是push，但是格式如下：

	$ git push origin :refs/tags/v0.9
	To git@github.com:michaelliao/learngit.git
	 - [deleted]         v0.9

要看看是否真的从远程库删除了标签，可以登陆GitHub查看。


小结

- 命令git push origin <tagname>可以推送一个本地标签；

- 命令git push origin --tags可以推送全部未推送过的本地标签；

- 命令git tag -d <tagname>可以删除一个本地标签；

- 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

# 6 使用GitHub #

小结

- 在GitHub上，可以任意Fork开源仓库；

- 自己拥有Fork后的仓库的读写权限；

- 可以推送pull request给官方仓库来贡献代码。


# 7 使用码云 #

项目名称最好与本地库保持一致：

然后，我们在本地库上使用命令`git remote add`把它和码云的远程库关联：

	git remote add origin git@gitee.com:liaoxuefeng/learngit.git

之后，就可以正常地用`git push`和`git pull`推送了！

如果在使用命令`git remote add`时报错：

    git remote add origin git@gitee.com:liaoxuefeng/learngit.git
    fatal: remote origin already exists.

这说明本地库已经关联了一个名叫`origin`的远程库，此时，可以先用`git remote -v`查看远程库信息：

	git remote -v
	origin    git@github.com:michaelliao/learngit.git (fetch)
	origin    git@github.com:michaelliao/learngit.git (push)

可以看到，本地库已经关联了origin的远程库，并且，该远程库指向GitHub。

我们可以删除已有的GitHub远程库：

	git remote rm origin

再关联码云的远程库（注意路径中需要填写正确的用户名）：

	git remote add origin git@gitee.com:liaoxuefeng/learngit.git
此时，我们再查看远程库信息：

	git remote -v
	origin    git@gitee.com:liaoxuefeng/learngit.git (fetch)
	origin    git@gitee.com:liaoxuefeng/learngit.git (push)

现在可以看到，origin已经被关联到码云的远程库了。通过git push命令就可以把本地库推送到Gitee上。

有的小伙伴又要问了，一个本地库能不能既关联GitHub，又关联码云呢？

答案是肯定的，因为git本身是分布式版本控制系统，可以同步到另外一个远程库，当然也可以同步到另外两个远程库。

使用多个远程库时，我们要注意，git给远程库起的默认名称是`origin`，如果有多个远程库，我们需要用不同的名称来标识不同的远程库。

仍然以`learngit`本地库为例，我们先删除已关联的名为`origin`的远程库：

	git remote rm origin

然后，先关联GitHub的远程库：

	git remote add github git@github.com:michaelliao/learngit.git

注意，远程库的名称叫`github`，不叫`origin`了。

接着，再关联码云的远程库：

	git remote add gitee git@gitee.com:liaoxuefeng/learngit.git

同样注意，远程库的名称叫`gitee`，不叫`origin`。

现在，我们用`git remote -v`查看远程库信息，可以看到两个远程库：

	git remote -v
	gitee    git@gitee.com:liaoxuefeng/learngit.git (fetch)
	gitee    git@gitee.com:liaoxuefeng/learngit.git (push)
	github    git@github.com:michaelliao/learngit.git (fetch)
	github    git@github.com:michaelliao/learngit.git (push)

如果要推送到GitHub，使用命令：

	git push github master
如果要推送到码云，使用命令：

	git push gitee master
这样一来，我们的本地库就可以同时与多个远程库互相同步：

	┌─────────┐ ┌─────────┐
	│ GitHub  │ │  Gitee  │
	└─────────┘ └─────────┘
	     ▲           ▲
	     └─────┬─────┘
	           │
	    ┌─────────────┐
	    │ Local Repo  │
	    └─────────────┘
码云也同样提供了`Pull request`功能，可以让其他小伙伴参与到开源项目中来。你可以通过Fork我的仓库：https://gitee.com/liaoxuefeng/learngit，创建一个your-gitee-id.txt的文本文件，
写点自己学习Git的心得，然后推送一个pull request给我，这个仓库会在码云和GitHub做双向同步。

# 8 自定义Git #
让Git显示颜色，会让命令输出看起来更醒目：

	$ git config --global color.ui true

## 8.1 忽略特殊文件 ##
在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件

忽略文件的原则是：



1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通
另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。


小结

- 忽略某些文件时，需要编写.gitignore；

- .gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
## 8.2 配置别名  ##
如果敲git st就表示git status

	$ git config --global alias.st status

当然还有别的命令可以简写，很多人都用co表示checkout，ci表示commit，br表示branch：

	$ git config --global alias.co checkout
	$ git config --global alias.ci commit
	$ git config --global alias.br branch

## 8.3  ##


## 8.4  ##


## 8.2  ##


## 8.2  ##