---
title: Prometheus 查询按特定标签获取平均值 最大值 tags: TeXt article_header:
type: cover
---

### 在网络中获取一个机房打其他的延迟数据写入普罗米修斯

- 获取当前到各大机房的延迟
  ```PromQL
    probe_icmp_duration_seconds{phase='rtt',dest_region=~'^*.*',src_region='华为云-北京'}
  ```
- 获取当前到各大机房的丢包
  ```PromQL
    1 - avg_over_time(probe_success{src_region='华为云-北京',dest_region=~'^*.*'}[%ds])
  ```

### 延迟  丢包 max avg

  ```PromQL
  max_over_time(probe_icmp_duration_seconds{phase='rtt',dest_region=~'^*.*',src_region='华为云-北京'}[%ds])
	avg_over_time(probe_icmp_duration_seconds{phase='rtt',dest_region=~'^*.*',src_region='华为云-北京'}[%ds])
	1 - max_over_time(probe_success{src_region='华为云-北京',dest_region=~'^*.*'}[%ds])
	1 - avg_over_time(probe_success{src_region='华为云-北京',dest_region=~'^*.*'}[%ds])
  ```

<!--more-->
