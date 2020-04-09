---
layout: post
title: 将自己的公共类发布到Nuget
categories: [Nuget]
description: 将自己的公共类发布到Nuget
keywords: Nuget
typora-root-url: ..\..
---

   经常在Nuget找自己需要的包，最近自己写了一个 Excel导入导出公共类库，就想着怎样也把它弄上去呢？

说干就干，自然首先还是谷歌大法。

网上一搜，发现其实微软早就把文档写好了   [友情链接](https://docs.microsoft.com/en-us/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli ) 

不过既然是学习，我这里还是再走一遍加深印象

### 1.创建一个类库项目

这是不用讲的

### 2.设置属性

这一步很重要，所属权信息就在这里设置啦

![](/images/blog/Nuget/pack1.PNG)

### 3.将项目打包

打开 程式包管理控制台  在该类库下运行   dotnet pack 命令 将项目打包

![](/images/blog/Nuget/runpack.PNG)

生成一个ExcelCommon.1.0.0.nupkg 文件  这就是我们的打包文件了

项目已经打包了，我们就只要将这个打包文件推送到nuget上就ok了，怎么操作呢？看下一步

### 4. 创建  nuget.org 账户

用你的微软账户就ok了，没有就注册吧！

![](/images/blog/Nuget/WEBNUGET.PNG)

生成密钥，KeyName就写项目名，GlobPattern 不是很清楚就填 *号就ok

![](/images/blog/Nuget/key.png)

选好后生成，在Manage下面就有钥匙形状的项目，点击Copy 获取密钥，如果失效就要点击Regenerate重新生成

![](/images/blog/Nuget/qs_create-02-apikey.png)

### 5.将项目发布到Nuget

先前第三步我们获取了 ExcelCommon.1.0.0.nupkg文件，在程序控制台中cd 进入打包文件所在目录

运行以下命令,把包名和密钥改为自己的

```
dotnet nuget push ExcelCommon.1.0.0.nupkg -k qz2jga8pl3dvn2akksyquwcs9ygggg4exypy3bhxy6w6x6 -s https://api.nuget.org/v3/index.json
```

当出现下图所示则代表成功

![](/images/blog/Nuget/Nuget.png)

这时候再登录你的Nuget网站就可以看到已经上传上去了

![](/images/blog/Nuget/nugetweb.png)

使用VS的Nuget包管理也可以搜索到自己上传的包，上传信息也是自己了，可以直接使用

![](/images/blog/Nuget/Nuget1.0.png)

