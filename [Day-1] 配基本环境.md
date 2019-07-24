<!--
 * @Author: sunyz
 * @Date: 2019-07-25 00:01:50
 * @github: https://github.com/sunyz
 * @LastEditors: sunyz
 * @LastEditTime: 2019-07-25 03:34:43
 * @Description: content
 -->

# Linux 服务器基本配置

在一台新的服务器上，如果没有tmux， vim， zsh感觉整个人都不好了。
同时拥有root权限才可以为所欲为。所以在服务器配置上，今天把这三件套配置完毕。

## 1. vim

### 1.1 版本升级

服务器自带的是vim7.4, 所以顺手升级成为了vim8.1

```bash
sudo add-apt-repository ppa:jonathonf/vim
sudo apt-get update
sudo apt-get install vim
```

### 1.2 配置vimrc

Github上直接获取舒服的vimrc配置即可
个人经验用起来最舒服的是[amix](https://github.com/amix/vimrc)的版本

```bash
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_basic_vimrc.sh
```

## 2. zsh

zsh作为bash的替代工具，嗯，真香， 配合oh-my-zsh使用体验极佳

安利功能：

- 简化的git命令，如gco=’git checkout’、gd=’git diff’、gst=’git status’、g=’git’等等， 完整列表见<https://github.com/robbyrussell/oh-my-zsh/tree/master/plugins/git/>
- 强大的自动补全， 输入首字母单词即可匹配最近使用的命令，按↑可翻阅
- autojump的跳转，j path 轻松愉悦
- 好看啊！！！

### 2.1 安装zsh

1. 安装
    `sudo apt-get install zsh`
2. 设置默认shell
    `chsh -s /bin/zsh`
    注意： 不要使用sudo
3. 配置密码文件，解决chsh:PAM认证失败的问题
    `sudo vim /etc/passwd`

    把第一行的root用户和最后的自己用户的/bin/bash改成/bin/zsh

### 2.2 安装oh-my-zsh

单单只有zsh仍然只是咸鱼，配合oh-my-zsh一键舒服起来

1. 安装  
    `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
2. 安装autojump自动跳转插件

    `sudo apt-get install autojump`

    配置：

    ```bash
    vim .zshrc
    #在最后一行加入，注意点后面是一个空格
    . /usr/share/autojump/autojump.sh
    source ~/.zshrc
    ```

## 3. tmux

没有tmux，服务不敢挂起，电脑不敢关机

没有tmux，屏幕只敢独占，窗口常需切换

一切都是没有tmux的错！

~~所以tmux偶尔难受的鼠标控制和剪贴板可以忍了~~

因为可能在公司和宿舍切换，用tmux挂起可以放心的退出shell端

分屏对于一边看代码一边看控制台输出方便很多

### 3.1 安装

`sudo apt install tmux`

### 3.2 配置

仍然是直接取自Github
<https://github.com/gpakosz/.tmux>

```bash
cd ~
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

更多配置请参考其README

设置完三件套，之后就可以进入舒服的开发环境啦

明天继续！
[EOF]
