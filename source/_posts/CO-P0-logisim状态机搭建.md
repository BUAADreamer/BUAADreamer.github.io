---
title: 计组笔记-P0-logisim状态机搭建
date: 2021-01-29 23:35:52
tags: 计算机组成原理


---

北航计算机组成原理课程设计的Project0笔记，主要是关于logisim的使用和状态机的搭建。

<!--more-->

## p0复习（logisim）

### 注意的点

- 输入信号一般是通过**MUX，多路选择器**来实现对输出结果的控制。
- 刚连接好电路时或者连接电路中，可能出现有一些线路**莫名其妙是蓝色**，这时**关闭logisim后再次打开**往往就好了。
- **comparator**器件默认是有符号的，要调成**unsigned**来避免出现无符号数比较出现错误。
- 一些**Arithmetic**模块器件使用时，由于**上下两侧也有输出端**，如果连在一起可能出现意外。
- 组合逻辑部分如果输入、输出的位数较少，可直接用**combinational analysis** 里的 **table** 傻瓜式生成电路。
- 出现“xxxx”或者“EEEE”往往是因为电路中有**连错的电路**，比如**把输入当成输出元件连了；某条线没有连接在模块/元件的端口上，而是连在了空白区域**。
- 出现“ ”、fewer than output we expected可能是时序问题，比如超出了题目给出的时间。或者期待输出非零值时输出了0。也有可能是**appearance**不对
- DMX最好设置成 **Three-state:Yes**    **Disabled output:Float**  如果Three state勾选为yes，那么DMX输出端**没有被选中的路径**会**保持**原来的值不变

### 时序逻辑—状态机

**参考文章**：[lyyf的logisim状态机博客](https://www.cnblogs.com/BUAA-YiFei/articles/13855136.html)

#### 注意

- **状态转移、输出逻辑**用**真值表生成**。
- 寄存器的Q端在**时钟信号为0时**的值为**初态**，时钟信号的**上升沿**更新为**次态**。
- **Moore输出逻辑**，只和**当前状态**有关。
- **Mealy输出逻辑**，除了**当前状态**，还必须和**输入**发生联系。
- （**当前状态值**：时钟上升沿前是寄存器现有的值，上升沿时是状态转移模块的输出值s'）

#### Moore型

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/moore.png)

#### Mealy型

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/mealy.png)

#### 同步复位

具体来说就是用一个多路选择器，如果复位信号**为0**，则**正常更新状态值**，**如果为1**，就直接赋值给寄存器**常量0**。

![](https://fzc-1300590701.cos.ap-nanjing.myqcloud.com/blogImages/同步复位.png)

#### 异步复位

直接使用寄存器自带的**clear**端口进行复位即可

王pb助教的logisim评测帖子：http://cscore.net.cn/courses/course-v1:Internal+B3I062410+2020_T1/discussion/forum/course/threads/5f8ab909cf9bcc0efe0000bd

### logisim命令行调试

要用 jar 文件

我的电脑：java -jar logisim-generic-2.7.1.jar fsm.circ -tty table