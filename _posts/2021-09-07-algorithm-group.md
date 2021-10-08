---
title: 自定义分组函数算法
tags: TeXt
article_header:
  type: cover
---

### 背景需求
        首先mysql中存在一些告警日志，每分钟告警数。直接 select group 就能好了。
     但是数据中只有三个字 id content ctime 然后 content类型text 具体是一个json，json里面是有一个message 里面包含新消息 test 不参与统计
     这样模糊匹配都用不上了,担心里存在其他类似的字段 只能在程序里面处理掉



<!--more-->
