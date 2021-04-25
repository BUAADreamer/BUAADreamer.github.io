---
title: Hello,My personal Blog!个人博客搭建总结
date: 2021-01-29 6:00:00
tags: hello my blog

---

经过了好几天的摸索，终于大概部署好了个人博客，之后会把所学的一些CS知识技术整理成笔记记录在这里，欢迎大家时不时来踩一踩！

这篇博客主要是关于个人博客搭建和更新方法的笔记，怕之后忘了所以赶紧记一下。目前博客仅仅是实现了基本功能，之后可能会视情况设置一下评论区，点赞，社交等功能。

<!--more-->

### 个人博客搭建总结

#### Jekyll

这段时间为了搭建一下自己的博客尝试了两种方式，一种是基于`Jekyll`的，这种方式比较简单，但是有时过于简单了，之前试过用一个cayman的主题，但由于本人太菜，这个模板需要配置太多，只好放弃了，改用hexo搭建。如果想用`Jekyll`的可以参考这个网站：http://jekyllcn.com/ ，此站目标是**成为 Jekyll 的全面指南**，内容详细，值得看一看。

#### hexo搭建大致流程

大致参考了以下几篇博客

https://blog.csdn.net/jinxiaonian11/article/details/82900119（这个作者有两篇这方面的博客，讲的挺详细的）

https://blog.csdn.net/qq_37210523/article/details/80909983

1. 选好一个空文件夹，最好不要有中文路径，之后用bash cd到这个路径下，继续下面的操作。

2. 安装配置好node.js（node.js下载地址：https://nodejs.org/en/download/），hexo (`npm install hexo -g`)。

   npm安装hexo时可能会出问题，更换一下镜像就好了。（如：`npm config set registry http://registry.npm.taobao.org`

3. 安装必要的组件：`npm install`，之后输入处理命令 `hexo g`   并开启服务器`hexo s` 出现了能够打开本地网页的提示（这个打开和那个线上的是一样的效果）就说明已经成功了，之后`ctrl+C`退出继续下面步骤

4. 在github上新增仓库，命名为: username.github.io，并在本地的根目录下的 _config.yml 文件中的**Deployment**设置处，添加以下代码，**repo**和**branch**根据自己情况修改

```yaml
deploy:
  type: git
  repo: git@github.com:BUAADreamer/BUAADreamer.github.io.git 
  branch: main 
```

5. 之后选好主题，并利用作者提供的方式git clone到本地theme文件夹下即可，并在根目录下的)_config.yml改一下theme设置，很多人推荐next，我的这个用的是ocean，也看到有同学用的黄玄大神的https://huangxuan.me/  

   然后创建博客：`hexo new blog-name`，之后一行代码部署到github上即可：`hexo d -g`，再打开自己的网站：username.github.io即可看到了！

6. 后续配置，这个每个主题都有区别，去github上看作者的文档就行。可以添加很多功能。下面记录一下常用的几个tips和资源

   * 修改文章：直接修改之后用hexo d- g就可以完成push的操作。

   * 设置摘要：使用`<!--more-->`标签设置摘要。这样在index页只会展示这行代码之前的文字

   * hexo官方中文文档：https://hexo.io/zh-cn/docs/


#### 问题和个人用法总结

1. 图片用的腾讯云对象存储
2. md文件配置时一定是类似`tags: 123`这样的格式。`tags`和`123`之间**一定要有空格**！！！

#### 评论功能

我的博客使用的`Utterances`，配置非常简单，并且也比较安全简洁。

可以参考这篇文章：https://roife.github.io/2021/02/12/use-utterances-for-comment/

其他的还有`Gitalk`,`Valine`，也可以用，但是`Gitalk`配置麻烦且很容易挂，后者貌似需要花钱。

#### 更换代理

同时参考下面这**两篇**文章，因为都有些不全面。记得要实名认证+获取证书+域名解析

https://blog.csdn.net/obsession753/article/details/84110360

https://blog.csdn.net/i042416/article/details/89926005

#### 2021.4.24

更换了主题，换成了感觉更美观的huxblog主题

更换过程很简单，只需要到https://github.com/Kaijun/hexo-theme-huxblog 这个网站下载代码，之后将themes里的huxblog文件夹copy到你自己的themes目录下，然后在根目录下的_config.xml修改themes名字为huxblog即可。





