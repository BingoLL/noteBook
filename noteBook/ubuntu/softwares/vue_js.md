### ubuntu 安装vue

需要安装nodejs 
我的电脑已经安装nodejs，但是我需要把它升级到最新稳定版 
node有一个模块叫n，是专门用来管理Node.js的版本的。

首先 安装n模块

`npm install-g n`

第二步，升级

`n stable`

安装vue

`sudo npm install -g vue`

安装vue-cli 命令行工具

`npm install -g vue-cli`

创建一个基于 webpack 模板的新项目

`sudo vue init webpack my-project`

```
sudo cd my-project
sudo  npm install
sudo npm run dev
```


### 解决npm update的权限问题

```
sudo chown -R bingo ~/.npm
sudo chown -R bingo ~/node_modules
```

