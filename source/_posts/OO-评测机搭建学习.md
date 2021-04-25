---
title: OO-评测机搭建学习
date: 2021-03-09 10:53:25
tags: [BUAA-OO-2021]
---

关于OO作业评测机搭建的探索

<!--more-->

### 将java项目打包成jar文件并运行

#### 在IDEA中打包

`File->project structure`

在弹窗最左侧选中 `Artifacts->"+"` ,选 `jar`，选择 `from modules with dependencies`

此处需要注意两点：

- 需要选择jar包默认运行的入口类
- 需要设置MANIFEST.MF的位置

之后点`Build->Build Artifacts->在选项中点击build即可`

####  运行

`java -jar hello.jar`

### 生成数据—xeger

```python
from xeger import Xeger
str="你的正则表达式"
x=Xeger(limit=10) #初始化，设置最大长度
for i in range(10):
	x.xeger(str) #循环生成字符串
```

### 驱动文件获得输出—subprocess

最简单的就用`os.system("java 1.jar << 1.txt >> out.txt")`的方式即可

### 正确性判定—sympy

用到的`sympy`库函数有 `Symbol、diff、sympify`

此外可能需要`eval`来去掉字符串的外围双引号