---
title: golang internal package内部包引发的问题
tags: TeXt
article_header:
  type: cover
---

### 问题
       事情这样的,由于同事引入一个github的第三方包(github.com/ks3sdklib/aws-sdk-go),


![avatar](/assets/images/posts/img.png)

### 然后开始谷歌 得到答案如下:
    Go中命名为internal的package，只有该package的父级package才可以访问该package的内容。

    两点需要注意：

    只有直接父级package，以及父级package的子孙package可以访问，其他的都不行，再往上的祖先package也不行
    父级package也只能访问internal package使用大写暴露出的内容，小写的不行


### 找同事想办法 得到的答案是：
    那就手动把go mod目录下 github.com/ks3sdklib/aws-sdk-go/service/s3 这个包删掉，然后再
    清一下go mod缓存
    把goproxy改成：GOPROXY=https://goproxy.io
    重新启动IDE

### 最后的最后
    果然是go mod cache 的问题会一直访问之前的包 因此必须彻底的将pkg下的包删除 包括cache 里面的包
    然后重新下载包

<!--more-->
