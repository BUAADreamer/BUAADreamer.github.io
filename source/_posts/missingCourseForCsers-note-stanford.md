---
title: missingCourseForCsers_note_stanford
date: 2021-02-12 21:53:03
tags: Tools
---

关于一些CS必备工具使用的笔记，根据斯坦福的缺失的一课课程以及一些其他网络教程整理得来。

<!--more-->

https://missing-semester-cn.github.io/

## 一、shell脚本

`Bourne Again SHell`——Bash

终端，文字接口，Shell

### 使用shell执行程序

如果执行的程序不是shell编程关键字，则咨询环境变量

shell使用空格分割命令

```shell
#打开后看到的:
muller:~$ #$表示不是root用户 muller-->用户名	下面的代码略去了用户名，只保留$后的
$ date #执行程序，打印当前时间
$ echo hello #echo-->打印函数 hello为参数 特殊字符(比如空格)可以用""、''将后面的参数括起来，也可以用转义字符
$ echo $path #打印环境变量所在路径
$ which echo #打印echo的环境变量路径
$ /bin/echo $path #直接使用路径名执行echo程序
```

### 在shell中导航

`linux`系统中`/`开头的都是**绝对路径**。

命令后带`-`接受标记和选项

```shell
$ pwd #获取当前目录
$ cd ../a #cd 切换到上级目录中的a文件夹 .表示当前目录 ..表示上一级目录
$ ls #查看目录下包含的文件
$ ls -help #或ls -h
$ mv 1.txt a.txt #重命名或移动文件
$ cp 1.txt #拷贝文件
$ mkdir #新建文件目录
$ man ls #man获取ls的手册 使用q退出
$ touch 1.txt #创建空文件
```

### 在程序间创建连接

shell中有输入输出流

```shell
$ echo hello > hello.txt #最简单的重定向输出，如果不存在hello.txt文件则生成出来一个
$ cat hello txt #获取文件内容
$ cat < hello.txt
$ cat < hello.txt > hello2.txt
```

使用>>可以进行追加输出

根用户：`sudo ` 命令开头，变为`root`用户

### 变量，控制语句

``` shell
$ a=1 #变量赋值
$ echo '$a' #打印出 $a
$ echo "$a" #打印出a变量的值，即1
```

以**`'`定义**的字符串为**原义字符串**，其中的变量不会被转义，而 **`"`定义**的字符串会**将变量值进行替换**。

```shell
mcd () {
    mkdir -p "$1"
    cd "$1"
}  #函数定义
```

- `$0` - 脚本名
- `$1` 到 `$9` - 脚本的参数。 `$1` 是第一个参数，依此类推。
- `$@` - 所有参数
- `$#` - 参数个数
- `$?` - 前一个命令的返回值
- `$$` - 当前脚本的进程识别码
- `!!` - 完整的上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 `sudo !!`再尝试一次。
- `$_` - 上一条命令的最后一个参数。如果你正在使用的是交互式shell，你可以通过按下 `Esc` 之后键入 . 来获取这个值。

命令通常使用 `STDOUT`来返回输出值，使用`STDERR` 来返回错误及错误码

**返回值0**表示**正常执行**，其他所有**非0的返回值**都表示**有错误发生**。

可以使用`false`和`true`以及`&&` `||`等逻辑表达式来作为条件，例如：`false || echo "1234"`

### Shell工具

#### 查找文件

```shell
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '**/test/**/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'

#对找到文件进行操作
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

`fd` `locate` 是另外两个查找工具

#### 查找代码

`grep` `ack` `ag` `rg`

```shell
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match "^#!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

#### 查找shell命令

`history` 命令允许您以程序员的方式来访问shell中输入的历史命令

使用 `Ctrl+R` 对命令历史记录进行回溯搜索。可以配合 [`fzf`](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r) 使用

#### 文件夹导航

Fasd 基于 [`frecency`](https://developer.mozilla.org/en/The_Places_frecency_algorithm) 对文件和文件排序，也就是说它会同时针对频率（frequency ）和时效（ recency）进行排序。

对于常用的目录，目录名子串前加入一个命令 `z` 就可以快速切换命令到该目录

还有一些更复杂的工具可以用来概览目录结构，例如 [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) 或更加完整的文件管理器，例如 [`nnn`](https://github.com/jarun/nnn) 或 [`ranger`](https://github.com/ranger/ranger)。

## 二、Vim编辑器

### 编辑模式

- 正常模式：在文件中四处移动光标进行修改（默认的模式）
- 插入模式：插入文本 
- 替换模式：替换文本
- 可视化（一般，行，块）模式：选中文本块
- 命令模式：用于执行命令

按下 `<ESC>` （退出键） 从任何其他模式返回正常模式。 正常模式和插入模式是最常用的两个模式

#### 正常模式与各个模式切换

键入 `i` 进入插入 模式， `R` 进入替换模式

 `v` 进入可视（一般）模式， `V` 进入可视（行）模式， `<C-v>` （Ctrl-V, 有时也写作 `^V`）进入可视（块）模式，

 `:` 进入命令模式。

- `:q` 退出 （关闭窗口）
- `:w` 保存 （写）
- `:wq` 保存然后退出
- `:e {文件名}` 打开要编辑的文件
- `:ls` 显示打开的缓存
- `:help {标题}`打开帮助文档
  - `:help :w` 打开 `:w` 命令的帮助文档
  - `:help w` 打开 `w` 移动的帮助文档

### 移动

- 基本移动: `hjkl` （左， 下， 上， 右）
- 词： `w` （下一个词）， `b` （词初）， `e` （词尾）
- 行： `0` （行初）， `^` （第一个非空格字符）， `$` （行尾）
- 屏幕： `H` （屏幕首行）， `M` （屏幕中间）， `L` （屏幕底部）
- 翻页： `Ctrl-u` （上翻）， `Ctrl-d` （下翻）
- 文件： `gg` （文件头）， `G` （文件尾）
- 行数： `:{行数}<CR>` 或者 `{行数}G` ({行数}为行数)
- 杂项： `%` （找到配对，比如括号或者 /* */ 之类的注释对）
- 查找：`f{字符}` `t{字符}` `F{字符}` `T{字符}`
  - 查找/到 向前/向后 在本行的{字符}
  - `,` / `;` 用于导航匹配
- 搜索: `/{正则表达式}`, `n` / `N` 用于导航匹配

### 编辑

- `i`进入插入模式
  - 但是对于操纵/编辑文本，不单想用退格键完成
- `O` / `o` 在之上/之下插入行
- `d{移动命令}` 删除 {移动命令}
  - 例如， `dw` 删除词, `d$` 删除到行尾, `d0` 删除到行头。
- `c{移动命令}`改变 {移动命令}
  - 例如， `cw` 改变词
  - 比如 `d{移动命令}` 再 `i`
- `x` 删除字符 （等同于 `dl`）
- `s` 替换字符 （等同于 `xi`）
- 可视化模式 + 操作
  - 选中文字, `d` 删除 或者 `c` 改变
- `u` 撤销, `<C-r>` 重做
- `y` 复制 / “yank” （其他一些命令比如 `d` 也会复制）
- `p` 粘贴
- 更多值得学习的: 比如 `~` 改变字符的大小写