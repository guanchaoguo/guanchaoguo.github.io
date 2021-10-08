---
title: gitlab发生迁移 主机 IP改变 ssh免密失效
tags: TeXt
article_header:
  type: cover
---

### 背景需求
        公司对GitLab CE 12.5.4 -> GitLab CE 13.11.4 升级后IP改变 导致ssh免密失效报错如下
```shell
  @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
SHA256:2rAd4Uy45Mm8txcdfvJt7MWhbVVXJG/eZP3R+oCPB3E.
Please contact your system administrator.
Add correct host key in /Users/zhanghao/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/zhanghao/.ssh/known_hosts:3
RSA host key for 192.168.＊.＊ has changed and you have requested strict checking.
Host key verification failed.
```
####  处理方式有两种
- 删除原本的ssh key(慎用) 会导致其他公钥失效 如堡垒机
- 修改 ~/.ssh/known_hosts gitlab域名下全部认证

### 删除原本的ssh key
```shell
##  删除原本的ssh key
rm -rf ~/.ssh/known_hosts
### 生成新的ssh key 一路回车，需要选择的时候输入y
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
## 测试ssh key连接
ssh -T git@xxxx.com

## 如果需要认证指纹
ssh-keygen -R xxxx.com
```

##### 修改gitlab域名认证
```shell
vim  -rf ~/.ssh/known_hosts

### 删除改域名开头的行数

### 执行新的只问你认证
ssh-keyscan -t rsa xxxx.com >> ~/.ssh/known_hosts
```

<!--more-->
