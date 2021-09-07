---
title: github token
tags: TeXt
article_header:
  type: cover
  image:
    src: images/jiegeng.jpeg
---

### github 免密
- github中的免密登录方式有两种
   - ssh秘钥免密
   - httpBasic http://username:password@gitxxxxx.git

### github token
- 但是github更新了安全机制强制需要使用 github token

### github token 免密登录
   - 路径是： github 个人中心 --> setting --> Developer settings --> Personal access tokens
   - 然后勾选你的权限选项 勾选后下一步 生成token
   - 注意保存你的token不要泄露  这是访问你仓库的凭证
   - 然后 git clone https://${tokne}@gitxxxxx.git 就开始免密登录


<!--more-->
