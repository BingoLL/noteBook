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


### centos mysql8.0 修改密码方式有变化

root权限下

```
vi /etc/my.cnf
添加skip-grant-table

#systemctl stop mysqld.service

#systemctl start mysqld.service

重启数据库
# service mysqld restart
#mysql –u root

[敲回车进入]

mysql> flush privileges;

Query OK, 0 rows affected (0.00 sec)

mysql> alter user'root'@'%' IDENTIFIED BY 'MyNewPass@123'; 

 【mysql8.0以上密码策略限制必须要大小写加数字特殊符号，我之前用mysqladmin,set,update，参考修改root密码的前人：http://www.cnblogs.com/liufei88866/p/5619215.html】

 Query OK, 0 rows affected (0.05 sec)

 退出，把Skip-grant-table语句删除，重新启动数据库



```
