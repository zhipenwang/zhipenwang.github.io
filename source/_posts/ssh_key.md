---
title: git 配置ssh密钥
date: 2018-03-27 19:21:01
tags: GIT
categories: GIT
---
在github帐号注册好之后，将项目clone到本地，加入git bash命令。
1、输入cd ~/.ssh 回车，查看是否有ssh key密钥，有了就备份
<center>
![](/img/ssh_dir.png)
</center>
2、创建ssh key，输入下面命令回车，之后会让你输入github的账号密码，如图
```
ssh-keygen -t rsa -C "youremail@youremail.com"

Creates a new ssh key using the provided email # Generating public/private rsa key pair.

Enter file in which to save the key (/home/you/.ssh/id_rsa):
```
直接按Enter就行。然后，会提示你输入密码，如下(建议输一个，安全一点，当然不输也行，应该不会有人闲的无聊冒充你去修改你的代码)：
```
Enter same passphrase again: [Type passphrase again]
```
完了之后，大概是这样：
```
Your public key has been saved in /home/you/.ssh/id_rsa.pub.
The key fingerprint is: # 01:0f:f4:3b:ca:85:d6:17:a1:7d:f0:68:9d:f0:a2:db your_email@youremail.com
```
<center>
![](/img/ssh_gen.png)
</center>
3、找到本地id_rsa.pub文件，复制里面的内容，这就是ssh密钥，可以通过pwd命令来查看当前文件位置
<center>
![](/img/ssh_pwd.png)
</center>
<center>
![](/img/ssh_key_dir.png)
</center>
4、登录github，在个人中心的setting中，找到 SSH and GPG keys
<center>
![](/img/github_ssh.png)
</center>
点击 new ssh key，将复制的ssh密钥粘贴进 “key”文本框，title随便输入即可。
点击add key。
添加ssh密钥到远程仓库完成。
5、验证ssh是否可用
```
ssh -T git@github.com
```
返回如下表示正常可用。
```
Hi xxx! You've successfully authenticated, but GitHub does not # provide shell access.
```
6、此时查看你的远程分支地址是否是ssh协议的
```
git remote -v
origin https://github.com/zhipenwang/zhipenwang.git (fetch)
origin https://github.com/zhipenwang/zhipenwang.git (push)
```
如果是https协议，修改为ssh协议：
```
git remote set-url origin git@github.com:zhipenwang/zhipenwang.git
```
这个时候就可以进行push了。