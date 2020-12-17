---
title: "理解5G RNTI"
date: 2020-12-17T21:43:40+08:00
draft: false
keywords: ["5G","RNTI"]
description: "5G RNTI解释"
categories: [5G]
tags: [5G]
---


>大成若缺，其用不弊。大盈若冲，其用不穷。大直若刷，大巧若拙，大辩若讷。静胜躁，寒胜热。清静为天下正。

RNTI, Radio Network Temporary Identifier, 是一种标识，两类用途：用于加扰DCI的CRC,UE只有使用正确的RNTI才能对接收到的消息解码；基站用于识别UE,UE发送的RRCmessage会携带自己专用的RNTI值。
<!--more-->

RNTI | Usage 
-----| ---------
 P-RNTI    |  Paging and System Information change notification   
 SI-RNTI    |   Broadcast of System Information    
 Temporary C-RNTI    |   Contention Resolution    
 RA-RNTI             |  Random Access Response   