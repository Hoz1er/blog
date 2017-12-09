---
title: Golang笔记，不定期更新
tags:
  - Go 
  - Linux
---

>## 在Linux服务器上部署GO项目

### 上传源码(test.go)至linux服务器上的%GOPATH%/src/test(新建文件夹)目录中
### 编译
```bash
go build test
```
### 后台运行GO项目
```bash
nohup ./test & >test.log &
或直接
nohup ./test & 
```

<!--More-->