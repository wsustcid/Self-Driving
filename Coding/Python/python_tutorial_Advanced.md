













<p align="center"> <font size=7> Python Advantaced Tutorial </font> </p>







<p align="center"> Shuai Wang </p>

<p align="center">USTC, April 11, 2019 </p>















<p align="center"> <font color=blue> Copyright (c) 2019 Shuai Wang. All rights reserved. </font> </p>

<p align="center"> <i>Shuai Wang copyrights this specification. No part of this specification may be reproduced in any form or means, without the prior written consent of Shuai Wang. </i></p>










# 3. Advanced Tutorial

## 类

self

https://www.jianshu.com/p/bdbd577314f9



每次调用内部的方法时，方法前面加 self.



举例：

```python
class MyClass:
    def __init__(self):
        pass
    def func1(self):
        # do something
        print('a')   #for example      
        self.common_func()
     def func2(self):
        # do something
        self.common_func()
         
     def common_func(self):
         pass
```



- 多线程

问题：
 1、Python 多线程为什么耗时更长？
 2、为什么在 Python 里面推荐使用多进程而不是多线程？

### 1 基础知识

现在的 PC 都是多核的，使用多线程能充分利用 CPU 来提供程序的执行效率。

#### 1.1 线程

线程是一个基本的 CPU 执行单元。它必须依托于进程存活。一个线程是一个**execution context（执行上下文）**，即一个 CPU 执行时所需要的一串指令。

#### 1.2 进程

进程是指一个程序在给定数据集合上的一次执行过程，是**系统进行资源分配和运行调用的独立单位**。可以简单地理解为操作系统中正在执行的程序。也就说，每个应用程序都有一个自己的进程。

每一个进程启动时都会最先产生一个线程，即主线程。然后主线程会再创建其他的子线程。

#### 1.3 两者的区别

- 线程必须在某个进程中执行。
- 一个进程可包含多个线程，其中有且只有一个主线程。
- 多线程共享同个地址空间、打开的文件以及其他资源。
- 多进程共享物理内存、磁盘、打印机以及其他资源。

#### 1.4 线程的类型

线程的因作用可以划分为不同的类型。大致可分为：

- 主线程
- 子线程
- 守护线程（后台线程）
- 前台线程

### 2 Python 多线程

#### 2.1 GIL

其他语言，CPU 是多核时是支持多个线程同时执行。但在 Python 中，无论是单核还是多核，同时只能由一个线程在执行。其根源是 GIL 的存在。

GIL 的全称是 Global Interpreter Lock(全局解释器锁)，来源是 Python 设计之初的考虑，为了数据安全所做的决定。某个线程想要执行，必须先拿到 GIL，我们可以把 GIL 看作是“通行证”，并且在一个 Python 进程中，GIL 只有一个。拿不到通行证的线程，就不允许进入 CPU 执行。

而目前 Python 的解释器有多种，例如：

- **CPython**：CPython 是用C语言实现的 Python 解释器。 作为官方实现，它是最广泛使用的 Python 解释器。
- **PyPy**：PyPy 是用RPython实现的解释器。RPython 是 Python 的子集， 具有静态类型。这个解释器的特点是即时编译，支持多重后端（C, CLI, JVM）。PyPy 旨在提高性能，同时保持最大兼容性（参考 CPython 的实现）。
- **Jython**：Jython 是一个将 Python 代码编译成 Java 字节码的实现，运行在JVM (Java Virtual Machine) 上。另外，它可以像是用 Python 模块一样，导入 并使用任何Java类。
- **IronPython**：IronPython 是一个针对 .NET 框架的 Python 实现。它 可以用 Python 和 .NET framewor k的库，也能将 Python 代码暴露给 .NET 框架中的其他语言。

GIL 只在 CPython 中才有，而在 PyPy 和 Jython 中是没有 GIL 的。

每次释放 GIL锁，线程进行锁竞争、切换线程，会消耗资源。这就导致打印线程执行时长，会发现耗时更长的原因。

并且由于 GIL 锁存在，Python 里一个进程永远只能同时执行一个线程(拿到 GIL 的线程才能执行)，这就是为什么在多核CPU上，Python 的多线程效率并不高的根本原因。

#### 2.2 创建多线程

Python提供两个模块进行多线程的操作，分别是`thread`和`threading`， 前者是比较低级的模块，用于更底层的操作，一般应用级别的开发不常用。

- 方法1：直接使用`threading.Thread()` 

```python
import threading

# 这个函数名可随便定义
def run(n):
    print("current task：", n)

if __name__ == "__main__":
    t1 = threading.Thread(target=run, args=("thread 1",))
    t2 = threading.Thread(target=run, args=("thread 2",))
    t1.start()
    t2.start()
```

- 方法2：继承`threading.Thread`来自定义线程类，重写`run`方法

```python
import threading

class MyThread(threading.Thread):
    def __init__(self, n):
        super(MyThread, self).__init__()  # 重构run函数必须要写
        self.n = n

    def run(self):
        print("current task：", n)

if __name__ == "__main__":
    t1 = MyThread("thread 1")
    t2 = MyThread("thread 2")

    t1.start()
    t2.start()
```

#### 2.3 线程合并

`Join`函数执行顺序是逐个执行每个线程，执行完毕后继续往下执行。主线程结束后，子线程还在运行，`join`函数使得主线程等到子线程结束时才退出。

```python
import threading

def count(n):
    while n > 0:
        n -= 1

if __name__ == "__main__":
    t1 = threading.Thread(target=count, args=("100000",))
    t2 = threading.Thread(target=count, args=("100000",))
    t1.start()
    t2.start()
    # 将 t1 和 t2 加入到主线程中
    t1.join()
    t2.join()
```

#### 2.4 线程同步与互斥锁

线程之间数据共享的。当多个线程对某一个共享数据进行操作时，就需要考虑到线程安全问题。`threading`模块中定义了Lock 类，提供了互斥锁的功能来保证多线程情况下数据的正确性。

用法的基本步骤：

```python
#创建锁
mutex = threading.Lock()
#锁定
mutex.acquire([timeout])
#释放
mutex.release()
```

其中，锁定方法acquire可以有一个超时时间的可选参数timeout。如果设定了timeout，则在超时后通过返回值可以判断是否得到了锁，从而可以进行一些其他的处理。

acquire 锁定某个线程后，即使这个线程耗时非常久，后面的线程也必须等待其把这个线程执行完毕后才会继续执行（特别是对数据的操作完成后），这样就保证了对数据的顺序操作：

具体用法见示例代码：

```python
#!/usr/bin/env python

import threading
import time

num = 0
delay = 5
mutex = threading.Lock()

class MyThread(threading.Thread):
    def run(self):
        global num 
        global delay
        

        if mutex.acquire(1):  
            num = num + 1
            print("now the num is:", num)
            time.sleep(delay)
            msg = self.name + ': num value is ' + str(num)
            print(msg)
            mutex.release()

if __name__ == '__main__':
    for i in range(5):
        delay = 10-2*i
        t = MyThread()
        t.start()
        
  
```

否则，不锁定每个线程，如果第一个线程耗时太长，第二个线程已经修改了数据，那个第一个线程使用的数据就是第二个线程修改过的，而不是第一个线程开始执行时候的期望数值，导致数据顺序错乱。

```Python
import threading
import time

num = 0
delay = 5
mutex = threading.Lock()

class MyThread(threading.Thread):
    def run(self):
        global num 
        global delay
        
        num = num + 1
        print("now the num is: %d", num)
        time.sleep(delay)
        msg = self.name + ': num value is ' + str(num)
        print(msg)

if __name__ == '__main__':
    for i in range(5):
        delay = 10-2*i # 每个线程延时越来越短，导致后面的线程会先执行
        t = MyThread()
        t.start()
        
        
        
 ## ====output====
('now the num is:', 1)
('now the num is:', 2)
('now the num is:', 3)
('now the num is:', 4)
('now the num is:', 5)
Thread-5: num value is 5
Thread-4: num value is 5
Thread-3: num value is 5
Thread-2: num value is 5
Thread-1: num value is 5
```



#### 2.5 可重入锁（递归锁）

为了满足在同一线程中多次请求同一资源的需求，Python 提供了可重入锁（RLock）。
 `RLock`内部维护着一个`Lock`和一个`counter`变量，counter 记录了 acquire 的次数，从而使得资源可以被多次 require。直到一个线程所有的 acquire 都被 release，其他的线程才能获得资源。

具体用法如下：

```python
#创建 RLock
mutex = threading.RLock()

class MyThread(threading.Thread):
    def run(self):
        if mutex.acquire(1):
            print("thread " + self.name + " get mutex")
            time.sleep(1)
            mutex.acquire()
            mutex.release()
            mutex.release()
```

#### 2.6 守护线程

如果希望主线程执行完毕之后，不管子线程是否执行完毕都随着主线程一起结束。我们可以使用`setDaemon(bool)`函数，它跟`join`函数是相反的。它的作用是设置子线程是否随主线程一起结束，必须在`start()` 之前调用，默认为`False`。

#### 2.7 定时器

如果需要规定函数在多少秒后执行某个操作，需要用到`Timer`类。具体用法如下：

```python
from threading import Timer
 
def show():
    print("Pyhton")

# 指定一秒钟之后执行 show 函数
t = Timer(1, show)
t.start()  
```

### 3 Python 多进程

#### 3.1 创建多进程

Python 要进行多进程操作，需要用到`muiltprocessing`库，其中的`Process`类跟`threading`模块的`Thread`类很相似。所以直接看代码熟悉多进程。

- 方法1：直接使用`Process`, 代码如下：

```
from multiprocessing import Process  

def show(name):
    print("Process name is " + name)

if __name__ == "__main__": 
    proc = Process(target=show, args=('subprocess',))  
    proc.start()  
    proc.join()
```

- 方法2：继承`Process`来自定义进程类，重写`run`方法, 代码如下：

```
from multiprocessing import Process
import time

class MyProcess(Process):
    def __init__(self, name):
        super(MyProcess, self).__init__()
        self.name = name

    def run(self):
        print('process name :' + str(self.name))
        time.sleep(1)

if __name__ == '__main__':
    for i in range(3):
        p = MyProcess(i)
        p.start()
    for i in range(3):
        p.join()
```

#### 3.2 多进程通信

进程之间不共享数据的。如果进程之间需要进行通信，则要用到`Queue模块`或者`Pipi模块`来实现。

- **Queue**

Queue 是多进程安全的队列，可以实现多进程之间的数据传递。它主要有两个函数,`put`和`get`。

put() 用以插入数据到队列中，put 还有两个可选参数：blocked 和 timeout。如果 blocked 为 True（默认值），并且 timeout 为正值，该方法会阻塞 timeout 指定的时间，直到该队列有剩余的空间。如果超时，会抛出 Queue.Full 异常。如果 blocked 为 False，但该 Queue 已满，会立即抛出 Queue.Full 异常。

get()可以从队列读取并且删除一个元素。同样，get 有两个可选参数：blocked 和 timeout。如果 blocked 为 True（默认值），并且 timeout 为正值，那么在等待时间内没有取到任何元素，会抛出 Queue.Empty 异常。如果blocked 为 False，有两种情况存在，如果 Queue 有一个值可用，则立即返回该值，否则，如果队列为空，则立即抛出 Queue.Empty 异常。

具体用法如下：

```
from multiprocessing import Process, Queue
 
def put(queue):
    queue.put('Queue 用法')
 
if __name__ == '__main__':
    queue = Queue()
    pro = Process(target=put, args=(queue,))
    pro.start()
    print(queue.get())   
    pro.join()
```

- **Pipe**

Pipe的本质是进程之间的用管道数据传递，而不是数据共享，这和socket有点像。pipe() 返回两个连接对象分别表示管道的两端，每端都有send() 和recv()函数。

如果两个进程试图在同一时间的同一端进行读取和写入那么，这可能会损坏管道中的数据。

具体用法如下：

```
from multiprocessing import Process, Pipe
 
def show(conn):
    conn.send('Pipe 用法')
    conn.close()
 
if __name__ == '__main__':
    parent_conn, child_conn = Pipe() 
    pro = Process(target=show, args=(child_conn,))
    pro.start()
    print(parent_conn.recv())   
    pro.join()
```

#### 3.3 进程池

创建多个进程，我们不用傻傻地一个个去创建。我们可以使用`Pool`模块来搞定。

Pool 常用的方法如下：

| 方法          |                             含义                             |
| :------------ | :----------------------------------------------------------: |
| apply()       |                       同步执行（串行）                       |
| apply_async() |                       异步执行（并行）                       |
| terminate()   |                        立刻关闭进程池                        |
| join()        | 主进程等待所有子进程执行完毕。必须在close或terminate()之后使用 |
| close()       |               等待所有进程结束后，才关闭进程池               |

具体用法见示例代码：

```
from multiprocessing import Pool
def show(num):
    print('num : ' + str(num))

if __name__=="__main__":
    pool = Pool(processes = 3)
    for i in xrange(6):
        # 维持执行的进程总数为processes，当一个进程执行完毕后会添加新的进程进去
        pool.apply_async(show, args=(i, ))       
    print('======  apply_async  ======')
    pool.close()
    #调用join之前，先调用close函数，否则会出错。执行完close后不会有新的进程加入到pool,join函数等待所有子进程结束
    pool.join()
```

#### 3.4 选择多线程还是多进程？

在这个问题上，首先要看下你的程序是属于哪种类型的。一般分为两种 CPU 密集型 和 I/O 密集型。

- CPU 密集型：程序比较偏重于计算，需要经常使用 CPU 来运算。例如科学计算的程序，机器学习的程序等。
- I/O 密集型：顾名思义就是程序需要频繁进行输入输出操作。爬虫程序就是典型的 I/O 密集型程序。

如果程序是属于 CPU 密集型，建议使用多进程。而多线程就更适合应用于 I/O 密集型程序。

作者：猴哥Yuri

链接：https://www.jianshu.com/p/a69dec87e646

来源：简书

简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。



#### 执行终端/控制台命令

```python
import os
import pexpect

## python 执行终端/控制台命令并返回
os.system('ping www.baidu.com') #执行成功 返回 0 
#注：os.system() 执行完成 会关闭 所以当执行后续命令需要依赖前面的命令时，请将多条命令写到一个 os.system() 内

ping = os.popen('pint www.baidu.com').read().strip()  #返回输出结果


## 执行终端命令并交互
ch = pepect.spwn('命令')
ch.expect('Password:')
ch.sendline('密码')

```

## 检测键盘输入

https://stackoverflow.com/questions/2408560/python-nonblocking-console-input

```python
#!/usr/bin/env python
'''
A Python class implementing KBHIT, the standard keyboard-interrupt poller.
Works transparently on Windows and Posix (Linux, Mac OS X).  Doesn't work
with IDLE.

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Lesser General Public License as 
published by the Free Software Foundation, either version 3 of the 
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

'''

import os

# Windows
if os.name == 'nt':
    import msvcrt

# Posix (Linux, OS X)
else:
    import sys
    import termios
    import atexit
    from select import select


class KBHit:

    def __init__(self):
        '''Creates a KBHit object that you can call to do various keyboard things.
        '''

        if os.name == 'nt':
            pass

        else:

            # Save the terminal settings
            self.fd = sys.stdin.fileno()
            self.new_term = termios.tcgetattr(self.fd)
            self.old_term = termios.tcgetattr(self.fd)

            # New terminal setting unbuffered
            self.new_term[3] = (self.new_term[3] & ~termios.ICANON & ~termios.ECHO)
            termios.tcsetattr(self.fd, termios.TCSAFLUSH, self.new_term)

            # Support normal-terminal reset at exit
            atexit.register(self.set_normal_term)


    def set_normal_term(self):
        ''' Resets to normal terminal.  On Windows this is a no-op.
        '''

        if os.name == 'nt':
            pass

        else:
            termios.tcsetattr(self.fd, termios.TCSAFLUSH, self.old_term)


    def getch(self):
        ''' Returns a keyboard character after kbhit() has been called.
            Should not be called in the same program as getarrow().
        '''

        s = ''

        if os.name == 'nt':
            return msvcrt.getch().decode('utf-8')

        else:
            return sys.stdin.read(1)


    def getarrow(self):
        ''' Returns an arrow-key code after kbhit() has been called. Codes are
        0 : up
        1 : right
        2 : down
        3 : left
        Should not be called in the same program as getch().
        '''

        if os.name == 'nt':
            msvcrt.getch() # skip 0xE0
            c = msvcrt.getch()
            vals = [72, 77, 80, 75]

        else:
            c = sys.stdin.read(3)[2]
            vals = [65, 67, 66, 68]

        return vals.index(ord(c.decode('utf-8')))


    def kbhit(self):
        ''' Returns True if keyboard character was hit, False otherwise.
        '''
        if os.name == 'nt':
            return msvcrt.kbhit()

        else:
            dr,dw,de = select([sys.stdin], [], [], 0)
            return dr != []


# Test    
if __name__ == "__main__":
    # 加上下面这句，将始终输出hhh，即使你一直按着某个键
    # 可能的原因是kb.kbhit() 键盘输入的刷新频率过慢，而while true则超级快
    # 导致返回值绝大大部分情况都为false，即没有按键按下
    # 解决方案是加入延时，降低while频率

    kb = KBHit()

    print('Hit any key, or ESC to exit')

    while True:

        if kb.kbhit():
            c = kb.getch()
            if ord(c) == 27: # ESC
                break
            print(c)
        else:
            print('hhh')

    kb.set_normal_term()
```



#### 根据键盘输入执行对应任务

There are some sample code about how to remote control [CamJam’s fabulous Robot Kit](http://camjam.me/?page_id=1035) including a piece of code that would allow you to [control a robot with a bluetooth keyboard](https://github.com/recantha/EduKit3-RC-Keyboard) or the like.  Following the example provided below, it should be quite easy to adjust this for whichever keyboard presses you need to detect and then add in the functions you want to trigger from each key press!

```python
#!/usr/bin/python3
 
# adapted from https://github.com/recantha/EduKit3-RC-Keyboard/blob/master/rc_keyboard.py
 
import sys, termios, tty, os, time
 
def getch():
    # 获取标准输入的描述符
    fd = sys.stdin.fileno()
    # 获取标准输入(终端)的设置
    old_settings = termios.tcgetattr(fd)
    try:
        tty.setraw(sys.stdin.fileno())
        ch = sys.stdin.read(1)
 
    finally:
        # 使设置生效
        termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
    return ch
 
button_delay = 0.2
 
while True:
    char = getch()
 
    if (char == "p"):
        print("Stop!")
        exit(0)
 
    if (char == "a"):
        print("Left pressed")
        time.sleep(button_delay)
 
    elif (char == "d"):
        print("Right pressed")
        time.sleep(button_delay)
 
    elif (char == "w"):
        print("Up pressed")
        time.sleep(button_delay)
 
    elif (char == "s"):
        print("Down pressed")
        time.sleep(button_delay)
 
    elif (char == "1"):
        print("Number 1 pressed")
        time.sleep(button_delay)
```

**Note:** It seems to stop in the getch function if no key is pressed which pauses the game. There should be an output if no key is pressed, or a break rather than waiting for a key. 

This is because sys.stdin.read() blocks. One option for non-blocking input (at least on Linux) is as follows. 

```python
#!/usr/bin/python3

import sys, termios, tty, os, time
# added
import fcntl
 
def getch():
    fd = sys.stdin.fileno()
    old_settings = termios.tcgetattr(fd)
    try:
        tty.setraw(sys.stdin.fileno())
        ch = sys.stdin.read(1)
 
    finally:
        termios.tcsetattr(fd, termios.TCSADRAIN, old_settings)
    return ch
 
button_delay = 0.2

# added
fd = sys.stdin.fileno()
fl = fcntl.fcntl(fd, fcntl.F_GETFL)
fcntl.fcntl(fd, fcntl.F_SETFL, fl | os.O_NONBLOCK)
 
while True:
    char = getch()
 
    if (char == "p"):
        print("Stop!")
        exit(0)
    # added
    elif (not char):
        print("No char")
    elif (char == "a"):
        print("Left pressed")
        time.sleep(button_delay)
 
    elif (char == "d"):
        print("Right pressed")
        time.sleep(button_delay)
 
    elif (char == "w"):
        print("Up pressed")
        time.sleep(button_delay)
 
    elif (char == "s"):
        print("Down pressed")
        time.sleep(button_delay)
 
    elif (char == "1"):
        print("Number 1 pressed")
        time.sleep(button_delay)
        
```

处理　箭头：

https://stackoverflow.com/questions/22397289/finding-the-values-of-the-arrow-keys-in-python-why-are-they-triples



https://www.raspberrypi.org/forums/viewtopic.php?t=42608

```python

import curses

# get the curses screen window
screen = curses.initscr()

# turn off input echoing
curses.noecho()

# respond to keys immediately (don't wait for enter)
curses.cbreak()

# map arrow keys to special values
screen.keypad(True)

if __name__ == "__main__":
    try:
        while True:
            char = screen.getch()
            if char == ord('q'): 
                break
            elif char == curses.KEY_RIGHT:
                # print doesn't work with curses, use addstr instead
                print("right")
                screen.addstr(0, 0, 'right')
            elif char == curses.KEY_LEFT:
                screen.addstr(0, 0, 'left ')
                print("left")        
            elif char == curses.KEY_UP:
                screen.addstr(0, 0, 'up   ')
                print("up")        
            elif char == curses.KEY_DOWN:
                screen.addstr(0, 0, 'down ')
                print("down")
    finally:
        # shut down cleanly
        curses.nocbreak(); screen.keypad(0); curses.echo()
        curses.endwin()
```

#### 按任意键退出

初学Python时在总想实现一个按任意键继续/退出的程序(受.bat毒害), 奈何一直写不出来, 最近学习Unix C时发现可以通过`termios.h`库来实现, 尝试一下发现Python也有这个库, 所以终于写出一个这样的程序. 下面是代码:

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-
import os
import sys
import termios


def press_any_key_exit(msg):
    # 获取标准输入的描述符
    fd = sys.stdin.fileno()

    # 获取标准输入(终端)的设置
    old_ttyinfo = termios.tcgetattr(fd)

    # 配置终端
    new_ttyinfo = old_ttyinfo[:]

    # 使用非规范模式(索引3是c_lflag 也就是本地模式)
    new_ttyinfo[3] &= ~termios.ICANON
    # 关闭回显(输入不会被显示)
    new_ttyinfo[3] &= ~termios.ECHO

    # 输出信息
    sys.stdout.write(msg)
    sys.stdout.flush()
    # 使设置生效
    termios.tcsetattr(fd, termios.TCSANOW, new_ttyinfo)
    # 从终端读取
    os.read(fd, 7)

    # 还原终端设置
    termios.tcsetattr(fd, termios.TCSANOW, old_ttyinfo)


if __name__ == "__main__":
    press_any_key_exit("按任意键继续...")
    press_any_key_exit("按任意键退出...")
```

其他关于`termios`的信息可以参考Linux手册:

```python
man 3 termios
```

另补充一下*nix终端的三种模式(摘自<Unix-Linux编程实践教程>)

### 规范模式

规范模式, 也被成为cooked模式, 是用户常见的模式.驱动程序输入的字符保存在缓冲区, 并且仅在接收到回车键时才将这些缓冲的字符发送到程序.缓冲数据使驱动程序可以实现最基本的编辑功能, 被指派这些功能的特定键在驱动程序里设置, 可以通过命令stty或系统调用tcsetattr来修改

### 非规范模式

当缓冲和编辑功能被关闭时, 连接被成为非规范模式.终端处理器仍旧进行特定的字符处理, 例如处理Ctrl-C及换行符之间的转换, 但是编辑键将没有意义, 因此相应的输入被视为常规的数据输入 程序需要自己实现编辑功能

### raw模式

当所有处理都被关闭后, 驱动程序将输入直接传递给程序, 连接被成为raw模式.

**相关资源：**

1. https://stackoverflow.com/questions/13207678/whats-the-simplest-way-of-detecting-keyboard-input-in-python-from-the-terminal
2. https://raspberrypi.stackexchange.com/questions/28302/python-script-with-loop-that-detects-keyboard-input
3. http://www.zmonster.me/2015/03/02/keyboard-control-with-python.html
4. https://www.jianshu.com/p/03010ac70e4c
5. https://blog.csdn.net/howard2005/article/details/79446980
6. https://codeday.me/bug/20181024/320687.html



这些答案都不适合我。这个包，pynput，正是我所需要的。 <https://pypi.python.org/pypi/pynput>

```python
from pynput.keyboard import Key, Listener
def on_press(key):
    print('{0} pressed'.format(
        key))
def on_release(key):
    print('{0} release'.format(
        key))
    if key == Key.esc:
        # Stop listener
        return False
# Collect events until released
with Listener(
        on_press=on_press,
        on_release=on_release) as listener:
    listener.join()
```



## 获取手柄输入

### linux

<https://github.com/FRC4564/Xbox>

<https://diyprojects.io/python-library-evdev-raspberry-pi-use-gamepad-diy-projects-servomotor-games/#.Xa8ZR-gzaUl>

### win

<https://github.com/zeth/inputs>

## 数据分析

### 基本概念

**统计学中的基本概念：**

- 统计学里最基本的概念就是样本的均值、方差、标准差。首先，我们给定一个含有n个样本的集合，下面给出这些概念的公式描述：

- 均值：

  

  ![img](https:////upload-images.jianshu.io/upload_images/6634703-f37620002ec4a293.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/89/format/webp)

  

- 标准差：

  

  ![img](https:////upload-images.jianshu.io/upload_images/6634703-c418d79c89498be9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/176/format/webp)

  

- 方差：

  

  ![img](https:////upload-images.jianshu.io/upload_images/6634703-ac8487815412b284.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/163/format/webp)

- 均值描述的是样本集合的中间点，它告诉我们的信息是有限的，而标准差给我们描述的是样本集合的各个样本点到均值的距离之平均。

> 以这两个集合为例，[0, 8, 12, 20]和[8, 9, 11, 12]，两个集合的均值都是10，但显然两个集合的差别是很大的，计算两者的标准差，前者是8.3后者是1.8，显然后者较为集中，故其标准差小一些，标准差描述的就是这种“散布度”。
>
> 之所以除以n-1而不是n，是因为这样能使我们以较小的样本集更好地逼近总体的标准差，即统计上所谓的“无偏估计”。详见<https://www.zhihu.com/question/20099757>（马同学）而方差则仅仅是标准差的平方。



**为什么需要协方差：**

**标准差和方差一般是用来描述一维数据的**，但现实生活中我们常常会遇到含有多维数据的数据集，最简单的是大家上学时免不了要统计多个学科的考试成绩。面对这样的数据集，我们当然可以按照每一维独立的计算其方差，但是通常我们还想了解更多，比如，一个男孩子的猥琐程度跟他受女孩子的欢迎程度是否存在一些联系。**协方差就是这样一种用来度量两个随机变量关系的统计量**，我们可以仿照方差的定义：

![img](https:////upload-images.jianshu.io/upload_images/6634703-755f748b820b9662.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/241/format/webp)



来度量各个维度偏离其均值的程度，协方差可以这样来定义：



![img](https:////upload-images.jianshu.io/upload_images/6634703-1533c21438fb4073.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/258/format/webp)

协方差的结果有什么意义呢？如果结果为正值，则说明两者是正相关的（从协方差可以引出“相关系数”的定义），也就是说一个人越猥琐越受女孩欢迎。如果结果为负值， 就说明两者是负相关，越猥琐女孩子越讨厌。如果为0，则两者之间没有关系，猥琐不猥琐和女孩子喜不喜欢之间没有关联，就是统计上说的“相互独立”。

从协方差的定义上我们也可以看出一些显而易见的性质，如：

![img](https:////upload-images.jianshu.io/upload_images/6634703-ec30fb9a09a846d1.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/176/format/webp)

![img](https:////upload-images.jianshu.io/upload_images/6634703-cb349f1c318baf24.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/195/format/webp)

**皮尔森相关系数**(Pearson correlation coefficient)



![img](https:////upload-images.jianshu.io/upload_images/6634703-f5a1c35e731295c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/433/format/webp)
$$
p_x,_y = corr(X,Y)=\frac{cov(X,Y)}{\sigma_x\sigma_y}=\frac{E[(X-u_x)(Y-u_y)]}{\sigma_x\sigma_y}
$$


- 协方差就是俩人跳舞的舞步协同程度，如果一起向前走或者向后退，协方差就是正值；如果一个朝前一个朝后，协方差就是负值；如果各自都不动，就是零。
- 相关系数就是标准化的协方差，就是剔除了俩人舞步尺度大小不一的影响。
- 协方差对于相关系数 等价于 标准差对于变异系数。（这里感谢 x2yline 的指点！）



**协方差矩阵**

前面提到的猥琐和受欢迎的问题是典型的二维问题，而协方差也只能处理二维问题，那维数多了自然就需要计算多个协方差，比如n维的数据集就需要计算

![img](https:////upload-images.jianshu.io/upload_images/6634703-4b4d4b9140228a81.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/91/format/webp)

 个协方差，那自然而然我们会想到使用矩阵来组织这些数据。给出协方差矩阵的定义：

![img](https:////upload-images.jianshu.io/upload_images/6634703-3913643c3b1c0135.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/309/format/webp)

这个定义还是很容易理解的，我们可以举一个三维的例子，假设数据集有三个维度，则协方差矩阵为：

![img](https:////upload-images.jianshu.io/upload_images/6634703-f87c48298d442c3f.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/302/format/webp)

可见，协方差矩阵是一个对称的矩阵，而且对角线是各个维度的方差。

### 相关系数矩阵图

```python
import numpy as np
import pandas as pd 
import matplotlib.pyplot as plt

data = np.random.rand(1000,5)
df = pd.DataFrame(data) # 将numpy数组转为df
correlations = df.corr() # 计算变量之间的相关系数矩阵

# plot correlation matrix
fig, ax = plt.subplots(figsize=(9,9)) #调用figure创建一个绘图对象

cax = ax.matshow(correlations, vmin=-1, vmax=1)  #绘制热力图，从-1到1

fig.colorbar(cax)  #将matshow生成热力图设置为颜色渐变条
ticks = np.arange(0,5,1) #生成0-9，步长为1
ax.set_xticks(ticks)  #生成刻度
ax.set_yticks(ticks)
#ax.set_xticklabels(names) #生成x轴标签
#ax.set_yticklabels(names)
plt.show()
```





### 热力图（相关系数矩阵图）

```python
import seaborn as sns
import numpy as np
import pandas as pd
a = np.random.rand(4,3)
fig, ax = plt.subplots(figsize = (9,9))
#二维的数组的热力图，横轴和数轴的ticklabels要加上去的话，既可以通过将array转换成有column
#和index的DataFrame直接绘图生成，也可以后续再加上去。后面加上去的话，更灵活，包括可设置labels大小方向等。
sns.heatmap(pd.DataFrame(np.round(a,2), columns = ['a', 'b', 'c'], index = range(1,5)), 
                annot=True, vmax=1,vmin = 0, xticklabels= True, yticklabels= True, square=True, cmap="YlGnBu")
#sns.heatmap(np.round(a,2), annot=True, vmax=1,vmin = 0, xticklabels= True, yticklabels= True, 
#            square=True, cmap="YlGnBu")
ax.set_title('二维数组热力图', fontsize = 18)
ax.set_ylabel('数字', fontsize = 18)
ax.set_xlabel('字母', fontsize = 18) #横变成y轴，跟矩阵原始的布局情况是一样的

```



```python
ax.set_yticklabels(['一', '二', '三'], fontsize = 18, rotation = 360, horizontalalignment='right')
ax.set_xticklabels(['a', 'b', 'c'], fontsize = 18, horizontalalignment='right')
```

<https://zhuanlan.zhihu.com/p/35494575>



## 数据结构

### 队列

先进先出队列(或简称队列)是一种基于先进先出(FIFO)策略的集合类型.

队列的最简单的例子是我们平时碰到的,比如排队等待电影，在杂货店的收营台等待，在自助餐厅排队等待（这样我们可以弹出托盘栈）。行为良好的线或队列是有限制的，因为它只有一条路，只有一条出路。不能插队，也不能离开。你只有等待了一定的时间才能到前面。下图展示了一个简单的 Python 对象队列。




![img](https:////upload-images.jianshu.io/upload_images/8521113-b1793506cb18d9f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/612/format/webp)



> 队列是有序数据集合，队列的特点，删除数据项是在头部，称为前端(front)，增加数据在尾部，称为后端(rear)

#### Queue

```python
# 导入队列
#from queue import Queue
from multiprocessing import Queue

# 最多接收3个数据
q = Queue(3)

# put 向队列中添加数据
q.put(1)
q.put(2)
q.put(3)

# 获取当前队列长度
print(q.qsize())

# 取出最前面的一个数据 1 , 还剩两个
print(q.get())

# 再加入数据
q.put(4)

#超过三个了.如果没有timeout参数会处于阻塞状态,卡在那边.若设置2秒,2秒后会raise 一个 FULL的报错
q.put(5, timeout=2))

# 当然,也可以直接给个 block=False,强制设置为不阻塞(默认为会阻塞的)，一旦超出队列长度，立即抛出异常
q.put(6, block=False)

# 同样的,当取值(get)的次数大于队列的长度的时候就会产生阻塞，设置超时时间意为最多等待x秒，队列中再没有数据，就抛出异常.也可以使用block参数,跟上面一样
```

其他常用方法:

```python
# empty: 检查队列是否为空，为空返回True，不为空返回False
# full : 判断队列是否已经满了

# join & task_done :
q = Queue(2)
q.put('a')
q.put('b')
# 程序会一直卡在下面这一行，只要队列中还有值，程序就不会退出
q.join()
-------------------------------------------------------------
q = Queue(2)
q.put('a')
q.put('b')

q.get() 
q.get()
# 插入两个元素之后再取出两个元素，执行后发现，程序还是卡在下面的那个join代码
q.join()
-------------------------------------------------------------
q = Queue(2)
q.put('a')
q.put('b')

q.get()
# get取完队列中的一个值后，使用task_done方法告诉队列，我已经取出了一个值并处理完毕,下同
q.task_done() 
q.get()
#在每次get取值之后，还需要在跟队列声明一下，我已经取出了数据并处理完毕，这样执行到join代码的时候才不会被卡住
q.task_done()
q.join()
```

#### 双向队列

但是删除列表的第一个元素（抑或是在第一个元素之前添加一个元素）之类的操作是很耗时的，因为这些操作会牵扯到移动列表里的所有元素。

**deque**

collections.deque 类（双向队列）是一个线程安全、可以快速从两端添加或者删除元素的数据类型。而且如果想要有一种数据类型来存放“最近用到的几个元素”，deque 也是一个很好的选择。这是因为在新建一个双向队列的时候，你可以指定这个队列的大小，如果这个队列满员了，还可以从反向端删除过期的元素，然后在尾端添加新的元素.
 使用示例如下:

```python
>>> from collections import deque
>>> dq = deque(range(10), maxlen=10) ➊
>>> dq
deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
>>> dq.rotate(3) ➋
>>> dq
deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6], maxlen=10)
>>> dq.rotate(-4)
>>> dq
deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], maxlen=10)
>>> dq.appendleft(-1) ➌
>>> dq
deque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
>>> dq.extend([11, 22, 33]) ➍
>>> dq
deque([3, 4, 5, 6, 7, 8, 9, 11, 22, 33], maxlen=10)
>>> dq.extendleft([10, 20, 30, 40]) ➎
>>> dq
deque([40, 30, 20, 10, 3, 4, 5, 6, 7, 8], maxlen=10)
```

❶ maxlen 是一个可选参数，代表这个队列可以容纳的元素的数量，而且一旦设定，这个
 属性就不能修改了。
 ❷ 队列的旋转操作接受一个参数 n，当 n > 0 时，队列的最右边的 n 个元素会被移动到
 队列的左边。当 n < 0 时，最左边的 n 个元素会被移动到右边。
 ❸ 当试图对一个已满（len(d) == d.maxlen）的队列做尾部添加操作的时候，它头部
 的元素会被删除掉。注意在下一行里，元素 0 被删除了。
 ❹ 在尾部添加 3 个元素的操作会挤掉 -1、1 和 2。
 ❺ extendleft(iter) 方法会把迭代器里的元素逐个添加到双向队列的左边，因此迭代
 器里的元素会逆序出现在队列里。

#### 其他队列

<https://my.oschina.net/yangyanxing/blog/296052>

Python提供的所有队列类型  :

1. 先进先出队列 queue.Queue
2. 后进先出队列 queue.LifoQueue (Queue的基础上进行的封装)
3. 优先级队列 queue.PriorityQueue (Queue的基础上进行的封装)
4. 双向队列 queue.deque

除了上述提到的队列与双端队列,还有两个用的比较少的:后进先出队列与优先级队列

#### 自己队列实现

> 在实际编码中不会自己来实现一个队列.因为python本身就有自带的队列库.如果想自己实现可以利用列表的一些特性,比如.append或者.pop来实现.也可以抛开列表重新定义一个队列.这里有一个很好的例子来实现[http://zhaochj.github.io/2016/05/15/2016-05-15-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84-%E5%8D%95%E7%AB%AF%E9%98%9F%E5%88%97/](https://link.jianshu.com?t=http%3A%2F%2Fzhaochj.github.io%2F2016%2F05%2F15%2F2016-05-15-%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84-%E5%8D%95%E7%AB%AF%E9%98%9F%E5%88%97%2F)

#### **相关链接**

[https://facert.gitbooks.io/python-data-structure-cn/3.%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84/3.10.%E4%BB%80%E4%B9%88%E6%98%AF%E9%98%9F%E5%88%97/](https://link.jianshu.com?t=https%3A%2F%2Ffacert.gitbooks.io%2Fpython-data-structure-cn%2F3.%E5%9F%BA%E6%9C%AC%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%2F3.10.%E4%BB%80%E4%B9%88%E6%98%AF%E9%98%9F%E5%88%97%2F)   
 [https://docs.lvrui.io/2016/07/20/Python%E4%B8%AD%E5%85%88%E8%BF%9B%E5%85%88%E5%87%BA%E9%98%9F%E5%88%97queue%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8/](https://link.jianshu.com?t=https%3A%2F%2Fdocs.lvrui.io%2F2016%2F07%2F20%2FPython%E4%B8%AD%E5%85%88%E8%BF%9B%E5%85%88%E5%87%BA%E9%98%9F%E5%88%97queue%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%2F)





## 其他

### @ 修饰符

在模块或者类的定义层内对函数进行修饰。出现在函数定义的前一行，不允许和函数定义在同一行。

```python
def funcA(A):
    print("function A")
    print(A)
 
def funcB(B):
    print(B(2))
    print("function B")
 
@funcA
@funcB
def func(c):
    print("function C")
    return c**2


--output--
function C
4
function B
function A
```







```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```





```python
x
```

