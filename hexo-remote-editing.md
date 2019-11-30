---
title: hexo博客实现远程编辑
categories:
  - Solutions
date: 2019-08-24 08:38:34
tags: [hexo]
---

众所周知，hexo是一个静态的网页生成平台，其生成的page是无后端的，因此如果要实现讨论、投票、点赞等功能都需要依赖第三方的服务器如Leancloud进行存储。

<!--more-->

# 服务器云端部署

对于博客实现远程编辑也是如此。但是博客的远程编辑不仅依赖于其存储，还需要一个主机进行generate。那么仅依靠Leancloud是远不够的。

所以市面上几乎所有的hexo远程编辑的方法都是要将你的网站部署到服务器上。

但是这又牵扯出了一个问题，你既然已经花高价租用了服务器，为什么还要用静态的博客呢？

所以部署到服务器的方法仅限于已经有了服务器或不缺钱的同学。

网络上有许多类似的教程，这里就不讲了。

# Onedrive+Teamviewer

我们考虑远程编辑的两个重要部分——存储+部署。

将上述两步骤拆开则可以得到两个极其方便好用的软件——Onedrive、Teamviewer。

对于我来说，我大部分的时间都是呆在学校，而我的博客部署的主机在家里且不便于携带。那么解决方案就是这样。

## 上传

将你的blog的所有源码上传到Onedrive中，每次都到onedrive文件夹中生成和部署，记得每次都要清理一下public文件，免得每次都要生成后的源码上传。

![](https://www.micdz.cn/img/2019-10-02-26.png)


## 同步_post文件夹

![](https://www.micdz.cn/img/2019-10-02-27.png)

这个步骤通常非常迅速，因为MD的文件都非常小。

此时你的onedrive文件中就会包含你所有文章的MD文件。此时你将你需要编辑的文件打开修改再保存，你的文件就会自动地被上传到Onedrive中。

![](https://www.micdz.cn/img/2019-10-02-28.png)

## Teamviewer 连接

用Teamviewer远程连接你的部署用的电脑，找到Onedrive文件夹，此时的_post中的文件应该已经同步成功了，此时生成后的网站就应该有了你的改动。

但是值得注意的是，你的电脑必须要保持运作（可以熄屏锁定，要内存与硬盘通电），这样才可以保证随时唤醒Teamviewer。