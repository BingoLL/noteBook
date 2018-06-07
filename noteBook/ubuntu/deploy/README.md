### Django网站部署实例

今日（2018-05-07，周一晚）部署网站到服务器，特此记录。

> 注：本次部署的阿里云服务器是重置过之后的，重置之前pip3总是出问题，发现重置后依然存有问题。重置之前是先创建了一个用户账号，然后使用sudo权限安装一些系统软件或者python包，为保险起见，重置后的系统使用root权限安装各种软件包。然后创建虚拟环境。

## root权限下操作
远程登陆
`ssh root@0.0.0.0` ssh 用户名@IP地质


### 解决pip3问题

```
locale 
locale: Cannot set LC_ALL to default locale: No such file or directory 

export LC_ALL=C 

LANG=en_US.UTF-8
LANGUAGE=
LC_CTYPE="C"
LC_NUMERIC="C"
LC_TIME="C"
LC_COLLATE="C"
LC_MONETARY="C"
LC_MESSAGES="C"
LC_PAPER="C"
LC_NAME="C"
LC_ADDRESS="C"
LC_TELEPHONE="C"
LC_MEASUREMENT="C"
LC_IDENTIFICATION="C"
LC_ALL=C
```

### 安装mysql数据库：
```
apt install mysql-server mysql-client # 输入数据库root密码染成安装。
mysql -uroot -p
mysql> create database ms charset=utf8;
mysql> create user 'ms'@'%' identified by '123456'; 
mysql> grant all privileges on *.* to 'root'@'%' identified by 'mysql' with option grant;
mysql> grant all privileges on ms.* to 'root'@'%' identified by '123456';
mysql> flush privileges;
```


### 创建具有sudo权限的ddvd用户，用来专门管理所要部署的网站

useradd --system --shell /bin/bash --home /home/ms

> 网上有中方法是创建用户组，然后为该用户组添加用户，由于本次部署服务器的网站项目较为简单，且维护人员较少，故仅创建用户，不创建用户组。

添加用户组并添加用户(假设用户组mms,用户名ms )
事先创建文件夹 /home/ms/
```
group --system mms
useradd --system --gid mms --shell /bin/bash --home /home/ms ms
```
命令解释：
--gid    指定用户组
--shell  用户使用shell的路径
--home   指定登陆后进入的文件夹

设置密码`passwd ms`

给用户添加执行权限`vim /etc/sudoers`,发现此文件root权限下只读，修改权限。不用管，添加ms权限同root，然后:wq!

然后在home文件夹下执行`chown ms ms/`，将文件夹ms赋给ms用户。

切换用户 ` sudo su - ms `

### 创建虚拟环境ms_env

`mkvirtualenv -p /usr/bin/python3 ms_env`

### /home/ms/ 下

```
git init
git remote add 。。。
git pull 。。。
```

## nginx uwsgi 部署

root权限下apt安装nginx，pip3安装uwsgi，

### /home/.ms_config/ 文件夹下用来配置uwsgi配置文件

创建nginx的配置文件
```
# mysite_nginx.conf

# the upstream component nginx needs to connect to
upstream django {
    # server unix:///path/to/your/mysite/mysite.sock; # for a file socket
    server 127.0.0.1:8001; # for a web port socket (we'll use this first)
}

# configuration of the server
server {
    # the port your site will be served on
    listen      8000;
    # the domain name it will serve for
    server_name example.com; # substitute your machine's IP address or FQDN
    charset     utf-8;

    # max upload size
    client_max_body_size 75M;   # adjust to taste

    # Django media
    location /media  {
        alias /path/to/your/mysite/media;  # your Django project's media files - amend as required
    }

    location /static {
        alias /path/to/your/mysite/static; # your Django project's static files - amend as required
    }

    # Finally, send all non-media requests to the Django server.
    location / {
        uwsgi_pass  django;
        include     /path/to/your/mysite/uwsgi_params; # the uwsgi_params file you installed
    }
}
```

然后软连接

`sudo ln -s ~/path/to/your/mysite/mysite_nginx.conf /etc/nginx/sites-enabled/`

重启nginx
`/etc/init.d/nginx restart`

对应的uwsgi_params
```
uwsgi_param  QUERY_STRING       $query_string;
uwsgi_param  REQUEST_METHOD     $request_method;
uwsgi_param  CONTENT_TYPE       $content_type;
uwsgi_param  CONTENT_LENGTH     $content_length;

uwsgi_param  REQUEST_URI        $request_uri;
uwsgi_param  PATH_INFO          $document_uri;
uwsgi_param  DOCUMENT_ROOT      $document_root;
uwsgi_param  SERVER_PROTOCOL    $server_protocol;
uwsgi_param  REQUEST_SCHEME     $scheme;
uwsgi_param  HTTPS              $https if_not_empty;

uwsgi_param  REMOTE_ADDR        $remote_addr;
uwsgi_param  REMOTE_PORT        $remote_port;
uwsgi_param  SERVER_PORT        $server_port;
uwsgi_param  SERVER_NAME        $server_name;
```

uwsgi.ini
```
[uwsgi]
socket= :8000
chdir=/home/ddvd/ddvd_web_deploy/Web_source/
module=ddvd.wsgi
processes=4
threads=2
master=True
pidfile=uwsgi.pid
daemonize=uwsgi.log

```





