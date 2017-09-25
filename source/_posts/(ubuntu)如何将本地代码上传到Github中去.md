---
title: 如何将本地代码上传到Github中去
date: 2017-08-28 14:37:36
tags: [ubuntu, git , github, node, ssh]
categories: Github使用指南
---
## 检查是否安装git 

可以使用git --version 测试，如果没有安装，用以下命令安装：
```
sudo apt-get install git git-core 安装git
```

## 获取秘钥

ssh-keygen -C "你的github邮箱"　-f ./ssh/github
这时候会在你的电脑上~./下生成一个目录　ssh　文件夹
用以下命令打开编辑：

```
cd ~/.ssh
ssh -keygen -t rsa   //获取秘钥
```
## 秘钥认证

接下来登录我们的github在登录左上角里面进去设置，打开设置后你会发现在左边有个ssh　秘钥，进去把刚刚在命令行打开的文本里面的内容复制粘贴进去，保存．
好了，至此我们的环境就软搭建完成．

```
git@github.com
```
## 本地创建项目文件

在本地创建一个存放上传文件的的到github的仓库，进入其中，输入以下代码：

```
git clone git@github.com:xxx-xxx.git   // git@github.com:xxx-xxx.git 这里的地址是你仓库里项目的路径

```
这样你在云端仓库的项目就会clone在你本地电脑里面．

之后就按照下面的步骤：

```
git add . //更新整个目录
git config user.name "xxx-xxx" //xxx-xxx  你的github名称（首次）
git donfig user.email "xxx-xxx@163.com"  // xx-xxx@163.com　你的邮箱（首次）
git commit -m "更新的版本说明"
git push -u origin master 
```
之后提交文件直接把项目文件放到这个目录下， 代码如下：

```
 git add .
git commit -m "更新版本说明"
git push -u origin master
```
OK搞定了！
