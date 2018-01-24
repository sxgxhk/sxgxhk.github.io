---
title: Centos7上搭建Lnmp+Ghost 1.x博客
date: 2017-11-02 21:15
tags: Ghost
categories: 网站建设
permalink: 2504
---

上次写过[《Debian 8.X搭建Lnmp+ Ghost 1.x 教程》](https://blog.uu126.cn/debian-8-8da-jian-lnmp-ghost-1-x-jiao-cheng/) ，这不将主机从香港搬回到大杭州后，域名又倒腾回之前的uu126.cn（当然加了个前缀blog），加上前不久刚好在[海月博客](http://blog.imzhengfei.com/centos-7-an-zhuang-pei-zhi-ghost-1-x/) ，顺手也尝试了一下Centos，结果一切搭建顺利，又可以让我啰嗦一下了。


<!--more-->


1. Lnmp的搭建就不细说了，可以参照我之前的博文（点击：[传送门](https://blog.uu126.cn/debian-8-8da-jian-lnmp-ghost-1-x-jiao-cheng/) ），这里还是推荐使用Oneinstack，相对功能多一点。搭建好以后，使用PHPmyadmin创建所需的博客数据库用户名、数据库（用这个创建较方便），另外也可以直接配置好站点的Nginx文件和SSL，这里就不在啰嗦了，有问题的亲，可以留言。
2. 安装 Node.js
 - 方法一：

```
curl -sL https://deb.nodesource.com/setup_6.x | bash -  
apt-get install nodejs
```

 - 方法二（推荐使用）：

```
国外下载地址：
wget https://nodejs.org/download/release/v6.9.5/node-v6.9.5.tar.gz
国内下载地址：
wget https://npm.taobao.org/mirrors/node/v6.9.5/node-v6.9.5.tar.gz
tar zxvf node-v6.9.5.tar.gz
cd node-v6.9.5
./configure 
make && make install
```

安装好之后可以使用` node -v `查看，能看到版本号一般就OK了。
3. 配置 Yarn
  可选，使用 yarn 代替 npm，更加快速，安全，稳定。

```
wget https://dl.yarnpkg.com/rpm/yarn.repo -O /etc/yum.repos.d/yarn.repo
yum -y install yarn
```

同样安装之后可以使用` yarn -v `查看，看到版本号就行。
 - 查看 npm 全局生成可执行文件软链的路径：
```
npm -g bin
```
>/usr/bin

 - 查看 yarn 全局生成可执行文件软链的路径：
```
yarn global bin
```
>/usr/local/bin

修改 yarn 全局生成可执行文件软链的路径和 npm 保持一致：
```
yarn config set prefix /usr
```

>yarn config v1.2.1
>success Set "prefix" to "/usr".
>Done in 0.03s.

再次查看 yarn 全局生成可执行文件软链的路径：
```
yarn global bin
```
>/usr/bin

4. 安装 Ghost-CLI
使用 yarn 全局安装 ghost-cli：
```
yarn global add ghost-cli
```
查看安装的 ghost-cli 版本：
```
ghost -v
```
>Ghost-CLI version: 1.1.3

5. 安装Ghost1.X
创建网站目录：
```
mkdir -p /home/wwwroot/blog.uu126.cn
```
进入到网站目录：
```
cd /home/wwwroot/blog.uu126.cn
```
在当前目录免配置安装 ghost：
```
ghost install --no-setup
```

>✔ Checking system Node.js version
>✔ Checking current folder permissions
>System checks failed with message: 'Linux version is not Ubuntu 16'
>Some features of Ghost-CLI may not work without additional configuration.
>For local installs we recommend using ghost install local instead.
>? Continue anyway?

输入` y `，回车：

>? Continue anyway? Yes
>✔ Checking operating system
>✔ Checking MySQL is installed
>✔ Checking for latest Ghost version
>✔ Setting up install directory
>✔ Downloading and installing Ghost v1.16.0
>✔ Finishing install process

6. 配置Ghost1.x
 - 使用 ghost-ci 配置 ghost：
```
ghost config
```
>? Enter your blog URL: (http://localhost:2368)

输入自己网站完整访问路径` http://blog.uu126.cn `回车：

>? Enter your blog URL: http://blog.imzhengfei.com
>? Enter your MySQL hostname: (localhost)

输入 MySQL 主机地址，如果在本机直接回车：

>? Enter your MySQL hostname: localhost
>? Enter your MySQL username:

输入上面创建的 MySQL 用户名，密码，数据库名称：

>? Enter your MySQL username: ghost
>? Enter your MySQL password: [hidden]
>? Enter your Ghost database name: ghost

 - 初始化数据库

使用 ghost-ci 初始化数据库：
```
ghost setup migrate
```
>✔ Running database migrations

 -  配置系统用户

使用 ghost-ci 添加 ghost 用户来运行 ghost：
```
ghost setup linux-user
```

>Running sudo command: useradd --system --user-group ghost
>Running sudo command: chown -R ghost:ghost /home/wwwroot/blog.imzhengfei.com/content
>✔ Setting up "ghost" system user

 - 配置系统服务

使用 ghost-ci 创建系统服务：
```
ghost setup systemd
```

>Running sudo command: ln -sf >/home/wwwroot/blog.uu126.cn/system/files/ghost_undefined.service >/lib/systemd/system/ghost_undefined.service
>Running sudo command: systemctl daemon-reload
>✔ Setting up Systemd

7. 测试访问博客
 - 使用 ghost-ci 启动当前网站：
```
ghost start
```

>✔ Validating config
>Running sudo command: systemctl start ghost_undefined
>✔ Starting Ghost
>Running sudo command: systemctl enable ghost_undefined --quiet
>✔ Starting Ghost
>You can access your blog at http://blog.uu126.cn
>Ghost uses direct mail by default
>To set up an alternative email method read our docs at https://docs.ghost.org/docs/mail-config

 - 尝试访问自己的博客。

注册管理员账户**（ domain 是自己的博客域名）**：
```
http://<domain>/ghost
```
