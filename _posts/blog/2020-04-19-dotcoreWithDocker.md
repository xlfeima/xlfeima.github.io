---
layout: post
title: 通过 Docker 发布 .NetCore 项目
categories: [Docker,NetCore]
description: 通过 Docker 发布 .NetCore 项目
keywords: Docker,NetCore
typora-root-url: ..\..
---

最近学习了一下容器相关知识，学习之前觉得应该挺难，学完之后 感觉容器部署真方便，真好用。

 [Docker官方文档](https://docs.docker.com/)

## Docker相关命令

### 1.删除容器

获取容器信息命令:

```
docker container ls -a     #获得容器信息
```

```
docker container ls -a  -p   #获得容器id
```

如果你要删除的 container 还是运行状态，那么就要先把容器停止了：

```
docker container stop <container ID>
```

找到需要删除的容器对应的  container ID 或者名字,进行删除:

```
docker container rm  <container ID>
```

批量停止容器:

```
docker container stop $(docker container ls -a -q)
```

批量删除容器:

```
docker container rm $(docker container ls -a -q)
```

### 2.删除镜像

删除镜像文件的方法和删除容器的方法是一样的,将删除容器命令中的 container 更改成 image 即可

一.

![](/images/blog/Docker/dockererro1.png)

这是由于镜像正在被容器使用,用命令停止容器,移除容器后,在移除镜像即可

二.

![](/images/blog/Docker/dockererro2.png)

并且确定此镜像未被容器使用,这是由于镜像被 repositories (仓库)引用,解决方法如下:

删除 Repository

被删除的 ImageID，这里存在1个 Repository 名字引用，解决方法如下：

![](/images/blog/Docker/dockererro3.png)

即删除时指定名称，而不是 IMAGE ID。

```
docker rmi <Repository:tag>
```

查看镜像,已删除

三.

出现以下错误:

```
Error response from daemon: conflict: unable to delete c9cde4658340 (cannot be forced) - image has dependent child images
```

这样的错误，原因是有另外的 image FROM 了这个 image，可以使用下面的命令列出所有在指定 image 之后创建的 image 的父 image

```
docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=xxxxxx)
```

解决方法:

其中 xxxxxx 是报错 image 的 id，在文章的例子中就是 c9cde4658340 。从列表中查找到之后就可以核对并删除这些 image,然后此镜像就可以移除了

四:  none镜像

有时运行下面命令会出现如下错误:

```
docker images -a
```

![](/images/blog/Docker/dockererro4.png)

并且这些虚悬镜像删不掉,解决办法:

```
docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=xxxxxx)
```

删除关联的依赖镜像，关联的none镜像也会被删除

或者

```
docker rmi $(docker images -q -f dangling=true)
```

直接移除所有的虚悬镜像

#### 拓展：

```
# 停止所有容器
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker stop
 
# 删除所有容器
docker ps -a | grep "Exited" | awk '{print $1 }'|xargs docker rm
 
# 删除所有 none 容器
docker images|grep none|awk '{print $3 }'|xargs docker rmi

```

## .NetCore 项目在 Docker 中发布运行

### 1.在项目中创建 Dockerfile 文件

```
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

### 2.创建并且运行 项目 Docker 镜像

一.打开项目所在文件夹

二.使用下面命令创建运行项目 Docker 镜像

```
$ docker build -t aspnetapp .
$ docker run -d -p 8080:80 --name myapp aspnetapp
```

三.在浏览器中访问 http://localhost:8080 访问项目