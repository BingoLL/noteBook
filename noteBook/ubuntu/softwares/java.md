## 如何在Ubuntu 16.04上正确安装Oracle Java

### 查看已安装软件名

`dpkg -l | grep jdk`

### 第一步: 将webupd8team存储库添加到apt

我们将使用命令add-apt-repository通过以下命令添加webupd8team存储库：
```
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
```


### 第二步: 安装 Java

现在我们可以安装Java 6,7或8检查这个包列表：
```
oracle-java6-installer
oracle-java7-installer
oracle-java8-installer

    # 选择一个包并安装它
sudo apt-get install oracle-java8-installer
```

### 第三步: 选择Oracle Java作为默认JVM
`sudo apt-get install oracle-java7-set-default`

检查版本
java -version
javac -version
