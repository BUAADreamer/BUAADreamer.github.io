---
title: Python数据分析可视化笔记
date: 2021-02-04 15:27:43
tags: 数据科学
---

关于python数据科学相关的知识笔记

<!--more-->

# Python数据分析可视化笔记

#### jupyter lab好东西集合

https://www.cnblogs.com/feffery/p/13364668.html

## pandas基本操作和概念

`DataFrame`对象单独取出一行即：`df.loc[0]`和一列`df["a"]`都是`Series`对象

```python
import nunpy as np
import pandas as pd
df.columns #DataFrame对象所有表头值组成的列表
df.index #DataFrame对象所有索引组成的列表
```

### 创建DataFrame对象

最简单也最好用：根据两个列表创建

```python
index=[i for i in range(100)]
columns=["a","b"]
df=pd.DataFrame(index=index,columns=columns)
l1=[i for i in range(100)]
l2=[1,2]
df["a"]=l1 #直接用列表给df一列赋值
df.loc[0]=l2 ##直接用列表给df一行赋值
```

## 数据分析6步

### 一、读取数据

```python
import pandas as pd
import numpy as np
pd.read_excel("1.xlsx",)
pd.read_csv("1.csv")
#查询用法
pd.read_excel?
pd.*read*?
```

### 二、清洗数据

#### 查找异常

```python
#设置最多显示十行
pd.set_option('max_rows', 10) 
#查找异常
df[df["a"].isnull()]
#查找完全重复的行
df[df.duplicated()]
# 查找某⼀列重复的⾏
df[df.编号.duplicated()]
# 查找a属性的所有唯⼀值
df.a.unique()
# 查找a包含 30 的异常值
df[df.a.isin(['30'])]
#字符换匹配
df[df.a.str.contains("abc",na=False)]
# 查找a列值在1到5之间的⾏
df[df.a.between(1, 5)]
#逻辑运算
df[(df.a >= 1) & (df.b <= 5)]
```

#### 排除重复

```python
df.drop_duplicates()
df.drop_duplicates(inplace=True) #直接改变原数据
df.drop_duplicates(['a']) # 按某⼀列排除重复，默认保留第⼀⾏ keep='last'-->删除最后一行
```

#### 删除缺失值

```python
color.dropna()
color.dropna(how='all') # 删除全部为空的⾏
color.dropna(axis=1) # 删除包含缺失值的列
```

#### 补全缺失值

```python
df.fillna('a') #所有缺失处补上"a" 	实际工作时常补上0
df.fillna(method='bfill') #⽤后⾯的值填充
df.fillna( {'花⾊': 0, '牌⾯': 1} ) #按照字典填充
```

#### 完整例子

```python
import numpy as np
import pandas as pd
# 设置最多显示 10 ⾏
pd.set_option('max_rows', 10)
# 从 Excel ⽂件中读取原始数据
df = pd.read_excel('待清洗的扑克牌数据集.xlsx',sheet_name="name")
# 补全缺失值
df = df.fillna('Joker')
# 排除重复值
df = df.drop_duplicates()
# 修改异常值
df.loc[4, '牌⾯'] = 3
# 增加⼀张缺少的牌
df = df.append({'编号': 4,'花⾊': '⿊桃♠','牌⾯': 2},ignore_index=True)
# 按编号排序
df = df.sort_values('编号')
# 重置索引
df = df.reset_index()
# 删除多余的列
df = df.drop(['index'], axis=1 )
# 把清洗好的数据保存到 Excel ⽂件
df.to_excel('完成清洗的扑克牌数据.xlsx',index=False)
df
```

### 三、操作数据

#### 增加数据

```python
df["b"]=[i for i in range(100)] #相当于把df对象当字典用
df.insert(1, 'a', [1, 2]) #指定位置插入列
#插入一行
a=pd.DataFrame({"a":1,"b":2})
df2=df.append(a,ignore_index=True,sort=False)
#拼接两个数据框
pd.concat([df, a],ignore_index=True,sort=False)
还有merge()/join()函数
```

#### 删除数据

```python
df.drop([0,2]) #删除0/2两行数据
df.drop("a",axis=1) #删除"a"列的数据
del df2["a"] #直接删除原数据框中的a列数据
```

#### 修改数据

```python
df.replace(["1","2",3,4],"",regex=True) #替换多个数据 regex-->正则表达式
df.loc[0,"a"]="j" #修改第0行a属性的值
df.columns = list('AB') #修改列名
df.rename({"a":"b"},axis=1) #修改指定列名
df.rename({1:3,2:3}) #修改行名
```

#### 查询数据

```python
df.head()
import re
df[df.a.str.match('[jk]', flags=re.IGNORECASE, na=False)] #re.IGNORECASE-->不区分大小写
#使用query
df.query(
 '1 <= index <= 5 \
 and 牌⾯ not in @card '
)
df.query(
 '编号 > 编号.mean() \
 and 牌⾯ == "A" '
)
```

### 四、数据转换

#### 转换为时间

```python
from dateutil.parser import parse
df["time"]=df.Date.transform(parse).copy()
parse("10/9/2019",dayfirst=True)
```

#### 转换为数值

```python
pd.to_numeric(errors='coerce') #设置errors参数使得不能替换的值变为nan
df.apply(pd.to_numeric,errors='coerce') #errors默认为ignore即略过不能转换的
```

#### 类型转换

```python
df.astype(str).dtypes
对于时间型的数据，我们可以使⽤ strftime() 函数
```

#### 转换为区间

```python
pd.cut() #将数值切割为指定的区间
bins=[float("-inf"),280,float("inf")]
pd.cut(df.a,bins=bins,right=True) #左开右闭，默认左闭右开
pd.cut(df.a,bins=2) #按照2为间隔划分
```

#### 分组转换

```python
df.groupby("a")["b"].transform(sum) #在⾏数保持不变的情况下，对某列进⾏分组求和
#能够⽅便地计算每个数据对应各⾃分组的占⽐
df.groupby("a")["b"].apply(sum) #返回组数个结果
df.groupby("a")["b"].agg([sum,np.mean,"count"])
df.groupby("month").agg(
    天数=("Date",count),
	平均价格=("Open",np.mean)) #对不同列聚合，使用不同函数
```

#### 标准化

```python
#min-max标准化
(df.a-df.a.min())/(df.a.max()-df.a.min())
```

### 五、整理数据

#### 外连接(相当于并集)
```python
df.merge(df1,how="outer",on="用于连接的列名") #缺失值用nan 得到两个表的所有⾏
```

#### 内连接(相当于交集)
```python
df.merge(df1) #不指定on时以两个表共同的列名为参数
#只返回两个表相互匹配的数据。无nan
```

#### 左连接与右连接
```python
df.merge(df1,how="left"）#df为左表，即使df1中某行值无也照样显示,同理右连接
```

#### 交叉连接
```python
操作:
（1）⽤ assign() 函数增加 key 列；
（2）⽤ merge() 函数进⾏连接；
（3）删掉 key 列。
df.assign(key=1).merge(df1.assign(key=1),on="key").drop("key",1)
```

#### 联合拼接
```python
pd.concat([df1,df2],sort=False)
```

### 六、分析数据

比较nb的三个数据分析库

参考文章：https://my.oschina.net/u/4581316/blog/4898542

#### pandas_profiling

```python
import pandas_profiling
import seaborn as sns
sns.set_style("whitegrid",{"font.sans-serif":["simhei","Arial"]})
x=pandas_profiling.ProfileReport(df)
x.to_file("name.html")
```

#### Sweetviz

```python
import sweetviz as sv
my_report = sv.analyze(mpg)
my_report.show_html()
```

#### pandasGUI

```python
from pandasgui import show
# 部署GUI的数据集
gui = show(mpg)
```

## 可视化

### pyecharts

官网：https://pyecharts.org/#/zh-cn/intro