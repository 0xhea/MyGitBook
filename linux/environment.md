# 环境搭建

## 用户创建

useradd -r -m -s /bin/bash eric

passwd eric

usermod -s /bin/bash eric  // 指定用户使用shell

vim /etc/sudoers  // 添加sudo权限



## 必备库

`sudo apt install git`

```
git config --global user.name "Eric Zhou"
git config --global user.email "eric.what.zhou@gmail.com"
ssh-keygen -t rsa -C "eric.what.zhou@gmail.com"
```

`sudo apt install curl`

### zsh

`sudo apt install zsh`

```
cat /etc/shells  查看系统有哪些shell
echo $SHELL  查看当前使用shell
chsh -s /bin/zsh  更改默认shell
```



#### oh-my-zsh



### stow

> [GNU Stow](http://www.gnu.org/software/stow/)
>
> [stow管理配置文件](https://github.com/jcouyang/dotfiles)
>
> [dotfiles](https://github.com/xero/dotfiles)

`apt install stow`

### v2ray

bash <(curl -L -s https://install.direct/go.sh)  // 有个timeout，应该删掉

[ubuntu19.04安装调试v2ray客户端,以及配置代理](https://www.jianshu.com/p/77a652450f91)

