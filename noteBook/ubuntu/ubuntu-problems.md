# 问题解决办法

1. 图形几面卡死：
Ctrl + Alt + F1 转到tty1
```
ps  all # 查看进程,找到Xorg进程到PID号 xxx
sudo  kill xxx
```

``` 
ps -t -tty7 查看进程,找到Xorg进程到PID号 xxx
kill xxx
```
