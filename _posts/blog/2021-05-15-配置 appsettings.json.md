---
layout: post
title: 通過環境變量 配置 不同 狀態下 appsettings.json
categories: [.NetCore]
description: some word here
keywords: NetCore
---



### 通過環境變量 配置 不同 狀態下 appsettings.json

1. #### **新增 appsettings.json**

   > ```
   > appsettings.json
   >     appsettings.Development.json  
   >     appsettings.Test.json
   > ```

   說明: 读取我们的配置文件回显读取 appsettings.json 并设置为optional: false，表示该配置为必要的配置；再往下继续读取再读取 appsettings.{env.EnvironmentName}.json 文件。当加载遇到相同的Key那么就会覆盖掉前面的配置项。

   `命名方式：appsettings.+環境變量+.json`

2. #### **Program 起始類 代碼配置**

   > ```
   >       await Host.CreateDefaultBuilder(args)
   >                 .ConfigureAppConfiguration((hostContext,config) =>
   >                 {
   >                     var env = hostContext.HostingEnvironment;
   >                     config.SetBasePath(Path.Combine(env.ContentRootPath))
   >                     .AddJsonFile(path: "appsettings.json", optional: false, reloadOnChange: true)
   >                     .AddJsonFile(path: $"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
   >                 })
   > ```

SetBasePath：设置配置的目录位置，如果是放在不同目录，再把路径换掉即可。
AddJsonFile：
path：文件的路径位置。
optional：如果是必要的配置文件，可选就要设定为false，当文件不存在就会引发 FileNotFoundException。
reloadOnChange：如果文件被更新，就同步更新 IConfiguration 实例的值。

1. #### 環境設置

- IIS

  web.config 配置环境变量

  > ```
  > <?xml version="1.0" encoding="utf-8"?>
  > <configuration>
  >   <system.webServer>
  >     <handlers>
  >       <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
  >     </handlers>
  >     <aspNetCore processPath="dotnet" arguments=".\Demo.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\stdout">
  >       <environmentVariables>
  >         <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Test" />
  >       </environmentVariables>
  >     </aspNetCore>
  >   </system.webServer>
  > </configuration>
  > ```

- Visual Studio Code

  launch.json中配置ASPNETCORE_ENVIRONMENT

  > ```
  > {
  >    "version": "0.1.0",
  >    "configurations": [
  >         {
  >             "name": ".NET Core Launch (web)",
  >             "type": "coreclr",
  >             "env": {
  >                 "ASPNETCORE_ENVIRONMENT": "Development"
  >             }
  >         }
  >     ]
  > }
  > ```

- Visual Studio

  Properties\launchSettings.json

  > ```
  > "profiles": {
  >     "IIS Express": {
  >       "commandName": "IISExpress",
  >       "launchBrowser": true,
  >       "environmentVariables": {
  >         "ASPNETCORE_ENVIRONMENT": "Test"
  >       }
  >     },
  >    }
  > ```

- Docker

  Dockerfile

  > ```
  >      FROM base AS final
  >      WORKDIR /app
  >      ENV ASPNETCORE_ENVIRONMENT=Development
  >      COPY --from=publish /app/publish .
  >      ENTRYPOINT ["dotnet", "MEDAS.IdentityServer4Admin.dll"]
  > ```
