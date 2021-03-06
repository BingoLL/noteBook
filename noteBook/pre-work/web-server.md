### 局域网内访问网站
Django 网站启动时候 python manage.py runserver 0.0.0.0:8000
#### UFW防火墙简单设置

安装: `sudo apt install ufw`

启用:
```
sudo ufw enable
sudo ufw default deny
```

防火墙重启:
`sudo ufw reload`

关闭: `sudo ufw disable`
查看防火墙状态: `sudo ufw status`

开启/禁用相应端口或服务举例:
```
sudo ufw allow 80 允许外部访问80端口
sudo ufw delete allow 80 禁止外部访问80 端口
sudo ufw allow from 192.168.1.1 允许此IP访问所有的本机端口
sudo ufw deny smtp 禁止外部访问smtp服务
sudo ufw delete allow smtp 删除上面建立的某条规则
sudo ufw deny proto tcp from 10.0.0.0/8 to 192.168.0.1 port 22 要拒绝所有的TCP流量从10.0.0.0/8 到192.168.0.1地址的22端口

# 可以允许所有RFC1918网络（局域网/无线局域网的）访问这个主机（/8,/16,/12是一种网络分级）：
sudo ufw allow from 10.0.0.0/8
sudo ufw allow from 172.16.0.0/12
sudo ufw allow from 192.168.0.0/16 
```

推荐设置
```
sudo apt-get install ufw
sudo ufw enable
sudo ufw default deny 
```
这样设置已经很安全，如果有特殊需要，可以使用sudo ufw allow开启相应服务。 