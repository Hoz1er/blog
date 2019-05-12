---
title: .NET笔记
tags:
    - .NET
    - MVC
    - JSON
    - 随笔
---
# 特殊情况解决方案

## POST请求中Request数据过大，JSON反序列化时字符串的长度超过了为maxJsonLength属性设置的值

### 解决方案：

```XML
<!--web.config-->
<!--设置最大请求长度-->
<httpRuntime maxRequestLength="1048576" executionTimeout="1200" targetFramework="4.5" />
```  
```C#
//反序列化
public static T DeserializeRequest<T>()
{
    Stream req = System.Web.HttpContext.Current.Request.InputStream;
    req.Seek(0, System.IO.SeekOrigin.Begin);
    string json = new StreamReader(req).ReadToEnd();
    return JsonConvert.DeserializeObject<T>(json);
}

/***************使用示例***************/
public class MyModel{
    public string imgBase64{get;set;}
    public MyModel(){ }
}
public void UploadImg(){
    MyModel myModel = DeserializeRequest<MyModel>();
    /*
     * MyDeal(myModel);
     */
}


```

<!--More-->