## Django网站部署实例

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

## windows server 部署Django

1. 安装IIS

开始-管理工具-服务器管理器-添加角色-下一步，勾选Web服务器(IIS)-添加角色服务，多添加几个吧，不然网站运行会报错

2. 配置安装

默认已经安装好python pip
```
安装wfastcgi
pip install wfastcgi 安装
wfastcgi-enable 这个命令是启动 会输出一个目录路径
```
配置web.config
在项目根目录 即manage.py同级目录 新建文本文件web.config
模板如下
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <add name="Python FastCGI" 
            path="*" 
            verb="*" 
            modules="FastCgiModule" 
            scriptProcessor="<Path to Python>\python.exe|<Path to Python>\lib\site-packages\wfastcgi.py" 
            resourceType="Unspecified" 
            requireAccess="Script"/>
        </handlers>
    </system.webServer>
    <appSettings>
        <add key="WSGI_HANDLER" value="django.core.wsgi.get_wsgi_application()" />
        <add key="PYTHONPATH" value="<Path to Django App>" />
        <add key="DJANGO_SETTINGS_MODULE" value="<Django App>.settings" />
    </appSettings>
</configuration>

```
criptProcessor的值，要改为前文说过的运行wfastcgi输出的那个值。
PYTHONPATH的value要改为manager.py的那个目录，也就是你项目的根目录例如：“C:\Users\Administrator\PycharmProjects\mydj"。DJANGO_SETTINGS_MODULE的value中的<Django App>要改为你的项目名 例如：”mydj“。

此时在iis中新建一个网站 目录就设置为项目根目录
应该就可以浏览了

css，js等文件获取不到 就在static目录下放一个web.config 其中name与前面相同

```

<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <handlers>
            <remove name="Python FastCGI" />
        </handlers>
    </system.webServer>
</configuration>
```
ettings里设置有问题
可以用manage.py collectstatic 把静态文件归拢 然后替换你的现在的static目录

若出现500.19 权限不足的问题
就在项目根目录右击-属性-安全-编辑-添加-输入Everyone-确定-（修改打钩） 重启网站就可以解决了
至此 发布完毕


### css静态文件仍然无法加载的解决办法

1. 项目应用下的static文件下的web.config文件中<handlers>下添加
```
<clear/>
      <add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="File" requireAccess="Read" />
```

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <system.webServer>
        <!-- this configuration overrides the FastCGI handler to let IIS serve the static files -->
        <handlers>
            <clear/>
            <add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="File" requireAccess="Read" />
        </handlers>
    </system.webServer>
</configuration>

```

2. IIS管理器，选定网站项目，右击添加虚拟路径
填写虚拟路径的别名和路径，别名一般为static，路径则是该网页app下的static文件夹，也就是刚才创建web.config的文件夹
重启IIS，应该问题会解决

