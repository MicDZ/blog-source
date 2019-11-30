---
title: 什么，你还在Mac上用Win！
date: 2018-7-29 14:30:09
tags: [随笔] 
categories:
- Others   
---

*注：本文适于使用macOS High Sierra的OIER*

*本文可能引起部分win用户的不良反应，请谨慎观看。*

<!--more-->

# 前言

苹果将在今年秋季发布macOS mojave的正式版本，到时候可能会写一篇与mojave有关文章。小D还不敢在新买的iMac上安装测试版系统。废话不多说，下面直接进入正题。

# VIM的配置及使用

众所周知，在NOI linux这个膜改版的Ubuntu 14下，VIM可以说是效率最高，最方便的编辑器了。而macOS和Linux有着同一个祖先——Unix，自然他们俩的一些功能也就很相似了。

接下来小D将会介绍如何在macOS下把vim 配置得与NOI linux一样好用。

打开终端，终端的地址在应用程序-实用工具

打开终端后，分别输入如下指令

```
$ cd ~
```

```
$ vim ~/.vimrc
```

*经过实测，并不要专门为vim新建一个文件夹*

![](https://www.micdz.cn/img/2019-10-02-2.png)

你会得到一个全新的文件，此时你就可以在这个文件中进行操作了，不需要担心，在这里的一切操作都是可以复原的。

你看到的可能与下面的有些许不同，是因为我已经把配色方案设置好了

![](https://www.micdz.cn/img/2019-10-02-1.png)

这是我的配置文件内容

文档版

```
set encoding=utf-8
set fileencoding=utf-8
set fileencodings=ucs-bom,utf-8,chinese
colorscheme evening
syntax enable
syntax on
set shiftwidth=4
set nowrap
set nu 
set mouse=a
set tabstop=4
set autoindent
set ruler
set cursorline
set nobackup
color evening
map <F5> <Esc>:w<CR> :call CompileRunGcc()<CR>
imap <F5> <Esc>:w<CR> :call CompileRunGcc()<CR>
func! CompileRunGcc()
    exec "w" 
    if &filetype == 'c' 
        exec '!g++ % -o %< -lm -O2'
        exec '!time ./%<'
    elseif &filetype == 'cpp'
        exec '!g++ % -o %< -lm -O2'
        exec '!time ./%<'
    elseif &filetype == 'sh'
        :!time bash %
    endif                                                                              
endfunc 
map <F6> <Esc>:w<CR>
map <F7> <Esc>:w<CR> :q<CR> 
```

*更多的配置方案可以参考：[真实有效的Vim配置记录(macOS)](https://www.jianshu.com/p/a03fcb09924c)*

利用以上的配置可以实现：

1. F5一键运行cpp、c的代码，您可以自己进行添加以实现py、pa的功能。
2. F6一键保存，F7一键保存并退出

当您配置好文件后，按键盘左上角的Esc然后依次输入:w和:q即可完成配置。

此时您可以到你的工作目录愉快地使用vim进行编程了。

# 将VIM的编译器变成g++

当你按照上面的方法完成对VIM的配置后，愉快地进行编程，按下F5后的那一刻

![](https://www.micdz.cn/img/2019-10-02-3.png)

woc，macOS竟然不支持bits？！此时心中有千万只***奔腾而过。不要慌张，下面是最轻松的解决方法。

macOS 自带的C++编译器是Clang而这一个C++编译器是不支持bits下的所有头文件的，即使你添加了源码也不支持（为此我专门打开了macOS的安全模式关闭了rootless）。

于是，我意识到，我可能需要安装一个g++，g++是肯定支持bits的。

于是就有了解决方案：

1. 没有安装HomeBrew 的利用下面的代码安装

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. 安装g++和gcc(4.9)

```
brew install gcc@4.9 --with-all-languages
```

3. 将编译指令改为g++-4.9

![](https://www.micdz.cn/img/2019-10-02-4.png)

![](https://www.micdz.cn/img/2019-10-02-5.png)

  

4. 尝试执行下面的代码查看你的编译器

```
#include<bits/stdc++.h>
using namespace std;
int main() {
    cout << __VERSION__ << endl;
    return 0;
}
```

如果输出为g++-4.9，从此以后妈妈再也不用担心我忘记头文件。


# 在macOS上与外界交流

![](https://www.micdz.cn/img/2019-10-02-6.png)

你只需要登陆你的iCloud账号就可以与任何拥有任意苹果设备的人（包括iPhone、iPod、Mac、iWatch、iPad）进行Facetime通话。如果你的手机在同一个局域网内并且登陆了同一个苹果账号即可进行普通的电话拨打给任意用户。

![](https://www.micdz.cn/img/2019-10-02-7.png)

同样的，如果你登陆了iCloud即可给任意拥有苹果设备的人发送iMessage，如果你的手机在同一个局域网内并且登陆了同一个苹果账号即可发送普通的短信给任意用户。

![](https://www.micdz.cn/img/2019-10-02-11.png)

不仅如此，还有更加厉害的联络方式——使用邮件

![](https://www.micdz.cn/img/2019-10-02-8.png)

目前，已经支持了以上的所有邮件公司，足够满足我们的日常生活，但是！

它还支持你的企业邮箱。

以qq企业邮箱为例

![](https://www.micdz.cn/img/2019-10-02-9.png)

苹果的邮箱账户可以利用IMAP协议与你的企业邮箱进行连接，只需要输入您的地址和密码即可

![](https://www.micdz.cn/img/2019-10-02-10.png)

# 接力使你的工作更加轻松

当你在iPad上打开luogu，看到一道非常好的题目，想要到电脑上实现它，非常轻松，只需要到duck的左下角，你会发现一个浏览器的图标左上角跟随了你的iPad图标。点开后甚至能够做到页面上的工作内容“接力”，例如luogu在线IDE。

# 更多方便的功能等你来探索

在macOS上有许多的高级的软件，例如Xcode、Final Cut……