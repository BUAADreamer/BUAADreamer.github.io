---
title: OO-Pre-学习笔记
date: 2021-03-08 13:24:08
tags: [Java,BUAA-OO-2021]
---

2021年OO的Pre部分学习笔记

<!--more-->

## 工具

### gitlab维护代码

#### Command line instructions

##### Git global setup

```shell
git config --global user.name "冯张驰"
git config --global user.email "19373573@buaa.edu.cn"
```

##### Create a new repository

```shell
git clone https://gitlab.buaaoo.top/oo_homeworks_2021/仓库名.git
cd oo_2021_pre2_19373573_pre2_task2
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

##### Existing folder

```shell
cd existing_folder
git init
git remote add origin https://gitlab.buaaoo.top/oo_homeworks_2021/仓库名.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

##### Existing Git repository

```shell
cd existing_repo
git remote rename origin old-origin
git remote add origin https://gitlab.buaaoo.top/oo_homeworks_2021/仓库名.git
git push -u origin --all
git push -u origin --tags
```

#### 补充-实验1git学习

##### step 1 新建仓库

在本地新建一个空文件夹，在此目录下打开终端（bash/git bash/power shell/…）

输入

> git init

从而得到一个新的`git`仓库

##### step 2 关联远程仓库

目前为止，`step 1`建立的文件夹还是一个空文件夹，需要关联两个远程仓库，首先考虑第一个远程仓库

第一个远程仓库是课程组提供的远程公共仓库`share`

> git remote add share git@gitlab.buaaoo.top:oo_2021_public/experiment/exp1_public.git

##### step 3 从远程仓库拉取文件

关联后可以从远程仓库拉取文件

执行命令

> git pull share master

从`share`仓库的`master`分支下得到两个文件夹

##### step 4 忽略不必要的文件

文件夹`share`中的内容是实现了`Comparable`接口并按序输出的例程，可以尝试跑通

文件夹`poly`中的内容是任务二中需要完善的程序，是需要提交的，而且实验要求，提交时**忽略**`share`文件夹下的所有内容，故需要使用`.gitignore`

在当前工作的**根目录**下新建文件`.gitignore`

在其中写入

> share/*

表示`push`时忽略子目录`share`下的所有文件

##### step 5 删除暂存区文件

为了保证`.gitignore`正常工作，需要删除暂存区的文件

> git rm --cached . -r

##### step 6 删除与远程仓库的关联

为了避免不必要的干扰，需要删除与远程仓库`share`的关联

> git remote remove share

##### step 7 关联个人实验1仓库，并尝试一次提交

> git remote add origin 你的个人实验1的远程仓库链接
>
> git add .
>
> git commit -m "anything you want to write"
>
> git push -u origin master

关联到远程仓库`origin`，并尝试提交

至此，任务一的内容已全部介绍完毕，如果按照上述步骤操作后得到预期结果（例如可以正常`pull`和`push` ，且`.gitignore`文件生效等），即可开始进行任务二，课程组会对你的`git`操作进行评判

#### 平时常用操作

##### 在IDEA里使用git

点击工具栏的`VCS`，选择 `Import into Vision Control` 里的 `Create Git Repository `

之后就可以在IDEA里完成 `commit` `push` 的操作了（右上角的对钩和绿色箭头），第一次 `push` 时可以设置远程仓库关联。还有`rollback`等操作

##### 一行shell代码完成更新提交

###### 初次提交

```shell
git add . && git commit -m "Initial commit" && git push -u origin master 
```

###### 之后提交

```
git add . && git commit -m "Initial commit" && git push
```

##### 代码回退

```shell
git log #查看历史版本
git reset --hard xxx #xxx为对应版本的hashcode 
git push origin HEAD --force #远程也更新为当前版本
```

##### 一个项目配置多个远程源

更改源的名字

```bash
git remote rename origin task1
```

一次性添加好所有 task 的 remote：

```bash
#!/bin/bash
projectName="pre3"
ID=19XXXXXX
taskNum=6
for ((i=1;i<=$taskNum;++i))
do
    git remote add task${i} git@gitlab.buaaoo.top:oo_homeworks_2021/oo_2021_${projectName}_${ID}_${projectName}_task${i}.git
done
```

然后 push 的时候 specify 一下 push 到哪个 remote，比如：

```bash
git push task3 master
```

如果某个 task 反复 push，可以将其设置为当前 branch 的 upstream：

```bash
git push -u task3 master
```

之后直接使用 `git push` 即可。

也可以在 IDEA 内直接进行 push 操作。选择 Git > Push…，出现操作框。左侧显示你的 branch 和 commit，上面显示你要 push 的 remote 和 branch，点击可以选择具体要 push 的内容 / 具体要 push 到哪个 remote。

### IDEA

#### 使用checkstyle

##### 配置

最简单的配置方式是直接在`Settings -> Plugins` 里安装好

然后在 `New Projects Settings -> Settings for New Projects`里导入`config.xml`文件或者选择自带的两种代码风格检查方式。（这个default的设置仅适用于2020版，**IDEA 2018** :  `File -> Other Settings -> Default Settings`  **IDEA 2019** :  `File -> Other Settings -> Settings for New Projects` ）

##### 平时使用

`鼠标右键 -> Check Current File`

或者点击左下角窗口里的`checkstyle`的按钮也可

#### 配置符合课程要求

##### 设置不自动 `import xx.*`

`File -> setting -> Editor -> Code Style -> Java -> Imports`设置两个count为大于100的数字

### 评论区规范

#### 提问方式

针对作业内容答疑区

- 作业内容，请使用“【作业内容】”
- 评测要求，请使用“【评测要求】”

针对公共讨论区

- 课程规则，请使用“【课程规则】“
- 系统使用，请使用”【系统使用】“
- 技术交流，请使用”【技术交流】“

## Pre2

### task1

**封装**：是指隐藏对象的属性和实现细节（属性变量和内部方法用`private`修饰），仅对外提供公共访问⽅法 （访问方法使用`public`）。

### task2

java**常用容器**：HashSet，HashMap，ArrayList

容器**常用方法**：

```java
// 创建
ArrayList<Bookset> booksArrayList = new ArrayList<>(16);
Bookset book = new Bookset(name, price, num);
// 添加新元素
booksArrayList.add(book);
// 判断是否存在
if (booksArrayList.contains(book)) {
    System.out.println("We have it!");
}
// 遍历所有元素
for (Bookset item : booksArrayList) {
    System.out.println(item.getName());
}
// 输出容器规模
System.out.println(booksArrayList.size());
// 删除元素
booksArrayList.remove(book);
```

主要的坑在 `BigDecimal` `BigInteger` 类的使用上

需要用 `BigDecimal ans = BigDecimal.valueOf(price);` 这样的方式来维持精度，如果直接用 `BigDecimal(price)` 会丢失精度

### task3

#### 工厂模式

参考菜鸟教程https://www.runoob.com/design-pattern/factory-pattern.html

步骤如下

##### 1.创建一个接口

```java
public interface Shape {
   void draw();
}
```

##### 2.创建实体类

注意实体类是可以使用接口的方法去**调用类内的属性值**的

同时可以认为这些实体类都是**Shape**类型的

```java
public class Rectangle implements Shape {
   int a=0;
   @Override
   public void draw() {
      System.out.println(a);
   }
}

public interface BookFace {
    String getType();
    String getName();
    double getPrice();
    long getNum();
    String getOutput();
}

public class Bookset implements BookFace {
	//定义各个变量
    private String type;
    private String name;
    private double price;
    private long num;   
    private String output; 
    
    //定义构造方法
    public Bookset() {
        init(); //调用init函数进行初始化
    }
    
    //重写各个方法
}


public class BookShape {
    public static Bookset getShape(String type) {
        switch (type) {
            case "Other":
                return new Bookset();
            case "OtherA":
                return new OtherA();
            case "Novel":
                return new Novel();
            case "Poetry":
                return new Poetry();
            case "OtherS":
                return new OtherS();
            case "Math":
                return new Math1();
            case "Computer":
                return new Computer();
            default:
                break;
        }
        return null;
    }
}

BookFace book = BookShape.getShape(type);
```

同时也可以对一个实体类进行继承，子类也是实体类

```java
public class child extends Rectangle {
   int a=0;
   int b=0;
   public void draw() {
      System.out.println(a);
      System.out.println(b);
   }
}
```

##### 3.创建工厂，生成指定类型对象

```java
public class ShapeFactory {
   //使用 getShape 方法获取形状类型的对象
   public Shape getShape(String shapeType){
      if(shapeType == null){
         return null;
      }        
      if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      } else if(shapeType.equalsIgnoreCase("child")){
         return new child();
      }
      return null;
   }
}
```

##### 4.使用该工厂，通过传递类型信息来获取实体类的对象。

```java
ShapeFactory factory = new ShapeFactory();
Shape a = factory.getShape("RECTANGLE");
a.draw();
```

### task4

#### 异常处理

我写这里的时候用了比较拉跨的`if else`写法，不展示了，参考Roife的博客，记录一下怎么写

```java
package exceptions;

public class BooksetExistedException extends Exception {
    public BooksetExistedException(String name) {
        super("Oh, no! The " + name + " exist.");
    }
}

---------------------------------------------------------

import exceptions.BooksetExistedException;
try {
    bookshelves.get(i).addNewBookset(bookset);
} catch (BooksetExistedException e) {
    System.out.println(e.getMessage());
} finally { // 想想为啥要放在 finally 里面
    scanner.nextLine();
}
```

### task5

#### clone和equals方法

```java
@Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }
        if (obj == null) {
            return false;
        }
        if (getClass() != obj.getClass()) {
            return false;
        }
        BookFace other = (BookFace) obj;
        //其他比较逻辑
        if(other.a==this.a) {
            return true;
        } else if() {
            ...
        }
    }

//对于基本类型可以直接继承主类方法即可，String是不可变类型
@Override
public BookFace clone() {
	super.clone();
}
```

## Pre3

正则表达式菜鸟教程：https://www.runoob.com/regexp/regexp-tutorial.html

java正则表达式用法：https://www.runoob.com/java/java-regular-expressions.html

一篇补充的博客：https://blog.csdn.net/baidu_28289725/article/details/80414445

## 测试方法

https://blog.csdn.net/asdx1020/article/details/104956074

https://blog.csdn.net/asdx1020/article/details/104870918

