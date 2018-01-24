---
title: Debian 8.X搭建Lnmp+ Ghost 1.x 教程
date: 2017-09-28 15:22
tags: Ghost
categories: 网站建设
permalink: 2490
---

其实类似的教程挺多的，尤其是Ghost 1.X版本之前的，真的不要太多，写这个教程也只是自己参照官网教程和烧饼博客之后，对自己的搭建经历作一个小小的回顾。因为本人还要折腾点别的小玩意，所以不仅要将Ghost博客搭建起来，还要将PHP环境等也一同搭建，便于日后的各种折腾。好了，废话不多说，开始啰嗦之旅吧。

<!--more-->

 - 首先搭建Lnmp，可用的环境包有很多，比如军哥的Lnmp，还有最近比较火的OneinStack等等，个人比较喜欢OneinStack，觉得功能上比前者要丰富。具体的搭建步骤，请参考[官网](https://oneinstack.com/install/)，一路配置下来估计也要半个小时左右，成功率100%（反正我折腾了这么多次，从来没有失败过），搭建好之后，建议先做个快照！
2017-8-19补充：还是把Oneinstack的搭建步骤照抄一下吧，给看的亲们省点时间233
```
yum -y install wget screen curl python #for CentOS/Redhat
# apt-get -y install wget screen curl python #for Debian/Ubuntu
wget http://aliyun-oss.linuxeye.com/oneinstack-full.tar.gz #阿里云经典网络下载
wget http://mirrors.linuxeye.com/oneinstack-full.tar.gz #包含源码，国内外均可下载
wget http://mirrors.linuxeye.com/oneinstack.tar.gz #不包含源码，建议仅国外主机下载
tar xzf oneinstack-full.tar.gz
cd oneinstack #如果需要修改目录(安装、数据存储、Nginx日志)，请修改options.conf文件
screen -S oneinstack #如果网路出现中断，可以执行命令`screen -R oneinstack`重新连接安装窗口
./install.sh #注：请勿sh install.sh或者bash install.sh这样执行
```

![install_oneinstack](https://cdn.uu126.cn/201708/install_oneinstack.png)

 - 开始搭建Ghost（过程可参照官网或烧饼博客）
1. 更新系统并安装必要软件
```
apt-get update 
apt-get upgrade
apt-get -t stretch-backports update 
apt-get -t stretch-backports upgrade
```
2. 安装 Node.js 6.x LTS
方法一：
```
curl -sL https://deb.nodesource.com/setup_6.x | bash -  
apt-get install nodejs
```
方法二：
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

>2017-10-22补充，最新版的Ghost已经不支持Node V6.5版本了，所以嘛……得更新233

推荐使用方法二，成功率高，如果使用方法一没有成功的话，还可以再使用方法二，成功与否，可以使用
`node -v `，如果能显示版本就说明Node.js搭建成功了
4. 配置Nginx（因为前面已搭建好Lnmp，所以这里直接配置站点文件即可）
  先配置站点文件
  ```
  vi /usr/local/nginx/conf/vhost/ghost.conf
  ```
  然后写入以下内容：
  ```
  server {  
  listen 80;
  listen 443;
  server_name 1984n.win;  #记得改成自己的域名
  ssl on;
  ssl_certificate /usr/local/nginx/conf/1984.crt; #SSL证书路径
  ssl_certificate_key /usr/local/nginx/conf/1984.key;  #SSL证书路径

    location / {
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        proxy_pass         http://127.0.0.1:2368;
    }
}
  ```
  保存好之后，记得重启一下Nginx。
5. 添加 ghost 运行用户（按照官方的说明，需要另建一个账户，名称随便，这里以`ghost`作为用户名来介绍。首先使用`adduser `而不是` useradd `命令添加用户，后者只能添加一个简单的无密码无执行权限的用户，前者是后者的加强版命令
```
adduser ghost
```
然后系统会提示你输入两次密码，其他的一律回车即可。接着给予` ghost `用户` sudo `权限，这样这货就可以执行好多操作了。
```
usermod -aG sudo ghost
```
6. 安装 Ghost-CLI 和 Ghost 1.x
Ghost 从 1.0 开始，已经不需要其他第三方的软件来保持后台运行、更新、安装等操作，因为他们出了个命令行软件` Ghost-CLI `有了这货，我们再也不需要安装` pm2 `来保持后台运行，也不需要用` ghost-upgrade `来升级，因为他基本已经全部带了以前的功能。
首先我们需要切换到` ghost `这个用户
```
su - ghost
```
输入密码回车,然后直接用 npm 安装` Ghost-CLI `
```
sudo npm i -g ghost-cli
```
假设你的博客要放在` /var/www/ghost `目录，那么我们就创建一个并赋予权限
```
sudo mkdir /var/www/ghost
sudo chown ghost:ghost /var/www/ghost
```
进入这个目录就可以开始安装 Ghost 了
```
cd /var/www/ghost
ghost install
```
按照提示输入即可，如果要使用` sqlite3 `数据库的，可以将` ghost install `改为：
```
ghost install local --db=sqlite3
```
然后修改` /var/www/ghost/config.development.json `文件，修改自己的域名即可，保存好之后，再使用命令：` ghost stop && ghost estart `命令强行关闭再启动（注意不是重启），看是否正确，正确的输出信息是类似这样的:
```
ghost@sb-blog:/var/www/ghost$ ghost start
✔ Validating config
✔ Starting Ghost
You can access your blog at https://1984n.win/

Ghost uses direct mail by default
To set up an alternative email method read our docs at https://docs.ghost.org/docs/mail-config
```
没问题以后，就可以把这个文件改成正式环境的文件了:
```
mv config.development.json config.production.json
```
然后再使用命令：` ghost stop && ghost estart `命令强行关闭再启动。

#### 注意:
另外千万不要在 root 用户下执行 ghost 相关命令，否则 `/var/www/ghost/.ghostpid `这个文件的权限就变成` root `用户的了，导致后续会遇到` Message: 'EACCES: permission denied, open '/var/www/ghost/.ghostpid''`

一切正常的话，就可以开始折腾Ghost了。


本文中搭建Ghost的部分步骤来自烧饼博客：https://sb.sb/debian-ubuntu-install-upgrade-ghost/ ，还有Ghost官网教程：https://docs.ghost.org/docs/install ，写完看电影咯。

2017-8-19补充：话说现在新版的Ghost编辑器还是蛮好用的，连小工具都有了，这在以前的0.7.4版本可是没有的，这也极大方便像我这种非代码控，点点即可上传图片、链接等等。

![ghost_blog02](https://cdn.uu126.cn/201708/ghost_blog02.jpg)