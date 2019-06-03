---
title: nginx配置
tags:
    - nginx
    - linux
    - 随笔
---

## 设置请求Body的大小  
##### nginx 默认Body大小为 1M 
```config
client_max_body_size 20M;
```
可以选择在http{ }/server{ }/location{ }中设置  
三者到区别是：http{} 中控制着所有nginx收到的请求。而报文大小限制设置在server｛｝中，则控制该server收到的请求报文大小，同理，如果配置在location中，则报文大小限制，只对匹配了location 路由规则的请求生效。

>[>参考此文<](https://blog.csdn.net/li396864285/article/details/53522828)