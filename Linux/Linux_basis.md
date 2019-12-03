### 环境变量

对于这里的“变量”二字，不需要有特殊的理解，就可以理解为编程时用到的变量，通过printenv, 我们可以看到很多定义好的变量

```
PWD=/home/ubuntu16
USER=ubuntu16
ROS_MASTER_URI=http://localhost:11311
PATH=/opt/ros/kinetic/bin:/usr/local/cuda-9.0/bin:/home/ubuntu16/bin:/home/ubuntu16/.local/bin:/usr/local/cuda-9.0/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

```

打开终端，使用 echo $PWD 便能看到这些变量的值，当然你在编程时也可以随意使用这些定义好的变量，而不需要每次定义

#### 什么是环境变量

bash shell用一个叫作`环境变量(environment variable)`的特性来存储有关shell会话和工作环境的信息(这也是它们被称作环境变量的原因)。
 这项特性允许你在内存中存储数据，以便程序或shell中运行的脚本能够轻松访问到它们。这也是存储持久数据的一种方法。

Linux是一个多用户多任务的操作系统，可以在Linux中为不同的用户设置不同的运行环境，具体做法是设置不同用户的环境变量。

#### Linux环境变量分类

一、按照生命周期来分，Linux环境变量可以分为两类：
 1、永久的：需要用户修改相关的配置文件，变量永久生效。
 2、临时的：用户利用export命令，**在当前终端下声明环境变量，关闭Shell终端失效。**

二、按照作用域来分，Linux环境变量可以分为：
 1、系统环境变量：系统环境变量对该系统中所有用户都有效。
 2、用户环境变量：顾名思义，这种类型的环境变量只对特定的用户有效。

#### Linux设置环境变量的方法

一、在`/etc/profile`文件中添加变量 **对所有用户生效（永久的）**
 用vim在文件`/etc/profile`文件中增加变量，该变量将会对Linux下所有用户有效，并且是“永久的”。
 例如：编辑/etc/profile文件，添加CLASSPATH变量

```
  vim /etc/profile    
  export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib

```

**注：修改文件后要想马上生效还要运行`source /etc/profile`不然只能在下次重进此用户时生效。**

**思考？** ROS 工作空间每次使用前都要source 一下的原因就是系统不会自动source其setup.bash文件，将source命令写入系统能自动执行的文件，便达到了自动执行相关设置的效果



 二、在用户目录下的.bash_profile文件中增加变量 **【对单一用户生效（永久的）】**
 用`vim ~/.bash_profile`文件中增加变量，改变量仅会对当前用户有效，**并且是“永久的”。**

```
vim ~/.bash.profile
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib

```

注：修改文件后要想马上生效还要运行$ source ~/.bash_profile不然只能在下次重进此用户时生效。

修改`.bashrc`文件

这种方法更为安全，它可以把使用这些环境变量的权限控制到用户级别,这里是针对某一个特定的用户，如果需要给某个用户权限使用这些环境变量，只需要修改其个人用户主目录下的.bashrc文件就可以了。

```
root@iZbp1e036pdvwobr4d5fr2Z:~#  vim /root/.bashrc
export PATH=$PATH:/usr/local/mysql/bin
root@iZbp1e036pdvwobr4d5fr2Z:~# source /root/.bashrc

#单个用户添加环境变量
ray@iZbp1e036pdvwobr4d5fr2Z:~$ vim ~/.bashrc 
export PATH=$PATH:/usr/local/mysql/bin
ray@iZbp1e036pdvwobr4d5fr2Z:~$ source ~/.bashrc

```

 三、直接运行export命令定义变量 **【只对当前shell（BASH）有效（临时的）】**
 在shell的命令行下直接使用`export 变量名=变量值`
 **定义变量，该变量只在当前的shell（BASH）或其子shell（BASH）下是有效的，shell关闭了，变量也就失效了，再打开新shell时就没有这个变量，需要使用的话还需要重新定义。**



#### 全局环境变量

1. 全局环境变量对于shell会话和所有生成的子shell都是可见的。
2. 局部变量则只对创建它们的shell可见。
3. Linux系统在你开始shell会话时就设置了一些全局环境变量。
4. 系统环境变量基本上都是使用全大写字母，以区别于普通用户的环境变量。
5. 要查看全局变量，可以使用`env`或`printenv`命令。
   `$ printenv`
6. 其中有很多是在登录过程中设置的。你的登录方式也会影响到所设置的环境变量。
7. 要显式个别环境变量的值，可以使用`printenv`命令，但是不要使用`env`命令。
   `$ printenv HOME`
   也可以使用`echo`显示变量的值。在这种情况下引用某个环境变量的时候，必须在变量前面加上一个美元符($)。

   `$ echo $HOME`

   /home/ubuntu16

```
在echo命令中，在变量名前加上`$`不仅仅是要显示变量当前的值。它能够让变量作为命令行参数。
`$ ls $HOME`

```

#### 局部环境变量

1. 局部环境变量如其名只能在定义它们的进程中可见。
2. 和全局环境变量一样重要。
3. Linux系统也默认定义了标准的局部环境变量。
4. 自己也可以定义自己的局部变量，这些被称为用户定义局部变量。
5. 在linux系统并没有一个只显示局部环境变量的命令。`set`命令会显示为某个特定进程设置的所有环境变量，包括局部变量、全局变量以及用户定义变量，并会按照字母顺序对结果进行排序。
   `$ set` 

#### 设置用户定义变量

设置局部用户定义变量

一单启动了bash shell(或者执行一个shell脚本)，就能创建在这个shell进程内可见的局部变量了。
 可以通过等号给环境变量赋值，值可以是数值或字符串。

```
$ my_variable=Hello
$ echo $my_variable
Hello

```

现在每次引用my_variable环境变量的值，只要通过my_variable引用即可。
 如果要给环境变量赋一个含有空格的字符串值，必须用单引号来界定字符串的首和尾。

```
$ my_variable='Hello World'
$ echo my_variable
Hello World
## 现在打开另一个终端，执行 echo my_variable 就不会有任何输出

```

在涉及用户定义的局部变量时坚持使用小写字母，这能够避免重新定义系统环境变量可能带来的灾难。

变量名、等号和值之间没有空格。如果在赋值表达式中加上了空格，bash shell就会把值当成一个单独的命令：

```
$ my_variable = 'Hello World'
-bash: my_variable: command not found

```

#### 设置全局环境变量 

方法：先创建一个局部环境变量，然后再把它导出到全局环境中。
 **这个过程通过`export`命令来完成**，变量名前面`不`需要加`$`。

```
$ my_variable='I am Global now'
$ export my_variable
$ echo $my_variable
I am Global now
## 打开另一个shell echo也不会有输出，这些都是临时的变量
printenv | grep my_variable
#可以发现 my_variable 已经变成了全局变量

```

修改子shell中全局环境变量并不会影响到父shell中该变量的值。
 即：在父shell中定义并导出变量my_variable后，使用bash命令启动一个子shell。在这个子shell中修改了这个变量的值，但是这种改变仅在子shell中有效，并不会被反映到父shell中。子shell甚至无法使用export命令改变父shell中全局环境变量的值。

## 删除环境变量

可以用`unset`命令完成这个操作。在`unset`命令中引用环境变量时，`不`要使用`$`。
 `unset my_variable`

------

**注意：如果要`用到`变量，使用`$`；如果要`操作`变量，`不`使用`$`。**这条规则的一个例外是使用`printenv`显示某个变量的值。
 涉及全局环境变量时，同样在子进程中删除一个全局环境变量，只对子进程有效，该全局环境变量在父进程中依然有效。

------

## 默认的shell环境变量

尽管有很多默认环境变量，但并不是每一个都必须有一个值。

## 设置PATH环境变量

1. 当你在shell命令行界面中输入一个外部命令时，shell必须搜索系统来找到对应的程序。

   **PATH环境变量定义了：用于进行命令和程序查找的目录。**

```
$ echo $PATH
/usr/lib64/qt-3.3/bin:/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/sbin:/home/lsh/bin

```

1. PATH中的目录使用冒号分隔。
2. 如果命令或者程序的位置没有包括在`PATH`变量中，**那么如果不使用绝对路径的话，shell是没法找到的。如果shell找不到指定的命令或程序，它会产生`command not found`的错误信息。**

**问题是**：应用程序放置可执行文件的目录常常不在PATH环境变量所包含的目录中。

**解决的办法是**：保证PATH环境变量包含了所有存放应用程序的目录。

可以把新的搜索目录添加到现有的PATH环境变量中，无需从头定义。你只需引用原来的PATH值，然后再给这个字符串添加新目录就行了。如果希望子shell也能找到你的程序的位置，`一定`要把修改后的PATH环境变量`导出`。

**关于PATH的作用：**
PATH说简单点就是一个字符串变量，当输入命令的时候LINUX会去查找PATH里面记录的路径。比如在根目录/下可以输入命令ls,在/usr目录下也可以输入ls,但其实ls这个命令根本不在这个两个目录下，事实上当你输入命令的时候LINUX会去/bin,/usr/bin,/sbin等目录下面去找你此时输入的命令，而PATH的值恰恰就是/bin:/sbin:/usr/bin:……。

其中的冒号使目录与目录之间隔开。

**关于新增自定义路径：**
现在假设你新安装了一个命令在/usr/locar/new/bin下面，而你又想像ls一样在任何地方都使用这个命令，你就需要修改环境变量PATH了，准确的说就是给PATH增加一个值/usr/locar/new/bin。你只需要一行bash命令export PATH=$PATH:/usr/locar/new/bin。这条命令的意思太清楚不过了，使PATH自增:/usr/locar/new/bin,既PATH=PATH+":/usr/locar/new/bin";**通常的做法是把这行bash命令写到/root/.bashrc的末尾，然后当你重新登陆LINUX的时候（应该是linux启动时就会执行这个文件），新的默认路径就添加进去了**。当然这里你直接用source /root/.bashrc执行这个文件重新登陆了。你可以用

echo $PATH命令查看PATH的值。



## 定位系统环境变量

在你登入Linux系统启动一个bash shell时，默认情况下bash会在几个文件中查找命令。这些文件叫作`启动文件`或`环境文件`。bash检查的启动文件取决于你启动bash shell的方式。
 启动bash shell有3种方式：

- 登录时作为默认登录shell
- 作为非登录shell的交互式shell
- 作为运行脚本的非交互shell

#### 登录shell

登录shell会从`5个`不同的启动文件里读取命令：

```
/etc/profile
$HOME/.bash_profile
$HOME/.bashrc
$HOME/.bash_login
$HOME/.profile

```

`/etc/profile`文件是系统上默认的bash shell的主启动文件。系统上每个用户登录时都会执行这个启动文件。

------

另外4个启动文件是针对用户的，可根据个人需求定制。

#### 1./etc/profile文件

是bash shell默认的主启动文件。
 **只要登录了Linux系统，**bash就会执行`/etc/profile`启动文件中的命令。
 不同的Linux发行版在这个文件里放了不同的命令。

Ubuntu发行版和CentOS发行版的/etc/profile文件都用到了同一个特性：for语句。它用来迭代`/etc/profile.d`目录下的所有文件。这为Linux系统提供了一个放置特定应用程序启动文件的地方，当用户登录时，shell会执行这些文件。

#### 2.$HOME目录下的启动文件

剩下的启动文件都起着同一个作用：提供一个用户专属的启动文件来定义该用户所用到的环境变量。大多数Linux发行版只用这四个启动文件中的一到两个：

```
$HOME/.bash_profile
$HOME/.bashrc
$HOME/.bash_login
$HOME/.profile

```

这些启动文件中的环境变量会在**每次启动bash shell会话时生效**。

------

Linux发行版在环境文件方面存在的差异非常大。这里所列出的`$HOME`下的那些文件并非每个用户都有。例如有些用户可能只有一个`$HOME/.bash_profile`文件。这很正常。

------

shell会按照下列顺序，运行`第一个`被找到的文件，余下的则被忽略：

```
$HOME/.bash_profile
$HOME/.bash_login
$HOME/.profile

```

注意，这个列表中并没有`$HOME/.bashrc`文件。这是因为该文件通常通过其他文件运行的。

## 交互式shell进程

如果你的bash shell不是登录系统时启动的，那么你启动的shell叫做交互式shell。
 如果bash是作为交互式shell启动的，它就不会访问`/etc/profile`文件，只会检查用户`HOME目录`中的`.bashrc`文件。
 `.bashrc`文件有`2个`作用：一是查看`/etc`目录下通用的`bashrc`文件，二是为用户提供一个定制自己的命令别名和私有脚本函数的地方。

## 非交互式shell

系统执行shell脚本时用的就是这种shell。它没有命令行提示符。
 但是当你在系统上运行脚本时，也许希望能够运行一些特定启动的命令。为了处理这种情况，bash shell提供了`BASH_ENV`环境变量。当shell启动了一个非交互式shell进程时，它会检查这个环境变量来查看要执行的启动文件。如果有指定的文件，shell会执行该文件里的命令，这通常也包括shell脚本变量设置。

在CentOS和Ubuntu发行版中，这个环境变量在默认情况下并未设置。如果环境变量未设置，`printenv命令`只会返回CLI提示符：

```
$ printenv BASH_ENV

```

而`echo命令`会显示一个空行，然后返回CLI提示符：

```
$ echo $BASH_ENV

```

尽管没有设置BASH_ENV变量，但是子shell可以继承父shell导出过的变量。
 对于那些不启动子shell的脚本，变量已经存在于当前shell中了。所以就算没有设置`BASH_ENV`，也可以使用当前shell的局部变量和全局变量。

## 环境变量持久化

对于全局环境变量来说，可能更倾向于将新的或修改过的变量设置放在`/etc/profile`文件中，`但这可不是好主意。如果升级了所用发行版，这个文件也会跟着更新，那你所有定制过的变量设置就都没了`。
 最好是在`/etc/profile.d`目录中创建一个以`.sh`结尾的文件。把所有新的或修改过的全局环境变量设置放在这个文件中。
 在大多数发行版中，**存储个人用户永久性bash shell变量的地方是`$HOME/.bashrc`文件。**这一点适用于所有类型的shell进程。但如果设置了`BASH_ENV`变量，那么除非它指向的是`$HOME/.bashrc`，否则你应该讲非交互式shell的用户变量放在别的地方。

------

图形化界面的组成部分的环境变量可能需要在另外一些配置文件中设置，这和设置bash shell环境变量的地方不一样。

------

可以把自己的alias设置放在`$HOME/.bashrc`启动文件中，使其效果永久化。



## 获取当前脚本绝对路径

```
DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"
```

详见：https://blog.csdn.net/davidhopper/article/details/78989369 

BASH_EOURCE和BASH_SOURCE[0]的作用都是一样的，就是取得当前执行的shell脚本的相对路径

如果希望获得，当前执行脚本的绝对路径，可以采用以下方式：

```
DIR_T="$( cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

  1  #!/bin/bash
  2 
  3  echo $0 $1 $2
  4 
  5  echo "BASH_SOURCE = ${BASH_SOURCE}"
  6  echo "BASH_SOURCE[0] = ${BASH_SOURCE[0]}"
  7  echo "dirname BASH_SOURCE[0] = $(dirname "${BASH_SOURCE[0]}")"
  8  DIR_T="$( cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
  9  echo "DIR_T=$DIR_T"
```

执行结果：

```
./build_test.sh kernel
BASH_SOURCE = ./build_test.sh
BASH_SOURCE[0] = ./build_test.sh
dirname BASH_SOURCE[0] = .
DIR_T=/home/highgreat-xyw2018/Documents/share-2018xyw/rk3288_linux
./build_test.sh

/home/highgreat-xyw2018/Documents/share-2018xyw/rk3288_linux
```


原文：https://blog.csdn.net/u010299133/article/details/85048431 