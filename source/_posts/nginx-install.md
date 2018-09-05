---
title: Nginx详细安装部署教程
date: 2018-07-31 15:49:48
tags: 
- nginx 
categories: 
- fe
---
转载自：[Nginx详细安装部署教程](https://www.cnblogs.com/taiyonghai/p/6728707.html)
### 安装遇到的问题
#### 重启防火墙的时候遇到了如下问题
输入如下命令
```shell
//重启防火墙
service iptables restart
```
输入以上命令的时候，出现了如下报错***Failed to restart iptables.service: Unit not found***,后来排查了之后找到原因貌似是因为在***CentOS 7或RHEL 7或Fedora中防火墙由firewalld来管理***,想要采用`service iptables restart`需要执行如下命令
```shell
systemctl stop firewalld
systemctl mask firewalld

//安装iptables-services：
yum install iptables-services

//设置开机启动
systemctl enable iptables
```
后续运行
```shell
systemctl restart iptables

//命令有stop|start|restart
//or
//service iptables restart
//命令有stop|start|restart
```
在进行了以上步骤之后，又出现了新的问题提示`Redirecting to /bin/systemctl restart iptables.service
Job for iptables.service failed because the control process exited with error code.See "systemctl status iptables.service" and "journalctl -xe" for details`,解决方案如下：
```shell
//重新运行网络管理装置

chkconfig network off

chkconfig network on

service NetworkManager stop

service NetworkManager start
```
这回终于重启成功，在host中配置了

```text
47.104.185.99 nginx.test.com
```
ping了一下nginx.test.com，链接成功

```shell
ping nginx.test.com

```