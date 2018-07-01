### shell script

```
#!/bin/bash
echo "Hello world"
...
```

交互式自动输入密码命令等，需要借助expect

1. 安装expect

```
ls /usr/bin | grep expect 
如果显示为空，说明未安装expect，使用以下命令安装

sudo apt install tcl tk expect
```
2. 安装好，编写的shell脚本如下，需用expect运行

```
#!/usr/bin/expect
spawn ssh bingo@127.0.0.2
expect "*password:"
send "yourpassword\r"
expect "*#"
interact
```


