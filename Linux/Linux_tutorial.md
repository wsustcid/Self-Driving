# 软件
## ppa源
1. 添加：
```language
sudo add-apt-repository ppa:user/ppa-name
sudo apt-get update
```
2. 删除
然后进入 /etc/apt/sources.list.d 目录，将相应 ppa 源的保存文件删除。然后更新一下。





# 文件基本操作
+ 文件夹
    - mkdir temp
+ 文件
    - mv filename.txt temp 
+ 压缩
    - .tar.bz2 
　　解压：tar jxvf FileName.tar.bz2
　　压缩：tar jcvf FileName.tar.bz2 DirName 
　- .zip 
　　解压：unzip FileName.zip 
　　压缩：zip FileName.zip DirName 
　　压缩一个目录使用 -r 参数，-r 递归。例： $ zip -r FileName.zip DirName 
    ​    - .tar 
　　解包：tar xvf FileName.tar 
　　打包：tar cvf FileName.tar DirName 
　　（注：tar是打包，不是压缩！） 

  https://www.cnblogs.com/wangluochong/p/7194037.html



```bash
rm -rf * 删除当前目录下的所有文件及文件夹

# 删除 out/目录下的所有fingerprint.default_WFH.so文件。
find out/  -name fingerprint.default_WFH.so  |xargs rm -rf 

# 删掉out目录下后缀是so文件：    
find out/  -name '*.so'  |xargs rm -rf 

find ./ -name '*.jpg' | xargs -i mv {}  ../test/

 

find ./ -name '*' | xargs rm -rf


```





Linux下有三个命令：`ls`、`grep`、`wc`。通过这三个命令的组合可以统计目录下文件及文件夹的个数。

- 统计当前目录下文件的个数（不包括目录）

```
$ ls -l | grep "^-" | wc -l
```

- 统计当前目录下文件的个数（包括子目录）

```
$ ls -lR| grep "^-" | wc -l
```

- 查看某目录下文件夹(目录)的个数（包括子目录）

```
$ ls -lR | grep "^d" | wc -l
```

**命令解析：**

- `ls -l`

长列表输出该目录下文件信息(注意这里的文件是指目录、链接、设备文件等)，每一行对应一个文件或目录，`ls -lR`是列出所有文件，包括子目录。

- `grep "^-"`
  过滤`ls`的输出信息，只保留一般文件，只保留目录是`grep "^d"`。
- `wc -l`
  统计输出信息的行数，统计结果就是输出信息的行数，一行信息对应一个文件，所以就是文件的个数。

# 系统修复
1. 修复新加卷```sudo ntfsfix /dev/sdb8 ```






# 系统美化
hollwood
```language
$ sudo apt-add-repository ppa:hollywood/ppa
$ sudo apt-get update
$ sudo apt-get install hollywood cmatrix
```



安装terminator

```
sudo apt-get install terminator
#修改配置文件
cd ~/.config/
mkdir terminator
vim config
```

```bash
[global_config]
  title_transmit_bg_color = "#d30102"
  focus = system
  suppress_multiple_term_dialog = True
[keybindings]
[profiles]
  [[default]]
    palette = "#2d2d2d:#f2777a:#99cc99:#ffcc66:#6699cc:#cc99cc:#66cccc:#d3d0c8:#747369:#f2777a:#99cc99:#ffcc66:#6699cc:#cc99cc:#66cccc:#f2f0ec"
    background_color = "#2D2D2D" # 背景颜色
    background_image = None   
    background_darkness = 0.85 
    cursor_color = "#2D2D2D" # 光标颜色
    cursor_blink = True # 光标是否闪烁
    foreground_color = "#EEE9E9" # 文字的颜色
    use_system_font = False # 是否启用系统字体
    font = Ubuntu Mono 13  # 字体设置，后面的数字表示字体大小
    copy_on_selection = True # 选择文本时同时将数据拷贝到剪切板中
    show_titlebar = False # 不显示标题栏，也就是 terminator 中那个默认的红色的标题栏
[layouts]
  [[default]]
    [[[child1]]]
      type = Terminal
      parent = window0
      profile = default
    [[[window0]]]
      type = Window
      parent = ""
[plugins]
```

## 常用快捷键

- 水平分割终端

  ```
  Ctrl+Shift+O
  ```

- 垂直分割终端

  ```
  Ctrl+Shift+E
  ```

- 搜索

  ```
  Ctrl+Shift+F
  ```

- 复制

  ```
  Ctrl+Shift+C
  ```

- 粘贴

  ```
  Ctrl+Shift+V
  ```

- 关闭当前终端

  ```
  Ctrl+Shift+W
  ```

- 退出当前窗口

  ```
  Ctrl+Shift+Q
  ```

- 打开终端

  ```
  Ctrl+Shift+T
  ```

- 切换显示当前窗口

  ```
  Ctrl+Shift+X
  ```

- 全屏状态

  ```
  F11
  ```

- clear屏幕

  ```
  Ctrl+Shift+G
  ```

- 在垂直分割的终端中将分割条向右移动

  ```
  Ctrl+Shift+Right
  ```

- 在垂直分割的终端中将分割条向左移动

  ```
  Ctrl+Shift+Left
  ```

- 隐藏/显示滚动条

  ```
  Ctrl+Shift+S
  ```





<https://blog.csdn.net/funnyPython/article/details/93588705>

<https://blog.csdn.net/Scythe666/article/details/86592035>