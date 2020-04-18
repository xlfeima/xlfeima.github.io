---
layout: wiki
title: 优秀的插件项目
categories: Nuget
description: 好的Nuget插件
keywords: Nuget
---

## RestSharp

RestSharp是一个轻量的，不依赖任何第三方的组件或者类库的Http的组件。RestSharp具体以下特性；

1、[支持.NET](http://xn--ruu81c.net/) 3.5+,Silverlight 4, Windows Phone 7, Mono, MonoTouch, Mono for Android, Compact Framework 3.5等
　　2、通过NuGet方便引入到任何项目 ( Install-Package restsharp )
　　3、可以自动反序列化XML和JSON
　　4、支持自定义的序列化与反序列化
　　5、自动检测返回的内容类型
　　6、支持HTTP的GET, POST, PUT, HEAD, OPTIONS, DELETE等操作
　　7、可以上传多文件
　　8、支持oAuth 1, oAuth 2, Basic, NTLM and Parameter-based Authenticators等授权验证等
　　9、支持异步操作
　　10、极易上手并应用到任何项目中

以上是RestSharp的主要特点，通用它你可以很容易地用程序来处理一系列的网络请求（GET, POST, PUT, HEAD, OPTIONS, DELETE），并得到返回结果

## HealthCheck

报告外部依赖项的健康状况  .NetCore
AspNetCore.Diagnostics.HealthChecks
程序运行需要内存， cpu，需要磁盘空间， 现在的网站更是依赖于各种各样的第三方系统， 比如：数据库， 缓存等等。这些东西如果不正常，我们的网站也不可能正常运行。好消息是， ASPNetCore提供了大量的辅助类型，来提供这些系统是否正常：

```
首先需要安装下面的nuget包
Install-Package AspNetCore.HealthChecks.System
Install-Package AspNetCore.HealthChecks.Network
Install-Package AspNetCore.HealthChecks.SqlServer
Install-Package AspNetCore.HealthChecks.MongoDb
Install-Package AspNetCore.HealthChecks.Npgsql
Install-Package AspNetCore.HealthChecks.Elasticsearch
Install-Package AspNetCore.HealthChecks.Solr
Install-Package AspNetCore.HealthChecks.Redis
Install-Package AspNetCore.HealthChecks.EventStore
Install-Package AspNetCore.HealthChecks.AzureStorage
Install-Package AspNetCore.HealthChecks.AzureServiceBus
Install-Package AspNetCore.HealthChecks.AzureKeyVault
Install-Package AspNetCore.HealthChecks.Azure.IoTHub
Install-Package AspNetCore.HealthChecks.MySql
Install-Package AspNetCore.HealthChecks.DocumentDb
Install-Package AspNetCore.HealthChecks.SqLite
Install-Package AspNetCore.HealthChecks.RavenDB
Install-Package AspNetCore.HealthChecks.Kafka
Install-Package AspNetCore.HealthChecks.RabbitMQ
Install-Package AspNetCore.HealthChecks.IbmMQ
Install-Package AspNetCore.HealthChecks.OpenIdConnectServer
Install-Package AspNetCore.HealthChecks.DynamoDB
Install-Package AspNetCore.HealthChecks.Oracle
Install-Package AspNetCore.HealthChecks.Uris
Install-Package AspNetCore.HealthChecks.Aws.S3
Install-Package AspNetCore.HealthChecks.Consul
Install-Package AspNetCore.HealthChecks.Hangfire
Install-Package AspNetCore.HealthChecks.SignalR
Install-Package AspNetCore.HealthChecks.Kubernetes
Install-Package AspNetCore.HealthChecks.Gcp.CloudFirestore

```

 