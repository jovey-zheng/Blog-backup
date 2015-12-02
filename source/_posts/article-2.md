title: 生成SSH keys
date: 2015.09.07 15:04:33
categories:
	- 笔记随笔
tags:
	- SSH
	- git
	- 加密
---
首先确认自己的系统中是否已经拥有密钥。在默认情况下SSH的密钥存储在其`~/.ssh`目录下。可以使用以下命令进入目录并列出内容：
```
$ cd ~/.ssh
$ ls

id_rsa  id_rsa.pub  known_hosts
```

其中`id_rsa`和`id_rsa.pub`就是存储密钥的文件，带有`.pub`后缀的是公钥，另外一个则是私钥。如果存在这些文件，则可以直接用`$ cat id_rsa.pub`来读取密钥内容。
如果找不到这样的文件（或者不存在`~/.ssh`目录），则可以通过`$ ssh-keygen`来创建它们。
```
$ ssh-keygen -t rsa -C `you@email.com`

Generating public/private rsa key pair.
Enter file in which to save the key (/home/schacon/.ssh/id_rsa):
Created directory '/home/schacon/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/schacon/.ssh/id_rsa.
Your public key has been saved in /home/schacon/.ssh/id_rsa.pub.
The key fingerprint is:
d0:82:24:8e:d7:f1:bb:9b:33:53:96:93:49:da:9b:e3 you@email.com
```

<!-- more -->

首先 `$ ssh-keygen`会确认密钥的存储位置（默认是` .ssh/id_rsa`），然后它会要求你输入两次密钥口令。如果你不想在使用密钥时输入口令，将其留空即可（为了方便以后操作，建议不设置密码）。

在完成上述操作之后即可获得SSH key，获得的公钥大概是这样的：
```
$ cat id_rsa.pub

ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAmzM2RosSFanpxK+d3Eagt3Wicef9QbgH1x4yH3MFg2
+6vIuFXchl+L3gMZabWH3BzKpBwoJICg8q9k4N8nOf5LNPtIp74hnEj/1b9Nh7OLrri82Ao6FYEdkC
0NVsfhKlqha10MQrYxctimabtuKZdoUvv0knSawwvql2mvCIDra2D2350ICycZi0Fg1QULF3QdDF8E
mtnvso1a5a9jgzf3tyHX6+r7lGnA+Ifzr8bxC4sqZ+aN0R7dn4uqQETF7l+n16dd370Efvbvj8CabZ
qVs7r5j/fdltcmSrH3i97Yfq0XsM0CIxltOIb8+MhkRzHAXdjWY51LyfyHtyysbgHw==
you@email.com
```

关于在多种操作系统中生成 SSH 密钥的更深入教程，请参阅 GitHub 的 SSH 密钥指南：[*https://help.github.com/articles/generating-ssh-keys*](https://help.github.com/articles/generating-ssh-keys)