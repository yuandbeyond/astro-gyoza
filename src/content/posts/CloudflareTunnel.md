---
title: 使用 CloudFlare Tunnel 快速实现内网穿透
date: 2025-05-10
tags: [内网穿透]
category: 教程
comments: true
draft: false
image: /images/CloudflareTunnel-1.jpg
---

## Cloudflare Tunnel 介绍

Cloudflare Tunnel 是 Cloudflare 这个赛博菩萨提供的一个免费的流量代理服务。通过在源站和 Cloudflare边缘节点建立一条隧道，所有访问此服务的流量都要先到达cloudflare。再经过 Cloudflare和服务器源站之间建立的 Cloudflare Tunnel 到达源站。简单的说就是一个简易的内网穿透服务。

## Tunnel 可以做什么

1. 可以把服务端的本地服务暴露给公网
2. 无需配置即可用的 HTTPS 证书
3. 将非常规端口服务转发到 80/443 常规端口

## 创建 Tunnel

1. 登录 [CloudFlare](https://www.cloudflare.com) 官网，进入 Dashboard，选择左边菜单中的 Zero Trust。

![CloudflareTunnel-1](images/CloudflareTunnel-1.png)

2. 在左边菜单中选择 Networks-Tunnels

![CloudflareTunnel-2](images/CloudflareTunnel-2.png)

3. 点击 Create a tunnel

![CloudflareTunnel-3](images/CloudflareTunnel-3.png)

## 连接服务端

1. 选择 Cloudflared，设置一个名称后，选择服务端的操作系统这里使用 Debian 演示。

![CloudflareTunnel-4](images/CloudflareTunnel-4.png)

![CloudflareTunnel-5](images/CloudflareTunnel-5.png)

![CloudflareTunnel-6](images/CloudflareTunnel-6.png)

2. 根据页面指示，若先前未安装过 Cloudflared，则使用以下指令：

```Shell
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb &&

sudo dpkg -i cloudflared.deb &&

sudo cloudflared service install eyJhIjoiNWYzYjgwNzQ1NWQyMDZkMmNiYzdmYzUyMDdkNmJmYTUiLCJ0IjoiNGZmNzUyMTUtZGQ0MC00MjRlLWI0MDYtNDYzYjMxYjk5YmYwIiwicyI6IlpXTmhZelV5WVRrdFlqWmtaaTAwWkRjNExUa3hOekl0TVdGaE1EWTFOekE1WWpsaCJ9
```

若已经安装好，则使用以下指令安装服务：

```shell
sudo cloudflared service install eyJhIjoiNWYzYjgwNzQ1NWQyMDZkMmNiYzdmYzUyMDdkNmJmYTUiLCJ0IjoiNGZmNzUyMTUtZGQ0MC00MjRlLWI0MDYtNDYzYjMxYjk5YmYwIiwicyI6IlpXTmhZelV5WVRrdFlqWmtaaTAwWkRjNExUa3hOekl0TVdGaE1EWTFOekE1WWpsaCJ9
```

## 配置 Tunnel

1. 安装好后，回到 Tunnels 页面，若 status 一栏显示 HEALTHY，则表示连接成功

![CloudflareTunnel-7](images/CloudflareTunnel-7.png) 2. 进入 configure 界面，选择 Public Hostname，点击 Add a public hostname。

![CloudflareTunnel-8](images/CloudflareTunnel-8.png) 3. 选择域名，自定义一个子域名后，Type 选择 http，url 填写服务器上服务的运行地址，例如：localhost:8001。填写完成后保存。

![CloudflareTunnel-9](images/CloudflareTunnel-9.png)

## 完成

大功告成！现在可以使用上一步自定的域名访问服务器的本地服务。
