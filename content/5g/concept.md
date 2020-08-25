---
title: "5G 基础概念!"
date: 2020-08-04T20:34:42+08:00
draft: false
keywords: ["5G","概念"]
description: "5G词汇"
categories: [5G]
tags: [5G]
---


https://www.jianshu.com/p/f9c013ac8eff
>常使民无知无欲, 使夫知者不敢为也. 也无为, 则无不治.  

`set -x`, Print commands and their arguments as they are executed.   
`set -e`, Exit immediately if a command exits with a non-zero status.   
`+-`, Using + rather than - causes these flags to be turned off.   
`set -o`  ## 查看当前选项的设置状态    
Ctrl开头的快捷键一般是针对字符的，而Alt开头的快捷键一般是针对词的. 记住一些快捷键.让你的生活更愉快哟.
<!--more-->
## 词汇

#. AAU: Active Antena Unit, 有源天线单元。 BBU的部分物理层处理功能与原RRU及无线合并为AAU
#. CU: Centralized Unit, 集中单元。 原BBU的非实时部分将分割出来，重新定义为CU，负责处理非实时协议和服务。
#. DU: Distrubute Unit, 分布单元。 BBU的剩余功能重新定义为DU，负责处理物理层协议和实时服务。
#. EPC: 4G核心网
#. MEC: Mobile Ddge Computing 移动网络边界计算平台
#. 5G: 5th generation
#. eMBB: 增强移动宽带
#. URLLC: 低时延高可靠
#. mMTC: 海量大连接
#. Millimeter Wave: 毫米波
#. Massive MIMO: 大规模天线技术
#. NFV: Network Function Virtualization. 网络功能虚拟化
#. NFV: Software Defined Network. 软件定义网络
#. NR: New Radio. 新空口技术
#. PDCP: 用户面在分组数据汇聚协议层
#. Fronthaul: AAU-DU,前传。 传递无线侧网元设备AAU和DU间的数据
#. Middlehaul: DU-CU,中传。 传递无线侧网元设备DU和CU间的数据
#. Backhaul: CU-NetCore,回传。 传递无线侧网元设备CU和核心网网元间的数据
#. ICN: Information core network. 信息中心网络
#. NSA: Non Standalone 非独立组网 
#. SA: Standalone 独立组网 
#. CCFD: Co-time Co-frequency Full Duplex. 5G全双工 
#. CDN: Content Delivery Network. 内容分发网络 
#. CoMP: Coordinated Multiple Points Transmission/Reception。多点协作传输 
#. RRU: 
#. PHY: 
#. 5GC: 5G Core Network
#. MME: Mobility Management Entity. 移动管理实体
#. PGW: PDN Gateway. PDN网关
#. SBA: Service Base Architecture. 基于服务的架构
#. NEF: 
#. NRF
#. PCF
#. UDM
#. AF: Application Function
#. AUSF: Authentication Server Function 认证服务器功能
#. CP: Control Plan 
#. DN: DataNetwork
SRB: Signalling Radio Bearer, 承载控制面数据(Control Plane Data, RRC消息和NAS消息)
DRB: Data Radio Bearer 
DCI: Downlink Control Information
UPD: User Plan Data
CPD: Control Plane Data
LC: Logical Channel, 逻辑信道
TC: Transport Channel, 传输信道 UL-SCH, DL-SCH
C-RNTI: Cell Radio Network Temporary Identifier
PC: Physical Channel, 物理信道  PUSCH, PDSCH
EIR: Equipment identity Register
GPSI: Generic Public Subscription Identifier
GTP-U: GPRS Tunneling Protocol - User Plane
GUTI: Globally Unique Temporary Identifier
IMS: IP Multimedia Subsystem
MO: Mobile Originated 
MT: Mobile Terminated
NEF: Network Exposure Function  网络能力开放
NG-RAN: Next Generation RAN
NRF: Network Repository Function  网络注册功能
NSSF: Network Slice Selection Function  网络切片选择功能
PFCP: Packet Forwarding Control Protocol
PCF: Policy Control Function 策略控制功能
PDU: Package Data Unit
PEI: Permanent Equipment Identity
RNTI: Radio Network Temporary Identity
SEPP: Security Edge Protection Proxy
SMF: Session Management Function
SMSF: SMS Function
BCH: Broadcast Channel
SUPI: Subscription Permantent Identifier
UDM: Unified Data Management
UDR: Unified Data Respository
QoS: Quality of Service
NCGI: NR Cell Global Identifier
SSB: Synchronisation Signal Block
CSI-RS: Channel-State information reference signal, 同步信号块
BWP: Bandwidth Part 部分带宽
NCR: Neighbour Cell Relation
NID: Network Identifier
PCH: Paging Channel
PCI: Physical Cell Identifier
PDCCH: Physical Downlink Control Channel
PDSCH: Physical Downlink Shared Channel
PLMN Cell: Public Land Mobile Network Cell
PRACH: Physical Random Access Channel, 随机接入物理信道
MOCN: Multi-Operator Core Network, 即一套无线网络可以同时连接到多个运营商的核心网节点，实现多家运营商共享同一套无线网络，这种共享方式的优点是可以在节省投资和网络独立两个方面找到 个平衡点，两者得到很好的兼顾，，中国联通和中国电信5G网络共建共享的方式就是采用MOCN.
OFDM: Orthogonal Frequency Division Multiplexing,正交频分复用
ZC: Zadoff-Chu Series， ZC序列
CAZAC: Constant Amplitude Zero Auto-Correlation
PUCCH: Physical Uplink Control Channel
PUSCH: Physical Uplink Shared Channel
TA: Timing Advance
NAS: Non-Access Stratum, 非接入层
AS: Access Stratum,接入层 无线接入网采用的协议
RRC: Radio Resource Control 无线资源控制协议。处理UE和ENodeB之间控制平面的第三层信息
SCTP: Stream Control Transmission Protocol 
RLC: Radio Link Control
PDCP: Packet Data Convergence Protocol, 分组数据汇聚协议。只负责处理分组业务数据。
PHY: 层一，物理层
MAC: Medium Access Control 层二，媒体接入控制层
MCG: Master Cell Group, 主小区组
SCG: Secondary Cell group, 辅小区组
PCell: Primary Cell,主小区
SCell: Secondary Cell, 辅小区
PSCell: Primary Secondary Cell,主辅小区
RACH: Random Access Channel, 随机接入信道, 是一种上行传输信道
#. AMF: Access and Mobility Management Function 接入和移动性管理

#. SMF:
#. UE: User Equipment
#. 5GS: 5G System
#. (R)AN: (Radio) Access Network
#. UPF: User Plan Function
#. USIM: Universal Subscriber Identity Module




## 移动光标
`Ctrl + a` ：移到命令行首   
`Ctrl + e` ：移到命令行尾   
Ctrl + f ：按字符前移（右向）   
Ctrl + b ：按字符后移（左向）   
`Alt + f` ：按单词前移（右向）   
`Alt + b` ：按单词后移（左向）   
Ctrl + xx：在命令行首和光标之间移动   
`Ctrl + u` ：从光标处删除至命令行首   
`Ctrl + k` ：从光标处删除至命令行尾   
`Ctrl + w` ：从光标处删除至字首   
`Alt + d` ：从光标处删除至字尾   
Ctrl + d ：删除光标处的字符   
Ctrl + h ：删除光标前的字符   
Ctrl + y ：粘贴至光标后   
Alt + c ：从光标处更改为首字母大写的单词   
Alt + u ：从光标处更改为全部大写的单词   
Alt + l ：从光标处更改为全部小写的单词   
Ctrl + t ：交换光标处和之前的字符     
Alt + t ：交换光标处和之前的单词    
Alt + Backspace：与 Ctrl + w 相同类似，分隔符有些差别  


## 执行命令
`Ctrl + r`：逆向搜索命令历史   
Ctrl + g：从历史搜索模式退出   
Ctrl + p：历史中的上一条命令   
Ctrl + n：历史中的下一条命令   
Alt + .：使用上一条命令的最后一个参数  

## 控制命令
`Ctrl + l`：清屏   
Ctrl + o：执行当前命令，并选择上一条命令   
Ctrl + s：阻止屏幕输出   
Ctrl + q：允许屏幕输出   
Ctrl + c：终止命令   
Ctrl + z：挂起命令  


## Bang (!) 命令

!!： 执行上一条命令   
!blah： 执行最近的以 blah 开头的命令，如 !ls   
!blah:p： 仅打印输出，而不执行   
!$： 上一条命令的最后一个参数，与 Alt + . 相同   
!$:p： 打印输出 !$ 的内容   
!*： 上一条命令的所有参数   
!*:p： 打印输出 !* 的内容   
^blah： 删除上一条命令中的 blah   
^blah^foo： 将上一条命令中的 blah 替换为 foo   
^blah^foo^： 将上一条命令中所有的 blah 都替换为 foo   