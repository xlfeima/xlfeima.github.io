---
layout: post
title: .NET 開發利用FreeSql自動生成實體類
categories: [.netcore]
description: 利用FreeSql自動生成實體類
keywords: netcore, 自動生成
typora-root-url: ..\..
---

### 使用 FreeSql 快速生成数据库的实体类

#### 一.首先  在cmd   安裝  FreeSql.Generator

```
dotnet tool install -g FreeSql.Generator
```

#### 二. 瞭解 FreeSql.Generator --help的用法

```
目前只支持.netcore 3.1

    更新工具：dotnet tool update -g FreeSql.Generator


  # 快速开始 #

  > FreeSql.Generator -Razor 1 -NameOptions 0,0,0,0 -NameSpace MyProject -DB "MySql,Data Source=127.0.0.1;..."

     -Razor 1                  * 选择模板：实体类+特性
     
     -Razor 2                  * 选择模板：实体类+特性+导航属性
     
     -Razor "d:\diy.cshtml"    * 自定义模板文件
     
     -NameOptions              * 总共4个布尔值，分别对应：
                               # 首字母大写
                               # 首字母大写,其他小写
                               # 全部小写
                               # 下划线转驼峰
                               
     -NameSpace                * 命名空间
     
     -DB "MySql,Data Source=127.0.0.1;Port=3306;User ID=root;Password=root;Initial Catalog=数据库;Charset=utf8;SslMode=none;Max pool size=2"
     
     -DB "SqlServer,Data Source=.;Integrated Security=True;Initial Catalog=数据库;Pooling=true;Max Pool Size=2"
     
     -DB "PostgreSQL,Host=192.168.164.10;Port=5432;Username=postgres;Password=123456;Database=数据库;Pooling=true;Maximum Pool Size=2"
     
     -DB "Oracle,user id=user1;password=123456;data source=//127.0.0.1:1521/XE;Pooling=true;Max Pool Size=2"

     -DB "Sqlite,Data Source=document.db;Attachs=xxxtb.db;"
     
     -DB "OdbcDameng,Driver={DM8 ODBC DRIVER};Server=127.0.0.1:5236;Persist Security Info=False;Trusted_Connection=Yes;UID=USER1;PWD=123456789;Max pool size=2"
                               OdbcDameng 是国产达梦数据库，需要使用 ODBC 连接

     -Filter                   Table+View+StoreProcedure
                               默认生成：表+视图+存储过程
                               如果不想生成视图和存储过程 -Filter View+StoreProcedure

     -FileName                 文件名，默认：{name}.cs

     -Output                   保存路径，默认为当前 shell 所在目录
                               推荐在实体类目录创建 gen.bat，双击它重新所有实体类
```

[相關鏈接]: https://github.com/2881099/FreeSql.Tools

#### 三.創建一個文件夾,cmd 進入該文件夾 執行生成命令

![](/images/blog/Freesql/cmdfreesql.PNG)

#### 四.在該目錄下查看生成的實體類

![](/images/blog/Freesql/Models.PNG)

