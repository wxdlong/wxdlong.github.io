---
title: "DNS issue"
date: 2019-06-17T21:58:01+08:00
draft: false
keywords: ["hugo","gitlab pages"]
description: "hugo 发布到gitlab pages上遇到的那些坑."
---
> 做生意讲信誉 ---- 理所当然


## Issue
　　ping IP successful, but ping host failed.

<!--more-->
```bash
[root@localhost ~]# vi /etc/hosts
[root@localhost ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.

64 bytes from 8.8.8.8: icmp_seq=3 ttl=38 time=72.9 ms

64 bytes from 8.8.8.8: icmp_seq=4 ttl=38 time=76.3 ms


^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 2 received, 50% packet loss, time 3000ms
rtt min/avg/max/mdev = 72.989/74.651/76.313/1.662 ms
[root@localhost ~]# ping www.baidu.com
ping: www.baidu.com: Temporary failure in name resolution
[root@localhost ~]# cat /etc/resolv.conf
nameserver 8.8.8.8

```