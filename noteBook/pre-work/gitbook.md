### 注意:本机系统环境 lubuntu 16.04 LTS 内核
gitbook 网站国内可以正常浏览,但是不能注册账户和登录账户,首次需要用vpn创建账户并且与github项目关联,关联后,gitbook会自动同步github仓库内的代码,无需再翻墙查看,gitbook默认免费用户可以创建一个隐私空间和公开逛空间.

如果本地没有安装nodejs和npm,请键入下面命令安装配置

```
sudo su
wget -qO- https://deb.nodesource.com/setup | bash -
exit
sudo apt install -y nodejs
sudo apt install -y npm
sudo ln -s /usr/bin/nodejs /usr/bin/node
```


下面安装gitbook,依次输入下面几条命令
```
sudo npm install gitbook -g
sudo npm install gitbook-cli -g
gitbook -V
```
这样,gitbook就已经安装好了.

gitbook常用命令
```
gitbook init //初始化目录文件
gitbook help //列出gitbook所有的命令
gitbook --help //输出gitbook-cli的帮助信息
gitbook build //生成静态网页
gitbook serve //生成静态网页并运行服务器
gitbook build --gitbook=2.0.1 //生成时指定gitbook的版本, 本地没有会先下载
gitbook ls //列出本地所有的gitbook版本
gitbook ls-remote //列出远程可用的gitbook版本
gitbook fetch 标签/版本号 //安装对应的gitbook版本
gitbook update //更新到gitbook的最新版本
gitbook uninstall 2.0.1 //卸载对应的gitbook版本
gitbook build --log=debug //指定log的级别
gitbook builid --debug //输出错误信息
```

