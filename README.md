转载自http://www.jianshu.com/p/8a499ee39fe7
1 init：新建一个Git管理项目。

2 add：添加新的文件（文件夹）到Git项目中，如果添加文件夹，该文件夹下所有文件将被包含。同时可以使用rm，mv从git项目中删除或是重命名文件（文件夹）。

3 commit：提交到仓库，告诉Git你想要记录现在的操作，Git会保留一个当前修改过文件的快照。

4 reset：如果你正在编辑的文件乱了，可以选择从上一次的commit的点重新开始编辑，通常是选择恢复到上一个编辑点。

5 check out：一般是在branch间切换。

6 branch：分支，master就是其中一个。

7 merge：合并分支，如果我正在编辑一个版本a，别人在编辑版本b，我们想把两个版本合成一个，就可以用merge。当然，合的过程中，有时候会检出有哪些地方不一样，询问到底要保留哪一个，需要手动处理不同的地方。事实上，这更像一个审查的过程。

8 diff：找出两个文档或目录的不同。

9 revert：回滚到指定的commit的点。

和远程仓库的互动：

10 clone：从远程仓库得到整个项目的拷贝。

11 pull：类似与SVN中的update动作，如果你之前clone得到某项目的一份拷贝，用pull可以更新到最新版本。相当于fetch + merge

12 push：把本地仓库的这份拷贝push到服务器。

13 HEAD：这就是一个指针，可以有任意指向，默认指向master分支的最后一次commit的点，这货控制着后面的发展，可以使用checkout更改HEAD指向。

14 master：这是系统默认生成的本地分支，当然你可以自定义，这只是方便操作而已

15 working tree:刚check out过来，并未修改的文件，也就是你在对哪些文件进行操作。

16 index（staging area）：有修改但是还没有commit的文件，新加进来的文件也在这里，就是暂存区咯

17 git directory（repository）：修改并commit后，一个文件快照被推送到这里，被保存起来，就是本地仓库

易混淆名词对比

1 log和status：log是查看commit的历史；status是查看是否有文件未commit，没有的话，当前的Git project是clean的。

2 reset和revert：首先，‘reset --hard’到某commit点后，用log查看，该点后所有commit都看不到了，文件被恢复到commit点时的样子；而revert到某commit点之后，用log查看，可以看到多了一个commit点，查看文件内容，被恢复到commit点。还有，必须是project clean时，才能执行revert。

git中文件的几种状态

1 unstaged：git仓库中没有此文件的相关记录

2 modified：git仓库中有这个文件的记录，并且此文件当前有改动

3 staged：追加,删除或修改的文件被暂时保存，这些追加,删除和修改并没有提交到git仓库

4 commited：追加或修改的文件被提交到本地git仓库（git仓库中大部分都是这种文件，所以git status不显示这些文件）

git全局部署

$ git config --global user.name "your name"  
$ git config --global user.email "your_email@xx.com"
本地创建ssh key

命令：

ssh-keygen -t rsa -C "your_email@xx.com"
完成后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在~/下生成.ssh文件夹。进去，打开id_rsa进去，打开id_rsa.pub，这就是我们需要的ssh key。回到github，进入Account

添加key到github

打开github，进入Account Settings，左边选择SSH Keys，Add SSH Key,title随便填，粘贴key。为了验证是否成功，在git bash下输入：

$$ ssh -T git@github.com
如果是第一次的会提示是否continue，输入yes就会看到：You’ve successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。

提交、上传

1 创建仓库

到你的github页面，创建一个仓库，比如HelloWorld

2 提交

本地新建一个文件夹HelloWorld(和你的github里的仓库名称一致)，进入该文件夹，右键git bash，初始化git，添加文件并提交commit

$ git init //初始化git，创建.git文件夹
$ git add README.md //建立一个待提交的文件README.md
$ echo "hello world!" >> README.md  //文件里写点东西，问候下美好的世界
$ git commit . -m "first commit" //提交文件，.是当前目录，就是提交所有文件
3 上传

$ git remote add origin git@github.com:yourName/yourRepo.git
$ git push -u origin master //可能需要输入用户名密码之类哦
origin可以是任意名字哦，是你远程仓库名，当然你可以添加多个哦，push的时候指定一个就可以。后面的yourName和yourRepo表示你再github的用户名和刚才新建的仓库，加完之后进入.git文件夹，打开config文件，这里会多出一个remote “origin”内容，这就是刚才添加的远程地址，也可以直接修改config来配置远程地址。

当然后面的地址是ssh形式，前面没有部署密钥的话是出错的哦，你也可以用https形式来上传：

$ git remote add origin https://github.com/yourName/yourRepo.git
这样你就会在你的github对应的仓库下看到对应的文件了哦。
git push命令会将本地仓库推送到远程服务器。git pull命令则相反。

$ git pull -u origin master //从远程服务器更新到本地仓库，相当于git fetch + git merge
修改完代码后，使用git status可以查看文件的差别，使用git add 添加要commit的文件，也可以用git add -i来智能添加文件。之后git commit提交本次修改，git push上传到github。
