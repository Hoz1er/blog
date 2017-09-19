---
title: React - 使用create-react-app快速创建和部署
tags:
  - React
  - GitHub Pages
---

>更多信息请查阅`create-react-app`生成的模板项目中的`README.md`

## 创建React项目  

### Step 1:  
`npm install -g create-react-app`全局安装create-react-app
### Step 2:  
`create-react-app my-app`快速生成React模板项目
### Step 3:  
进入目录并预览项目
```sh
  cd my-app
  npm start
  ```
### Step 4:  
此时访问`http://localhost:300`即可预览项目  

## React部署 - Gtithub Pages  
<!-- more -->
### Step 1:  
在`package.json`中添加`homepage`配置
```js
"homepage":"https://myusername.github.io/my-app"
//如果为Github Pages添加自定义域名则为
"homepage":"http(s)://mydomian/"
```
### Step 2:  

安装`gh-pages`:
```sh
npm install --save gh-pages
yarn add gh-pages
```
### Step 3:  
在`package.json`的`scripts`中添加`deploy`,即下面两行

```diff
  "scripts": {
+   "predeploy": "npm run build",
+   "deploy": "gh-pages -d build",
    "start": "react-scripts start",
    "build": "react-scripts build",
```

### Step 4:  
使用 `npm run deploy`部署至`github`
```sh
npm run deploy
```

### Step 5:  
确保项目的 `gh-pages`分支中Github Pages设置无误

### Step 6:  
自定义域名的`CNAME`文件  
在`public/`目录下添加`CNAME`文件，部署时自动上传至`gh-pages`分支