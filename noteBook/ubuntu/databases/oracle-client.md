## oracle client

http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html

```
oracle-instantclient12.2-basic-12.2.0.1.0-1.x86_64.rpm
oracle-instantclient12.2-sqlplus-12.2.0.1.0-1.x86_64.rpm
oracle-instantclient12.2-devel-12.2.0.1.0-1.x86_64.rpm


alien -i ./*.rpm 

sqlplus username/password@//dbhost:1521/SID
# 如果报错,/sqlplus: error while loading shared libraries: libsqlplus.so: cannot open shared object file: No such file or directory


环境变量  .bashrc文件内添加

export ORACLE_HOME=/usr/lib/oracle/12.2/client64
export PATH=$PATH:$ORACLE_HOME/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH

运行 source .bashrc 使其生效

创建 #mkdir -p /usr/lib/oracle/12.2/client64/network/admin/

再次链接数据,应该就可以正常了.

```


## 在Ubuntu 16.04下安装Oracle数据库客户端，使Django项目连接到远程Oracle数据库。

1. 下载oracle客户端安装包：

　　进入官网http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html下载如下所需的三个包。

　　oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm
　　oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm
　　oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm
2. 使用alien安装

　　下载alien（alien可以把rpm格式转为dev格式进行rpm包的安装）：
    ```
　　sudo apt-get install alien
　　进行安装

　　sudo alien -i oracle-instantclient11.2-basic-11.2.0.4.0-1.x86_64.rpm
　　sudo alien -i oracle-instantclient11.2-devel-11.2.0.4.0-1.x86_64.rpm
　　sudo alien -i oracle-instantclient11.2-sqlplus-11.2.0.4.0-1.x86_64.rpm

   alien -i ./*.rpm
   ```
　　安装所需要的库：

　　sudo apt-get install libaio1

3. 配置环境

　　①打开/etc/ld.so.conf文件
```
　　sudo gedit /etc/ld.so.conf
　　添加内容： /usr/lib/oracle/11.2/client64/lib/
　　执行/sbin/ldconfig命令，使其生效

　　sudo /sbin/ldconfig
　　打开/etc/profile文件

　　sudo gedit /etc/profile
　　②添加如下环境变量

　　export ORACLE_HOME=/usr/lib/oracle/11.2/client64
　　export ORACLE_BASE=/usr/lib/oracle/11.2
　　export LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:$ORACLE_HOME/lib
　　export PATH=$PATH:$ORACLE_HOME/bin

　　执行命令source /etc/profile， 使其生效
```
4. Django连接oracle， 还需要库cx_Oracle.

　　①在Django项目所用的虚拟环境下，安装cx_Oracle
```
　　pip install cx_Oracle-5.2.1
　　②Django项目下的settings.py里配置oracle相关参数

　　DATABASES = {
    　　'default': {
        　　'ENGINE': 'django.db.backends.oracle',
        　　'NAME': 'database_name',
        　　'USER': 'username',
        　　'PASSWORD': 'your_pwd',
        　　'HOST': 'your_remote_addr',
        　　'PORT': 'remote_port',
    　　}
　　} 
```
　　至此，Django可以成功连接oracle数据库。


查看当前用户下所有的表:

ORACLE下有三个视图

DBA_TABLES  拥有DBA角色的用户可以查看系统中的所有表

USER_TABLES 登录数据库的当前用户拥有的所有表

ALL_TABLES 登录数据库的当前用户有权限查看的所有表

select table_name from all_tables;


