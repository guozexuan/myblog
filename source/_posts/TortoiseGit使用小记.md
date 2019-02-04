---
title: TortoiseGit使用小记
date: 2018-10-12 00:11:03
tags:
---

> 这是一篇多图博客，[便捷插入图片分享](https://www.jianshu.com/p/cf0628478a4e)

## TortoiseGit简介

tortoiseGit是一个开放的git版本控制系统的源客户端,该软件功能和git一样。
不同的是：git是命令行操作模式，tortoiseGit界面化操作模式。
[下载](https://tortoisegit.org/download/)

## 安装步骤
1、下载所需版本，需要汉化的顺便下载第二个箭头指向的chinese,xxx，英语66的请忽略~~
点击安装，一直下一步即可，途中可能有那些配置什么什么的，先忽略，后面再配置

{% asset_img TortoiseGit使用小记.jpg 1.png %}

语言包直接点击打开安装即可，然后就可以选择中文版了~

{% asset_img TortoiseGit使用小记.jpg 2.png %}

可能会遇到git版本过低，重新安装覆盖即可
{% asset_img TortoiseGit使用小记.jpg 3.png %}
{% asset_img TortoiseGit使用小记.jpg 4.png %}

到这里安装好了~接下来配置

2、配置秘钥

[下载](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) PuttyGen:是一套可以产生密钥的工具;可以生成RSA或DSA密钥;用于Putty、Plink、PSFP、PSCP和Pageant.

- 点击Generate生成密钥：
<font color=#ff0606>注意：生成时鼠标要不停划过进度条，不然进度条会一直不动！ </font>
{% asset_img TortoiseGit使用小记.jpg 5.png %}

- 先点击Save private key把私有的密钥存起来，记住存储的位置,后面会用到
{% asset_img TortoiseGit使用小记.jpg 6.png %}

- 把生成出来的public Key复制粘贴到Gitlab上面，配置SSH key
{% asset_img TortoiseGit使用小记.jpg 7.png %}

- 打开：开始-->TortoiseGit-->Pageant，打开以后右下角会有图标，双击点开蓝屏幕电脑那个图标
<font color=#c35959>说明：使用TortoiseGit进行和远端输出项目时，Pageant必须启动且添加了对应的私钥。否则会报错 </font>
{% asset_img TortoiseGit使用小记.jpg 8.png %}
{% asset_img TortoiseGit使用小记.jpg 9.png %}

到这里已经可以使用了~

- Pageant在git中主要负责和服务器端进行身份验证，但是每次在启动Pageant后都需要手动的加载秘钥文件，为了避免每次使用都要重复添加私钥，可以设置一下自动加载。

(1)Pageant 的开机自启 
首先找到TortoiseGit 的安装目录的bin目录，然后找到pageant.exe 然后单击右键 创建快捷方式，此时bin目录中有 两个pageant.exe 文件

{% asset_img TortoiseGit使用小记.jpg 10.png %}

win10添加自启项：win+r => 输入：shell:startup =>  将自启项的快捷方式丢进去即可

{% asset_img TortoiseGit使用小记.jpg 11.png %}

win10查看自启项：win+r => 输入：msconfig => 启动-打开任务管理器-启动。  
即可查看是否添加成功

(2)关联加载秘钥ppk 
在上面的开机自启文件夹下 选中pageant 的快捷方式 查看属性，在目标的一栏，前面是 pageant 程序的位置 ，在后面空格 然后粘贴ppk秘钥的路径（前面保存过的），然后点击确定就搞定了 

{% asset_img TortoiseGit使用小记.jpg 12.png %}

## 使用
大致功能如图：（每台电脑貌似会存在差异，）
{% asset_img TortoiseGit使用小记.jpg 14.png %}

#### 克隆

空白文件夹右键 - Git克隆：
填入仓库地址
{% asset_img TortoiseGit使用小记.jpg 13.png %}


#### 分支操作

右键-切换切出-创建分支
右键-创建分支
{% asset_img TortoiseGit使用小记.jpg 15.png %}

#### 提交

{% asset_img TortoiseGit使用小记.jpg 16.png %}

{% asset_img TortoiseGit使用小记.jpg 17.png %}

{% asset_img TortoiseGit使用小记.jpg 18.png %}

{% asset_img TortoiseGit使用小记.jpg 19.png %}

{% asset_img TortoiseGit使用小记.jpg 20.png %}

。。。。还有很多功能呀，自己慢慢摸索~~~