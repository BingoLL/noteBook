### 安装MySQL-python

```
sudo apt install python-dev libmysqlclient-dev
pip install MySQL-python==1.2.5
```

### 完全卸载MySQL
```
sudo apt purge mysql-*
sudo rm -rf /etc/mysql/ /var/lib/mysql
sudo apt autoremove
sudo apt autoreclean
```

### 安装
安装
`sudo apt install mysql-server mysql-client`

然后按照提示输入
### 管理服务
启动
`service mysql start`

停止
`service mysql stop`

重启
`service mysql restart`

允许远程连接
找到mysql配置文件并修改
```
sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
将bind-address=127.0.0.1注释
```
登录mysql，运行命令
```
grant all privileges on *.* to 'root'@'%' identified by 'mysql' with grant option;
flush privileges;
```
记得重启mysql

登录
`mysql -uroot -p`

登录成功后，输入如下命令查看效果
```
查看版本：select version();
显示当前时间：select now();
```

远程连接

`mysql -hip地址 -uroot -p`

- h后面写要连接的主机ip地址
- u后面写连接的用户名
- p回车后写密码

### 数据库操作
创建数据库
`create database 数据库名 charset=utf8;`

删除数据库
`drop database 数据库名;`

切换数据库
`use 数据库名;`

查看当前选择的数据库
`select database();`

### 表操作
查看当前数据库中所有表
`show tables;`

创建表

auto_increment表示自动增长
```
create table 表名(列及类型);
如：
create table students(
id int auto_increment primary key,
sname varchar(10) not null
);
```
修改表
```
alter table 表名 add|change|drop 列名 类型;
如：
alter table students add birthday datetime;
```

删除表
`drop table 表名;`

查看表结构
`desc 表名;`

更改表名称
`rename table 原表名 to 新表名;`

查看表的创建语句
`show create table '表名';`

### 数据操作
查询
`select * from 表名`

增加
```
全列插入：insert into 表名 values(...)
缺省插入：insert into 表名(列1,...) values(值1,...)
同时插入多条数据：insert into 表名 values(...),(...)...;
或insert into 表名(列1,...) values(值1,...),(值1,...)...;
```
主键列是自动增长，但是在全列插入时需要占位，通常使用0，插入成功后以实际数据为准

修改
`update 表名 set 列1=值1,... where 条件`

删除
`delete from 表名 where 条件`

逻辑删除，本质就是修改操作update
```
alter table students add isdelete bit default 0;
如果需要删除则
update students isdelete=1 where ...;
```
### 备份与恢复
数据备份
```
# 进入超级管理员
sudo su
# 进入mysql库目录
cd /var/lib/mysql
# 运行mysqldump命令
mysqldump –uroot –p 数据库名 > ~/Desktop/备份文件.sql;
# 按提示输入mysql的密码
```
### 数据恢复
连接mysqk，创建数据库

退出连接，执行如下命令
```
mysql -uroot –p 数据库名 < ~/Desktop/备份文件.sql
根据提示输入mysql密码
```


### 如果安装时候未提示输入密码，后期登陆可能会出现问题。
```
bingo@bingo-JinMai:~$ mysql
ERROR 1045 (28000): Access denied for user 'bingo'@'localhost' (using password: NO)
bingo@bingo-JinMai:~$ mysql -uroot -p
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
bingo@bingo-JinMai:~$ mysql -uroot -p
Enter password: 
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```


### 解决办法

1. 打开/etc/mysql/mysql.conf.d/mysqld.cnf 文件,命令如下

   ` $ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf`

2. 找到[mysqld]段，并加入一行“skip-grant-tables”,如下图，

3. 重启mysql服务,用空密码进入mysql管理命令行，切换到mysql库,操作命令如下，
```
    $ mysql

        Welcome to the MySQL monitor.  Commands end with ; or \g. 

    mysql> use mysql
        Reading table information for completion of table and column names
        You can turn off this feature to get a quicker startup with -A
        Database changed

    mysql> update mysql.user set authentication_string=password('newpass') where user='root' and Host ='localhost';
        Query OK, 1 row affected, 1 warning (0.00 sec)
        Rows matched: 1  Changed: 1  Warnings: 1

    mysql> update user set plugin="mysql_native_password"; 
        Query OK, 0 rows affected (0.00 sec)
        Rows matched: 3  Changed: 0  Warnings: 0

    mysql> flush privileges;
        Query OK, 0 rows affected (0.01 sec)

    mysql> quit;
        Bye
```

4. 回到sudo vi  /etc/mysql/mysql.conf.d/mysqld.cnf，把刚才加入的那一行“skip-grant-tables”注释或删除掉。

5. 再次重启mysql服务sudo service mysql restart，使用新的密码登陆，修改成功。
```
    $ mysql -u root -p new_pass  
        Welcome to the MySQL monitor.  Commands end with ; or \g.  
    mysql> 
```
6. 至此，问题解决

## 创建用户，并赋予权限
```
mysql -uroot -p

create user 'test'@'localhost' identified by '123456';  
create user 'test'@'%' identified by '123456'; 
flush privileges; 
grant all on *.* to test@'%' identified by "123456";
```

## mysql identified怎么用

1. 用管理员登陆mysql；
2. 创建数据库create database db01；
3. 添加用户和用户权限：
### Mysql添加用户
使用可以对mysql数据库用户表有操作权限的用户名登陆mysql
`insert into user(Host,User,Password) values('%','name','password');`
如果work用户没有登陆权限，则
```
killall mysqld
share/mysql/mysql.server start
grant all on *.* to work@'%' identified by "password";
```
`
MySQL赋予用户权限的命令的简单格式为
```
grant 权限 on 数据库对象 to 用户
grant 权限 on 数据库对象 to 用户 identified by "密码"
```

用户名:ad,密码：ad_pass，登陆ip:192.168.0.10
//用户在所有登陆ip的权限
`grant all on *.* to ad@‘%’ identified by "ad_pass";`


//开放管理MySQL中所有数据库的权限
`grant all on *.* to ad@'192.168.0.10' identified by "ad_pass";`


//开放管理MySQL中具体数据库(test)的权限
`grant all privileges on test to ad@'192.168.0.10' identified by "ad_pass";`
或
`grant all on test to ad@'192.168.0.10' identified by "ad_pass";`


//开放管理MySQL中具体数据库中的表(test.table1)的权限
`grant all on test.table1 to ad@'192.168.0.10' identified by "ad_pass"`


//开放管理MySQL中具体数据库的表(test.table1)的部分列的权限
`grant select(id,se,rank) on test.table1 to ad@'192.168.0.10' identified by "ad_pass";`


//开放管理操作指令
`grant select,insert,update,delete on test.* to ad@'192.168.0.10' identified by "ad_pass";`


//回收权限
`revoke all on *.* from ad@localhost;`


//查看MySQL用户权限
```
show grants;
show grants for ad@localhost;
```

//MySQL5.7 修改用户密码
```
update mysql.user set authentication_string=PASSWORD('newpassword') where user='username';
```
