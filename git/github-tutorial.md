## 1. 基本配置

### Github 安装

- [下载 git OSX 版](http://code.google.com/p/git-osx-installer/downloads/list?can=3)
- [下载 git Windows 版](http://msysgit.github.io/)
- [下载 git Linux 版](http://book.git-scm.com/2_installing_git.html) （ubuntu 自带）

### 注册账户以及创建仓库

要想使用github第一步当然是注册github账号了， github官网地址：<https://github.com/>。 之后就可以创建仓库了（免费用户只能建公共仓库），Create a New Repository，填好名称后Create，之后会出现一些仓库的配置信息，这也是一个git的简单教程。

#### 使用https协议

若使用https协议建立本地仓库与远程仓库的联系，创建过程较为简单，按照下面的命令即可，但每次pull, push都要输入密码

**create a new repository on the command line**

```
echo "# CarSim" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wsustcid/CarSim.git
git push -u origin master
```

**or push an existing repository from the command line**

```
git remote add origin https://github.com/wsustcid/CarSim.git
git push -u origin master
```

#### 使用ssh协议

**配置公钥和私钥：**

使用ssh密钥, 创建时稍微麻烦一些，但省去每次都输密码。

公钥我们一般是给服务器的,他们到时候在权限中加入我给的公钥,然后当我使用`git clone 别人的或自己的项目的时候，虽然都可以clone, 但push时那个服务器我通过他的绑定的公钥来匹配我的私钥，这个时候,如果匹配,则就可以正常push,如果不匹配,则失败（无法向别人的仓库push）.

大多数 Git 服务器都会选择使用 SSH 公钥来进行授权。系统中的每个用户都必须提供一个公钥用于授权，没有的话就要生成一个。

首先在本地创建`ssh key；`

```
$ ssh-keygen -t rsa -C "your_email@youremail.com"
```

后面的`your_email@youremail.com`改为你在github上注册的邮箱，之后会要求确认路径和输入密码，我们这使用默认的一路回车就行。成功的话会在`~/`下生成`.ssh`文件夹，进去，打开`id_rsa.pub`，复制里面的`key`。

```
xsel < ~/.ssh/id_rsa.pub
xsel
```

回到github上，进入 Account Settings（账户配置），左边选择SSH Keys，Add SSH Key,title随便填，粘贴在你电脑上生成的key。

为了验证是否成功，在git bash下输入：

```
$ ssh -T git@github.com
```

如果是第一次的会提示是否continue，输入yes就会看到：You've successfully authenticated, but GitHub does not provide shell access 。这就表示已成功连上github。(如果要输入密码，是刚才创建的密码)

接下来我们要做的就是把本地仓库传到github上去，在此之前还需要设置username和email，因为github每次commit都会记录他们。

```
$ git config --global user.name "your name"
$ git config --global user.email "your_email@youremail.com"
```

在本地创建新文件夹

```
echo "# CarSim" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin git@github.com:wsustcid/CarSim.git
git push -u origin master
```



## 2. 基本命令

```
# 帮助命令
git help 
git add help

# 仓库初始化
git init
git init projectname

# 文件基本操作
git add filename
git add -u   # 编辑或删除的文件
git add -A   # 所有文件
git add -A . # 所有改变的文件
git add .    # 新文件和编辑文件（无删除文件）
git commit -m "message"

# 查看文件修改
git status
git diff <source_branch> <target_branch>

# 查看提交log
git log

# 忽略文件
touch .gitignore   # 创建文件
vim .gitignore     # 添加ignore文件 *.log | tmp/ | .sass-cache etc...

# 分支操作
git branch branchname   # 创建分支
git checkout branchname # 切换分支
git push origin branchname # 推送本地更改至远程分支

git checkout master
git merge brancename # 合并branchname分支到目前所在的分支（本地已合并，但github并未合并）
git push  # 将本地合并推送到github master主分支（亦可手动操作 pull request）

git branch -d branchname # 删除分支-已合并的（github 并未删除）
git branch -D branchname # 删除分支-未合并的 (github 未删除)

git branch -a # 查看所有分支
git push origin --delete brancename # 删除远程分支

git checkout commitID -b brancename # 从当前分支某个commit开始创建新分支
git push origin branchname

# 远程操作
git remote add origin https://github.com/accountname/projectname
git pull origin
git push origin master # master 可以是你想要推送的任何分支

# 在本地目录于远程repository的关联与取消：
git remote remove origin
git remote add origin git@github.com:git_username/repository_name.git
git push -u origin nvidia-docker

##  版本回退
# 1. 本地代码（或文件）已经add但是还未commit； 
# 2. 要回退的commit的代码已经commit了，但是还未push到远程个人repository 
git reset --hard id

# 3. 要回退的commit的代码已经push到远程的个人分支，但是还未merge到公共的repository 
先使用git reset [commit]回退，如何使用git push -f [commit]来强制更新你的远程库2

# 4. 要回退的commit的代码已被merge（合入)到公共的repository
对于最后一种情况，考虑到其他人的版本历史，使用git reset [commit]是不建议的，此时我们应该使用git revert [commit]改命令不会修改之前的提交历史，相当于对数据做了一次逆操作，然后再执行add，commit等命令。
git revert与reset的区别是git revert会生成一个新的提交来撤销某次提交，此次提交之前的commit都会被保留，也就是说对于项目的版本历史来说是往前走的。而git reset 则是回到某次提交，类似于穿越时空。
```



#### 标签

为软件发布创建标签是推荐的。这个概念早已存在，在 SVN 中也有。你可以执行如下命令创建一个叫做 *1.0.0* 的标签：
`git tag 1.0.0 1b2e1d63ff`
*1b2e1d63ff* 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：
`git log`
你也可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性。

#### 替换本地改动

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
`git checkout -- <filename>`
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`git fetch origin`
`git reset --hard origin/master`



## 3. Github 使用

### Watch

你选择 Watching，表示你以后会关注这个项目的所有动态，这个项目以后只要发生变动，如被别人提交了 pull request、被别人发起了issue等等情况，你都会在自己的个人通知中心，收到一条通知消息，如果你设置了个人邮箱，那么你的邮箱也可能收到相应的邮件。

在设置-通知里面可以查看你watch的项目

很多技术类的交流时常更新的项目可以watch, 看别人的回答或者帮助回答问题，提升自己。

### Star

收藏项目



### Fork

当选择 fork，相当于你自己有了一份原项目的拷贝，当然这个拷贝只是针对当时的项目文件，如果后续原项目文件发生改变，你必须通过其他的方式去同步。

有一些项目，可能存在 bug 或者可以继续优化的地方，你想帮助原项目作者去完善这个项目或者单纯的想在原来项目基础上己维护一个属于自己项目, 那么你可以 fork 一份项目下来，然后自己对这个项目进行修改完善，当你觉得项目没问题了，你就可以尝试发起 pull request 给原项目作者了。

**fork并且更新一个仓库**

 现在有这样一种情形：有一个叫做Joe的程序猿写了一个游戏程序，而你可能要去改进它。并且Joe将他的代码放在了GitHub仓库上。下面是你要做的事情：

![](https://dn-linuxcn.qbox.me/data/attachment/album/201411/24/162415ki4zz0z7zy14zv3y.png)

1. **Fork他的仓库：**这是GitHub操作，这个操作会复制Joe的仓库（包括文件，提交历史，issues，和其余一些东西）。复制后的仓库在你自己的GitHub帐号下。目前，你本地计算机对这个仓库没有任何操作。
2. **Clone你的仓库**：这是Git操作。使用该操作让你发送"请给我发一份我仓库的复制文件"的命令给GitHub。现在这个仓库就会存储在你本地计算机上。
3. **更新某些文件**：现在，你可以在任何程序或者环境下更新仓库里的文件。(如于原项目开发方向不同，恰当使用分支命令，维护两个方向的项目)
4. **提交你的更改**：这是Git操作。使用该操作让你发送"记录我的更改"的命令至GitHub。此操作只在你的本地计算机上完成。
5. **将你的更改push到你的GitHub仓库**：这是Git操作。使用该操作让你发送"这是我的修改"的信息给GitHub。Push操作不会自动完成，所以直到你做了push操作，GitHub才知道你的提交。
6. **给Joe发送一个pull request**：如果你认为Joe会接受你的修改，你就可以给他发送一个pull request。这是GitHub操作，使用此操作可以帮助你和Joe交流你的修改，并且询问Joe是否愿意接受你的"pull request"，当然，接不接受完全取决于他自己。

​       如果Joe接受了你的pull request，他将把那些修改拉到自己的仓库。胜利！

**Note:** **分支和fork中请求合并于以及更新fork都可以用pull request, 只是对象和方向不同，执行时注意理解。**

**同步一个fork**
 Joe和其余贡献者已经对这个项目做了一些修改，而**你将在他们的修改的基础上，还要再做一些修改。**在你开始之前，你最好"同步你的fork"，以确保在最新的复制版本里工作。下面是你要做的：
 ![](https://dn-linuxcn.qbox.me/data/attachment/album/201411/24/162416icr0h6wzr6ec2jze.png)

1. **从Joe的仓库中取出那些变化的文件：**这是Git操作，使用该命令让你可以从Joe的仓库获取最新的文件。
2. **将这些修改合并到你自己的仓库**：这是Git操作，使用该命令使得那些修改更新到你的本地计算机（那些修改暂时存放在一个"分支"中）。记住：步骤1和2经常结合为一个命令使用，合并后的Git命令叫做"pull"。
3. **将那些修改更新推送到你的GitHub仓库**（可选）：记住，你**本地计算机不会自动更新你的GitHub仓库**。所以，唯一更新GitHub仓库的办法就是将那些修改推送上去。你可以在步骤2完成后立即执行push，也可以等到你做了自己的一些修改，并已经本地提交后再执行推送操作。

```
git remote -v 
git remote add upstream git@github.com:xxx/xxx.git
git fetch upstream
git merge upstream/master
git push 
```



**比较一下fork和同步工作流程的区别**：当你最初fork一个仓库的时候，信息的流向是从Joe的仓库到你的仓库，然后再到你本地计算机。但是最初的过程之后，信息的流向是从Joe的仓库到你的本地计算机，之后再到你的仓库。
 结论
 我希望这是一篇关于GitHub和Git 的 [fork](https://link.jianshu.com?t=https://help.github.com/articles/fork-a-repo)有用概述。现在，你已经理解了那些概念，你将会更容易地在实际中执行你的代码。GitHub关于fork和[同步](https://link.jianshu.com?t=https://help.github.com/articles/syncing-a-fork)的文章将会给你大部分你需要的代码。
 如果你是Git的初学者，而且你很喜欢这种学习方式，那么我极力推荐书籍[Pro Git](https://link.jianshu.com?t=http://git-scm.com/book)的前两个章节，网上是可以免费查阅的。







### 实用小贴士

内建的图形化 git：
`gitk`
彩色的 git 输出：
`git config color.ui true`
显示历史记录时，每个提交的信息只显示一行：
`git config format.pretty oneline`
交互式添加文件到暂存区：
`git add -i`

### 链接与资源

#### 指南和手册

- [Git 社区参考书](http://book.git-scm.com/)
- [专业 Git](http://progit.org/book/)
- [像 git 那样思考](http://think-like-a-git.net/)
- [GitHub 帮助](http://help.github.com/)
- [图解 Git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)



# git撤销已经push到远端的commit

在使用git时，push到远端后发现commit了多余的文件，或者希望能够回退到以前的版本。

先在本地回退到相应的版本：

```
git reset --hard <版本号>
// 注意使用 --hard 参数会抛弃当前工作区的修改
// 使用 --soft 参数的话会回退到之前的版本，但是保留当前工作区的修改，可以重新提交
123
```

如果此时使用命令：

```
git push origin <分支名>
1
```

会提示本地的版本落后于远端的版本；
![这里写图片描述](https://img-blog.csdn.net/20160713201707723)

为了覆盖掉远端的版本信息，使远端的仓库也回退到相应的版本，需要加上参数`--force`

```
git push origin <分支名> --force
```



## 多人合作

## 从GitHub上克隆项目，创建并上传参与者分支

Git命令行进入想要创建项目的目录后，输入
`git clone git@github.com:Joeoeoe/test.git`或者到`clone or done`选项中直接获取对应地址

```
git clone 地址
```

（这里用户名和项目名字记得改）

接下来进入test目录创建分支，我们创建两个分支，一个叫Mike，一个叫Bob，输入以下两个命令
`git branch Mike`
`git branch Bob`
然后直接输入git branch 你会看见所有的分支
![clipboard.png](https://segmentfault.com/img/bVber36?w=129&h=79)

接着把所有分支推送到GitHub上(origin是远程仓库的默认名字)
`git push origin Mike`
`git push origin Bob`
完成后就是这样子，
![clipboard.png](https://segmentfault.com/img/bVber4c?w=360&h=79)

打开GitHub上的项目页，你会发现分支多了出来
![clipboard.png](https://segmentfault.com/img/bVber4e?w=559&h=378)

## 步骤3：邀请参与者

![clipboard.png](https://segmentfault.com/img/bVber4l?w=1355&h=465)

setting下输入username，把链接发送给小伙伴同意后就邀请成功，项目创建也就完成

# 三.参与项目

接下来就是小伙伴参与项目了

## 步骤1：从GitHub上克隆项目，创建分支到本地

同样输入命令
`git clone git@github.com:Joeoeoe/test.git`

输入给git branch后你会发现并没有所有的分支，所以要`创建远程仓库的分支到本地`
比如我是Bob，输入命令
`git checkout -b Bob origin/Bob`
这样就可以在自己的分支上进行项目了

## 步骤2：参与修改项目

举个实践的例子，在test目录下创建一个文本吧，随便写什么，我弄了Hello.txt
接下来跟正常步骤一样，提交分支
`git add Hello.txt`
`git commit -m"提交Hello.txt"`

然后把分支合并到master上（开发中一般是dev作为开发线，master作为主版本，这里就简化吧）
`git checkout master`
`git merge --no-ff -m"写合并分支的commit" Bob`
以上步骤先切换到master，再把Bob分支合并到master，并且不删除Bob分支

接下来推送master到远程仓库（当然也可以把自己的分支推送上去）
`git push origin master`
`git push origin Bob`
会有如下显示
![clipboard.png](https://segmentfault.com/img/bVber4t?w=566&h=166)

打开GitHub项目页，会发现上传成功
![clipboard.png](https://segmentfault.com/img/bVber4u?w=549&h=165)

## 步骤3：有冲突怎么办

多人协作时不可避免会出现冲突的，冲突的主要原因是`同一个文件的修改`，具体看廖老师的git教程吧，非常详细
多人协作：[https://www.liaoxuefeng.com/w...](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013760174128707b935b0be6fc4fc6ace66c4f15618f8d000)
解决冲突：[https://www.liaoxuefeng.com/w...](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001375840202368c74be33fbd884e71b570f2cc3c0d1dcf000)



更多方案：

https://blog.csdn.net/dietime1943/article/details/81391835

https://blog.csdn.net/yzr1216/article/details/81114560



# Bitbucket

## clone

### cloning a Git repository

You can use Sourcetree, Git from the command line, or any client you like to clone your Git repository. These instructions show you how to clone your repository using Git from the terminal.

1. From the repository, click **+** in the global sidebar and select **Clone this repository** under **Get to work**.

2. Copy the clone command (either the SSH format or the HTTPS).
   If you are using the SSH protocol, ensure your public key is in Bitbucket and loaded on the local system to which you are cloning.

3. From a terminal window, change to the local directory where you want to clone your repository.

4. Paste the command you copied from Bitbucket, for example:

   CLONE OVER HTTPS:

   ```none
    $ git clone https://username@bitbucket.org/teamsinspace/documentation-tests.git
   ```

   CLONE OVER SSH:

   ```none
   $ git clone git@bitbucket.org:teamsinspace/documentation-tests.git
   ```

If the clone was successful, a new sub-directory appears on your local drive in the directory where you cloned your repository. This directory has the same name as the Bitbucket repository that you cloned. The clone contains the files and metadata that Git requires to maintain the changes you make to the source files.



### Cloning a Mercurial repository

CLONE OVER HTTPS:

```none
 $ hg clone https://username@bitbucket.org/teamsinspace/hg-documentation-tests
```

CLONE OVER SSH:

```none
$ hg clone ssh://hg@bitbucket.org/teamsinspace/hg-documentation-tests
```



# [Mercurial 使用教程](https://www.mercurial-scm.org/wiki/ChineseTutorial)

```
$ sudo apt-get install mercurial

hg clone http://www.selenic.com/repo/hello my-hello
hg init

hg add
hg commit

//放弃更改
hg revert

hg branchs
hg branch branch-name
hg push -new-brance

hg pull 

hg log
hg log -v
hg status
hg diff

hg cat a.txt
hg cat -r 0 a.txt
hg diff -r 0:1 a.txt

//更新到最新版本
hg update
// 到达指定版本
hg update -r 0



```

