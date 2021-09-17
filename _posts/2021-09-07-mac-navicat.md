---
title: mac navicat无限试用
tags: TeXt
article_header:
  type: cover
---

### mac navicat无限试用
- 申明  本人支持正版 无奈暂时用不起 只能无限试用 有人问为何不破解 因为公司原因不能使用盗版
   - 直接说步骤
     1. 删除注册表
     2. 删除隐藏文件
     3. 需要plist编辑工具
        1. 需要破解 plistPro edit 图形化
        2. 免费的是自带的工具 /usr/libexec/PlistBuddy 命令行

   - 开始操作
    ```shell
     open ~/Library/Preferences/com.prect.NavicatPremium15.plist
    ```
  可以看到注册表里面有下面的字段，直接删除标红部分的内容
  ![avatar](https://oss-emcsprod-public.modb.pro/wechatSpider/modb_20210521_02e1301c-ba00-11eb-9090-00163e068ecd.png)

  ```shell
     cd ~/Library/Application\ Support/PremiumSoft\ CyberTech/Navicat\ CC/Navicat\ Premium/
     ls -a
     # ls这个文件里面的隐藏文件，然后删除 隐藏文件名有差异 一般下面就一个很长的文件隐藏文件
     rm -rf .BA51237A5534754C38BA80C35742EAF3
  ```
    ！！！如果失败多试几次
<!--more-->
