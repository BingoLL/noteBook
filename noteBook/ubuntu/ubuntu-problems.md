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

### Python pip3 - cannot import name 'main'

```
Traceback (most recent call last):
  File "/usr/bin/pip3", line 9, in <module>
    from pip import main
ImportError: cannot import name 'main'
```

解决办法 

`sudo vim /usr/bin/pip3`
```
import sys

# Run the main entry point, similarly to how setuptools does it, but because
# we didn't install the actual entry point from setup.py, don't use the
# pkg_resources API.
from pip import main
if __name__ == '__main__':
    sys.exit(main())
```

修改为
```
import sys

from pip import main
if __name__ == '__main__':
    sys.exit(__main__.main())

```
然后重启系统 `reboot now`
