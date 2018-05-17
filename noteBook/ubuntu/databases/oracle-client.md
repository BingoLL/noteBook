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

查看当前用户下所有的表:

ORACLE下有三个视图

DBA_TABLES  拥有DBA角色的用户可以查看系统中的所有表

USER_TABLES 登录数据库的当前用户拥有的所有表

ALL_TABLES 登录数据库的当前用户有权限查看的所有表

select table_name from all_tables;