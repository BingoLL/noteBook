## 两个命令把 Vim 打造成 Python IDE

```
curl -O https://raw.githubusercontent.com/vince67/v7_config/master/vim.sh
bash vim.sh
```
### 附脚本:

1. 注意一: 每次运行本脚本，会把旧的 ~/.vimrc 文件备份到 /tmp/vimrc.bak ，并替换原来的 ~/.vimrc 文件 。
2. 注意二: 目前支持MAC和Ubuntu系统。
3. 脚本代码如下:

```
#!/bin/bash
# install fisa vim config

echo '==============================='
echo 'start to install dependences...'
case "$OSTYPE" in
    darwin*)  brew install vim git pip curl;;
    linux*)   sudo apt-get install vim exuberant-ctags git pip curl;;
    *)        echo "unknown: OS: $OSTYPE, U should install dependences by yourself" ;;
esac
sudo pip install dbgp vim-debug pep8 flake8 pyflakes isort

echo '==============================='
echo 'start to download vimrc file...'
cp ~/.vimrc /tmp/vimrc.bak
curl -O https://raw.githubusercontent.com/fisadev/fisa-vim-config/master/.vimrc
mv .vimrc ~/.vimrc

echo '==============================='
echo 'start to install vim plugins...'
vim +BundleClean +BundleInstall! +qa

sudo chown $USER ~/.vim/
echo 'down! enjoy it!'
```