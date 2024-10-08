---
title: zsh简单入门
header: zsh简单入门 
date: 2024-10-07
excerpt: 记录下我的zsh简单入门
---

最近学了一下 [zshell](https://zsh.sourceforge.io/) 写篇文章记录一下。

主要是参考Youtube上的一个[视频](https://www.youtube.com/watch?v=rgdR27KMxpo)，虽然播放量不高，但很有用，我个人很不想用`on-my-zsh`这个，太笨重了，使用 `zsh` 也就是装个主题，安装两三个插件，用不上太多其他的功能，毕竟只是个简单的入门。

## 安装

在`Ubuntu`上可以使用以下命令安装`zsh`

```bash
sudo apt update

sudo apt install zsh

zsh --version
zsh 5.9 (x86_64-ubuntu-linux-gnu)

# 设置zsh为默认shell
chsh -s $(which zsh)
# 输入zsh进入zsh
zsh
```



## 配置

设置`zsh`配置文件目录

```bash
# 编辑zshenv文件
sudo vim /etc/zsh/zshenv

# 在文件末尾添加以下内容，作用是将$ZDOTDIR变量设置为指定的目录
# 以下设置的配置文件目录为/home/cd/.config/zsh

if [[ -z "$XDG_CONFIG_HOME" ]]
then
        export XDG_CONFIG_HOME="$HOME/.config"
fi

if [[ -d "$XDG_CONFIG_HOME/zsh" ]]
then
        export ZDOTDIR="$XDG_CONFIG_HOME/zsh"
fi  

# 保存并退出
```



最终zshenv文件内容为

```bash
# /etc/zsh/zshenv: system-wide .zshenv file for zsh(1).
#
# This file is sourced on all invocations of the shell.
# If the -f flag is present or if the NO_RCS option is
# set within this file, all other initialization files
# are skipped.
#
# This file should contain commands to set the command
# search path, plus other important environment variables.
# This file should not contain commands that produce
# output or assume the shell is attached to a tty.
#
# Global Order: zshenv, zprofile, zshrc, zlogin

if [[ -z "$PATH" || "$PATH" == "/bin:/usr/bin" ]]
then
        export PATH="/usr/local/bin:/usr/bin:/bin:/usr/games"
fi

if [[ -z "$XDG_CONFIG_HOME" ]]
then
        export XDG_CONFIG_HOME="$HOME/.config"
fi

if [[ -d "$XDG_CONFIG_HOME/zsh" ]]
then
        export ZDOTDIR="$XDG_CONFIG_HOME/zsh"
fi  
```



验证配置是否起作用

```bash
echo $ZDOTDIR

# /home/cd/.config/zsh
```



在`$ZDOTDIR`所指向的文件夹下`.zshrc`配置文件

```bash
# 设置保存命令的历史记录
HISTSIZE=1000000
SAVEHIST=1000000
HISTFILE=~/.histfile

zstyle :compinstall '/home/cd/.config/zsh/.zshrc'

autoload -Uz compinit
compinit
```

使用`zsh`的一个主要目的就是它的提示功能，接下来安装的提示插件也跟你输入命令的历史记录有关，所以多保存点

## 插件

- 主题 [powerlevel10k](https://github.com/romkatv/powerlevel10k)
- 语法高亮 [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- 提示补全 [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search)



这下插件安装手动其实也可以，在`$ZDOTDIR`下新建一个名为`plugins`的文件夹，用来保存这些插件，之后从`github`上clone到这个文件夹，然后编辑`.zshrc`文件，让`zsh`加载这些插件

最终的`.zshrc`文件是这样

```bash
# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.config/zsh/.zshrc.
# Initialization code that may require console input (password prompts, [y/n]
# confirmations, etc.) must go above this block; everything else may go below.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

HISTSIZE=1000000
SAVEHIST=1000000
HISTFILE=~/.histfile

zstyle :compinstall '/home/cd/.config/zsh/.zshrc'

autoload -Uz compinit
compinit

# 插件
source $ZDOTDIR/plugins/powerlevel10k/powerlevel10k.zsh-theme
source $ZDOTDIR/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source $ZDOTDIR/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source $ZDOTDIR/plugins/zsh-history-substring-search/zsh-history-substring-search.plugin.zsh

# To customize prompt, run `p10k configure` or edit ~/.config/zsh/.p10k.zsh.
[[ ! -f ~/.config/zsh/.p10k.zsh ]] || source ~/.config/zsh/.p10k.zsh
```



```bash
# 让配置生效
source .zshrc
```

## 总结

以上是一些基本的用法，之后还可以添加 `alias` 就是起一个别名，写一个函数管理这些插件

```bash
() {
  local ZPLUGINDIR=/etc/zsh/plugins

  apply() {
    github=$1
    plugin=$2
    
    if [ ! -d "${ZPLUGINDIR}/${plugin}" ]; then
      echo "WARNING: ${plugin} not found. Installing..."
      sudo git clone "https://www.github.com/${github}/${plugin}" "${ZPLUGINDIR}/${plugin}"
      echo "SUCCESS: ${plugin} installed!"
    fi
    
    if [ "${plugin}" = "powerlevel10k" ]; then
      source "${ZPLUGINDIR}/${plugin}/${plugin}.zsh-theme"
    else
      source "${ZPLUGINDIR}/${plugin}/${plugin}.plugin.zsh"
    fi
  }

  apply zdharma-continuum fast-syntax-highlighting
  apply zsh-users zsh-history-substring-search
  apply zsh-users zsh-autosuggestions
  apply jeffreytse zsh-vi-mode
  apply romkatv powerlevel10k 
}
```

也可以自己去写一些脚本来满足自己的需求，但简单入门也可以满足我当下需求，其他的日后有需要在去学

