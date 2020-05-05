---
layout: post
title: 利用 RestSharp 调用 WebApi 接口
categories: [webapi]
description: 利用 RestSharp 调用 WebApi 接口
keywords: WebApi
typora-root-url: ..\..
---

​      之前都是用  WebApiClient.JIT 调用 WebApi接口，今天了解一下 RestSharp  这个插件 调用WebAPI的用法,留码备份

### 一. 首先安装 RestSharp

![](/images/blog/RestSharp/nuget.png)

### 二.代码示例

```
获取Token
var client = new RestClient("http://********/");
            var request = new RestRequest("connect/token", Method.POST);
            request.AddHeader("Content-Type", "application/x-www-form-urlencoded");
            request.AddParameter("grant_type", "client_credentials");
            request.AddParameter("client_id", "***");
            request.AddParameter("client_secret", "**");
            IRestResponse response = client.Execute(request);
            var content = response.Content;
            var token = Newtonsoft.Json.JsonConvert.DeserializeObject<RestTokenModel>(content);//token值
```

```
        //插入
            var client1 = new RestSharp.RestClient("******");
            string tki = "Bearer " + Convert.ToString(token.access_token);//添加token
            client1.AddDefaultHeader("Authorization", tki);
            var requestPost = new RestRequest("/api/Home/InsertMs", Method.POST);
            var TB = new IOTTEST
            {
                DEV_ID = "12341",
                DEV_STATUS = "運行",
                C1_STATUS = 1,
                C2_STATUS = 0,
                C3_STATUS = 1,
                C4_STATUS = 0,
                C1_VALUE = 12,
                C2_VALUE = 11,
                C3_VALUE = 33,
                C4_VALUE = 45
            };
            var json = JsonConvert.SerializeObject(TB);
            requestPost.AddParameter("application/json", json, ParameterType.RequestBody);
            IRestResponse responsePost = client1.Execute(requestPost);
            var contentPost = responsePost.Content;
```

```

            //查詢
            var queryresultclient = new RestClient("http://10.151.110.88:8099/api/Home/QueryMS");
            string tk = "Bearer " + Convert.ToString(token.access_token);//添加token
            queryresultclient.AddDefaultHeader("Authorization", tk);
            RestRequest queryresultrequest = new RestRequest() { Method = Method.POST };
            var queryresultresponse = queryresultclient.Execute(queryresultrequest);
            var queryresult=queryresultresponse.Content；
```

```

public class RestTokenModel{

  public string access_token { get;set; } 
  
  public string expires_in  { get;set; }
 
  public string token_type  { get;set; }

  public string scope  { get;set; }

}
```

