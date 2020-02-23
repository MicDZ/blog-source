---
title: Linux配置ssr客户端
date: 2018-08-19 11:27:43
tags: [随笔,黑科技]
categories:
- Dev   
---

ShadowsocksR

A fast tunnel proxy that helps you bypass firewalls.

<!--more-->

Server
------

## Install

Debian / Ubuntu:

    apt-get install git
    git clone https://github.com/shadowsocksr/shadowsocksr.git

CentOS:

    yum install git
    git clone https://github.com/shadowsocksr/shadowsocksr.git

Windows:

    git clone https://github.com/shadowsocksr/shadowsocksr.git

## Usage for single user on linux platform

If you clone it into "~/shadowsocksr"  
move to "~/shadowsocksr", then run:

    bash initcfg.sh

move to "~/shadowsocksr/shadowsocks", then run:

    python server.py -p 443 -k password -m aes-128-cfb -O auth_aes128_md5 -o tls1.2_ticket_auth_compatible

Check all the options via `-h`.

You can also use a configuration file instead (recommend), move to "~/shadowsocksr" and edit the file "user-config.json", then move to "~/shadowsocksr/shadowsocks" again, just run:

    python server.py

To run in the background:

    ./logrun.sh

To stop:

    ./stop.sh

To monitor the log:

    ./tail.sh
