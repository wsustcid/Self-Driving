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

```python
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
git reset --hard id # 这样会丢弃刚才commit时做的更改，真正回到历史id，工作区将没有要commit的文件

git reset id # 这样是比较安全的做法，仅仅是撤销了刚才的commit,历史id之后所做的更新将会保留，你可以重新选择commit的文件

# 3. 要回退的commit的代码已经push到远程的个人分支，但是还未merge到公共的repository 
先使用git reset [commit]回退，如何使用git push -f [commit]来强制更新你的远程库2

# 4. 要回退的commit的代码已被merge（合入)到公共的repository
对于最后一种情况，考虑到其他人的版本历史，使用git reset [commit]是不建议的，此时我们应该使用git revert [commit]改命令不会修改之前的提交历史，相当于对数据做了一次逆操作，然后再执行add，commit等命令。
git revert与reset的区别是git revert会生成一个新的提交来撤销某次提交，此次提交之前的commit都会被保留，也就是说对于项目的版本历史来说是往前走的。而git reset 则是回到某次提交，类似于穿越时空。


## 合并
```



#### 撤销相关

假如你操作失误（当然，这最好永远不要发生），你可以使用如下命令替换掉本地改动：
`git checkout -- <filename>`
此命令会使用 HEAD 中的最新内容替换掉你的工作目录中的文件。已添加到暂存区的改动以及新文件都不会受到影响。

假如你想丢弃你在本地的所有改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它：
`git fetch origin`
`git reset --hard origin/master`



```python
1. git add 添加 多余文件 
这样的错误是由于， 有的时候 可能

git add . （空格+ 点） 表示当前目录所有文件，不小心就会提交其他文件

git add 如果添加了错误的文件的话

撤销操作

git status 先看一下add 中的文件 
git reset HEAD 如果后面什么都不跟的话 就是上一次add 里面的全部撤销了 
git reset HEAD XXX/XXX/XXX.java 就是对某个文件进行撤销了

2. git commit 错误

如果不小心 弄错了 git add后 ， 又 git commit 了。 
先使用 
git log 查看节点 
commit xxxxxxxxxxxxxxxxxxxxxxxxxx 
Merge: 
Author: 
Date:

然后 
git reset commit_id

over

PS：还没有 push 也就是 repo upload 的时候

git reset commit_id （回退到上一个 提交的节点 代码还是原来你修改的） 
git reset –hard commit_id （回退到上一个commit节点， 代码也发生了改变，变成上一次的）

3.如果要是 提交了以后，可以使用 git revert

还原已经提交的修改 
此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交 
git revert HEAD 撤销前一次 commit 
git revert HEAD^ 撤销前前一次 commit 
git revert commit-id (撤销指定的版本，撤销也会作为一次提交进行保存） 
git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，版本会递增，不影响之前提交的内容。
```

#### 标签

为软件发布创建标签是推荐的。这个概念早已存在，在 SVN 中也有。你可以执行如下命令创建一个叫做 *1.0.0* 的标签：
`git tag 1.0.0 1b2e1d63ff`
*1b2e1d63ff* 是你想要标记的提交 ID 的前 10 位字符。可以使用下列命令获取提交 ID：
`git log`
你也可以使用少一点的提交 ID 前几位，只要它的指向具有唯一性。

#### .gitignore

<https://www.jianshu.com/p/bf1bfa0890e8>

在项目根目录下创建.gitignore 文件，然后输入要忽略提交的文件夹路径或文件

```
# Byte-compiled / optimized / DLL files
__pycache__/
*.py[cod]
*$py.class
.idea/

# C extensions
*.so

# Distribution / packaging
.Python
build/
develop-eggs/
dist/
downloads/
eggs/
.eggs/
lib/
lib64/
parts/
sdist/
var/
wheels/
*.egg-info/
.installed.cfg
*.egg
MANIFEST

# PyInstaller
#  Usually these files are written by a python script from a template
#  before PyInstaller builds the exe, so as to inject date/other infos into it.
*.manifest
*.spec

# Installer logs
pip-log.txt
pip-delete-this-directory.txt

# Unit test / coverage reports
htmlcov/
.tox/
.coverage
.coverage.*
.cache
nosetests.xml
coverage.xml
*.cover
.hypothesis/
.pytest_cache/

# Translations
*.mo
*.pot

# Django stuff:
*.log
local_settings.py
db.sqlite3

# Flask stuff:
instance/
.webassets-cache

# Scrapy stuff:
.scrapy

# Sphinx documentation
docs/_build/

# PyBuilder
target/

# Jupyter Notebook
.ipynb_checkpoints

# pyenv
.python-version

# celery beat schedule file
celerybeat-schedule

# SageMath parsed files
*.sage.py

# Environments
.env
.venv
env/
venv/
ENV/
env.bak/
venv.bak/

# Spyder project settings
.spyderproject
.spyproject

# Rope project settings
.ropeproject

# mkdocs documentation
/site

# mypy
.mypy_cache/

.vscode/

models/
data/
examples/*.png
examples/*.svg
*.h5
*.pb
```



#### github上传大文件

GitHub不允许直接上传大文件（超过100M）的文件到远程仓库，若要想继续提交可以尝试使用大文件支持库：[https://git-lfs.github.com](https://link.jianshu.com/?t=https%3A%2F%2Fgit-lfs.github.com)
LFS使用的简单步骤：

1. 根据官网指示安装git - lfs到本机

   ```
   curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
   ```

2. 安装Git命令行扩展。只需要设置一次Git LFS。
   在项目目录下，执行以下命令：

```undefined
sudo apt-get install git-lfs
git lfs install
```

3. 选择您希望Git LFS管理的文件类型（或直接编辑.gitattributes）。您可以随时配置其他文件扩展名。这一步成功后会生成一个gitattributes文件

```css
git lfs track “* .gif” --这里的 “ *.gif "就是你要上传的大文件的路径
```

4. 添加并commit gitattributes文件

```csharp
git add .gitattributes
```

5. 把.gitattributes文件push到远程分支 (一定要注意先把gitattributes文件先push到远程再添加大文件)

   ```
   git commit -m "add gitattributes"
   git push -u origin master
   ```

   

6. 最后再添加大文件到本地缓存区并提交

```csharp
git add demo.gif
git commit -m "提交项目演示gif图"
git push 
```



注意：免费容量只有1G

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



## 4. 多人合作

motivation: 当我们多人合作一个项目时，因为每个人的本地都会有一个项目，如果A在自己的本地做了修改并传至github，如果B也要修改的话，就要先把项目的最新版本从github上pull下来，然后在最新版本之上修改；但经常发生的情况是B不知道A做了修改，直接在自己的本地在老版本上修改，这样自己本地的版本与github版本就会发生冲突，一般需要手动合并冲突，比较麻烦。我们这里使用分支来确保每人的修改都是独立进行的，这样可以在自己的分支上进行任意修改，修改完成后只需要将自己的分支合并到主分支上就可以了，一般不会发生冲突。

其实分支更广泛的用途是自己基于主分支开发出和多个完全不同的版本，主分支相当于自己的一个原始备份，自己开发失败后可以推导重来且不影响主分支，同时还能保证自己能接收主分支的更新，最后可以把在自己分支的测试成功的功能合并到主分支上；

比如我们比赛的代码首先有了一个基础版本的功能，但需要添加更多的模块，每人负责一个模块，如果大家都在主分支上修改，这样就乱套了，在大家测试成功之前基础功能也用不了，所以需要每人创建一个分支，各自干各自的，自己测试成功了，再往主分支上合并，这样保证主分支永远都是安全的。

```python
## 1. 将想要参与的项目clone到本地
git clone git@github.com:USTC-LAB-411/Group-Meeting.git

## 2. 创建参与者分支
# 每个参与者独占一个分支，可以自由修改，不影响主分支文件
# 最后根据需求合并到主分支(master)
git branch ws
# 查看当前已经存在的分支
# 带*号的表示你当前处于的分支
git branch

## 3. 切换到自己的分支下，并进行相应文件修改
git checkout ws

## 4. 提交自己的修改
git add -u
git commit -m "ws test"
# 将自己的分支也push到github上，以便下次使用
git push orgin ws

## 5. 切换到master分支，并合并自己分支
git checkout master
git merge --no-ff -m "merge by ws" ws
# 推送到github
git push origin master

# 现在去github 项目页，可以看到branch下有了两个分支，且两个分支都是最新的内容；

## 6. 合并提交完成后及时切换回自己的分支，防止不小心在主分支上做改动
git branch ws

## 7.1 再次在自己的分支上进行文件修改操作
git add 
git commit 
git push origin ws
## 7.2 此时whz也在自己的分支上进行了修改操作，并推送到了主分支

## 8. 拉取最新的主分支，合并自己新的修改并推送
git checkout master
git pull # 更新了whz的修改
git merege git merge --no-ff -m "merge test" ws
git push orgin master

## 返回步骤6，重复操作
```

### 解决冲突

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
