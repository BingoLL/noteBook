### git 服务器搭建

1. git安装，建议安装新版的，centos默认版本较低
2. 创建一个git 用户，不需要为这个用户创建密码，用来运行git服务

    ```
    adduser git
    su - git
    mkdir .ssh
    chmod 700 .ssh
    touch ~/.ssh/authorized_keys 
    chmod 644 ~/.ssh/authorized_keys
     
    vim ~/.ssh/authorized_keys

    ```
3. 初始化仓库项目
    ```
    su - git
    cd /home/git/
    mkdir gittest
    git init --bare gittest
    ```

4. 使用git-shell登录

    ```
    [root@BingoT ~]# which git-shell
    /usr/local/bin/git-shell
    那么上面的/etc/passwd文件就应该修改为：
    git:x:1002:1002:,,,:/home/git:/usr/local/bin/git-shell
    ```
5. 还要复制一个名为git-shell-commands的目录
    
    ```
    $ cp /usr/share/doc/git-1.7.4.4/contrib/git-shell-commands /home/git -R
    $ chown git:developers /home/git/git-shell-commands/ -R
    $ chmod +x /home/git/git-shell-commands/help
    $ chmod +x /home/git/git-shell-commands/list
    ```
6. 至此，git服务应该已经搭建完成了



### git 安装

1. Install Git on Linux
    By default Git included in all the Linux distro, it is easiest way to install Git on Linux using package manager.

    ```
    # Install Git to Debian based system #
    $ sudo apt-get install git

    # Install Git to RHEL/CentOS & Up to Fedora 21 system #
    $ sudo yum install git

    # Install Git to Fedora 22 & later system #
    $ sudo dnf install git

    # Install Git to Arch Linux system #
    $ sudo zyper install git

    # Install Git to openSUSE system #
    $ sudo pacman -S git
    ```

2. Install Git via PPA
    Alternatively we can install latest version of Git to Ubuntu/Mint systems via PPA.

    ```
    $ sudo apt-add-repository ppa:git-core/ppa
    $ sudo apt-get update
    $ sudo apt-get install git
    ```

3. Install Git on Linux from Source
    At this moment Linux Distribution repository having bit older version of Git, so i’m going to install from source archive.

    ```
    # Install Dependancy to RPM based system #
    $ sudo yum install curl-devel expat-devel gettext-devel openssl-devel zlib-devel gcc perl-ExtUtils-MakeMaker

    # Install Dependancy to Debian based system #
    $ sudo apt-get install libcurl4-gnutls-dev libexpat1-dev gettext libz-dev libssl-dev build-essential

    $ sudo wget https://www.kernel.org/pub/software/scm/git/git-2.8.0.tar.gz
    $ sudo tar -xzf git-2.8.0.tar.gz
    $ cd git-2.8.0
    $ sudo make configure
    $ sudo ./configure --prefix=/usr/local
    $ sudo make install

    # Install upgrade automatically in future #
    $ sudo git clone git://git.kernel.org/pub/scm/git/git.git
    ```

4. Checking Git Version

    ```
    $ git --version
    git version 2.8.0
    ```

### 分支branch

1. 查看分支
```
git branch
```

2. 以master分支数据为基础同步到bingo分支
```
git push ntb master:bingo
```

3. 推送到bingo分支
```
git push ntb bingo
```

4. 创建分支
```
git branch bingo
```

5. 切换分支
```
git checkout bingo
```

6. 版本退回
```
    git log

    commit c4a76e09cf5475fac3f168d66485de4a76e0d078 (HEAD -> bingo, pytb/bingo)
    Author: Bingo.Zhang <1358223058@qq.com>
    Date:   Wed Jun 27 09:20:26 2018 +0800

    mv?

    commit 37ec04681326fbf01783f0fdf33e37ae8652d406
    Author: Bingo.Zhang <1358223058@qq.com>
    Date:   Wed Jun 27 09:16:01 2018 +0800

    bingo_test.md

    commit 97809749c183b2ad60391ff766a73a388d3acf35
    Author: Bingo.Zhang <1358223058@qq.com>
    Date:   Wed Jun 27 09:07:52 2018 +0800

    test bingo branch

    commit cb29432215dbf1a5b876c4d1056c54970b30b6cf
    Author: Xu Yuan <nicai@unknow.com>
    Date:   Fri Jun 22 15:15:41 2018 +0800

    更新编译便脚本中的版本生成规则

    git reset --hard HEAD
    git reset --hard 978097
```


### github
##### 如果你已经有了一套名为 id_rsa 的公秘钥，将要生成另外一个公钥，比如 aysee ，你也可以使用任何你喜欢的名字。
##### 步骤如下：
1、生成一个新的自定义名称的公钥：

 `ssh-keygen -t rsa -C "YOUR_EMAIL@YOUREMAIL.COM" -f ~/.ssh/aysee`

 执行命令后，生成命名的公钥和生成默认公钥的步骤一样。

 执行完成后，会在 ~/.ssh/目录下生成一个 aysee 和 aysee.pub 文件。

2、 在 SSH 用户配置文件 ~/.ssh/config 中指定对应服务所使用的公秘钥名称，如果没有 config 文件的话就新建一个，并输入以下内容：

```
Host github.com www.github.com
IdentityFile ~/.ssh/aysee
```
3、添加 aysee.pub 到你的git服务器网站上。








4、测试配置文件是否正常工作

`ssh -T git@github.com`

 如果，正常的话，会出现如下提示：

```
Hi USERNAME! You've successfully authenticated, but github does not provide shell access.
```
 如果出现如下提示，则说明有权限问题：

`Permission denied (publickey)`

如果有权限问题的情况下，你对项目执行push操作的时候，会得到如下提示：

```
Warning: Permanently added the RSA host key for IP address '192.30.252.129' to the list of known hosts.
Permission denied (publickey).
fatal: Could not read from remote repository.
 
Please make sure you have the correct access rights
and the repository exists.
```
　　

#### 多用户时出现权限问题的原因：

github使用SSH与客户端连接。如果是单用户（first），生成密钥对后，将公钥保存至 GitHub ，每次连接时SSH客户端发送本地私钥（默认~/.ssh/id_rsa）到服务端验证。单用户情况下，连接的服务器上保存的公钥和发送的私钥自然是配对的。但是如果是 多用户 （first，second），我们在连接到second的帐号时，second保存的是自己的公钥，但是SSH客户端依然发送默认私钥，即first的私钥，那么这个验证自然无法通过。

 

#### 解决ssh权限问题（）:
通常一台电脑生成一个ssh不会有这个问题，当一台电脑生成多个ssh的时候，就可能遇到这个问题，解决步骤如下：

1、查看系统ssh-key代理,执行如下命令

`$ ssh-add -l`

以上命令如果输出  The agent has no identities. 则表示没有代理。如果系统有代理，可以执行下面的命令清除代理:

`$ ssh-add -D`

2、然后依次将不同的ssh添加代理，执行命令如下：

```
$ ssh-add ~/.ssh/id_rsa
$ ssh-add ~/.ssh/aysee
```
　你会分别得到如下提示：

`2048 8e:71:ad:88:78:80:b2:d9:e1:2d:1d:e4:be:6b:db:8e /Users/aysee/.ssh/id_rsa (RSA)`

　　和

```
2048 8e:71:ad:88:78:80:b2:d9:e1:2d:1d:e4:be:6b:db:8e /Users/aysee/.ssh/id_rsa (RSA)
2048 a7:f4:0d:f1:b1:76:0b:bf:ed:9f:53:8c:3f:4c:f4:d6 /Users/aysee/.ssh/aysee (RSA)
```

　　如果使用 ssh-add ~/.ssh/id_rsa的时候报如下错误，则需要先运行一下 ssh-agent bash 命令后再执行 ssh-add ...等命令

`Could not open a connection to your authentication agent.`


3、配置 ~/.ssh/config 文件

　　如果没有就在~/.ssh目录创建config文件，该文件用于配置私钥对应的服务器

```
# Default github user(first@mail.com)
 
Host github.com
HostName github.com
User git
IdentityFile C:/Users/username/.ssh/id_rsa
 
# aysee (company_email@mail.com)
Host github-aysee
HostName github.com
User git
IdentityFile C:/Users/username/.ssh/aysee
```

　　Host随意即可，方便自己记忆，后续在添加remote是还需要用到。 配置完成后，在连接非默认帐号的github仓库时，远程库的地址要对应地做一些修改，比如现在添加second帐号下的一个仓库test，则需要这样添加：

```
git remote add test git@github-aysee:ay-seeing/test.git

#并非原来的git@github.com:ay-seeing/test.git
　　ay-seeing 是github的用户名
```

### 记住添加remote后一定要执行以下代码：
```
git config --global user.email "you@example.com"
git config --global user.name "yourname"
```


4、测试 ssh

`ssh -T git@github.com`

　　你会得到如下提示，表示这个ssh公钥已经获得了权限

`Hi USERNAME! You've successfully authenticated, but github does not provide shell access.`

5、 如果三恢复备份的密钥，添加git remote时候可能会报错：

```
Permissions 0775 for '/home/bingo/.ssh/dtdjview' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
```

解决办法，进入文件夹 .ssh/：
`chmod 400 dtdjview`
或者执行：`chmod 400 ～/.ssh/dtdjview`

6、 pull 代码时候如果报错

```
Bad owner or permissions on /home/bingo/.ssh/config
fatal: 无法读取远程仓库。

请确认您有正确的访问权限并且仓库存在。
```
解决办法：
`chmod 600 ～/.ssh/config`

