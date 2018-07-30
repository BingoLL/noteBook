### Centos7 yum安装Python3.6环境，超简单

配置好Python3.6和pip3
安装EPEL和IUS软件源

```
yum install epel-release -y
yum install https://centos7.iuscommunity.org/ius-release.rpm -y
```

安装Python3.6

```
yum install python36u -y
```

创建python3连接符

```
ln -s /bin/python3.6 /bin/python3
```

安装pip3

```
yum install python36u-pip -y
```

创建pip3链接符

```
ln -s /bin/pip3.6 /bin/pip3
```

### 安装配置虚拟环境 virtualenvwrapper

```
sudo `which pip2.7` install virtualenvwrapper
sudo find / -name virtualenvwrapper.sh
```
配置

```
sudo vim /etc/profile 

export WORKON_HOME=~/.virtualenvs
mkdir -p $WORKON_HOME
source /usr/local/bin/virtualenvwrapper.sh
```
记得执行如下命令

```
source /etc/profile
```

`mkvirtualenv env1` 即可创建python3独立环境

http://virtualenvwrapper.readthedocs.io/en/latest/
