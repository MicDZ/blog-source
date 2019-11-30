---
title: 将Git本地仓库上传至github
date: 2018-05-25 20:28:22
tags: [随笔]
categories:
- Dev  
---



近一段时间入手Git和GitHub，一直有许多疑惑。今天得以解决。更加深了对于Git的使用以及对GitHub的看法。



本文将会系统地介绍将Git本地仓库上传至github。

<!--more-->

# 准备工作：

1. 一个GitHub账户。
2. 一个GitHub仓库。
3. 与GitHub账户绑定好ssh密钥。_具体操作[here](https://git-scm.com/book/zh/v1/%E6%9C%8D%E5%8A%A1%E5%99%A8%E4%B8%8A%E7%9A%84-Git-%E7%94%9F%E6%88%90-SSH-%E5%85%AC%E9%92%A5)_


# 操作步骤

接下来随便找到一个文件夹，右键git bash here。

将这个文件夹与你的仓库绑定

```
$ git remote add origin git@github.com:youusername/test.git
```
将仓库上的文件拉下来，否则会报错

```
$ git pull origin master
```
记录下刚刚操作的文件

```
$ git add .
```

提交到GitHub

```cpp
$ git push origin master
```



最后你就可以到你的GitHub仓库中查看代码了。