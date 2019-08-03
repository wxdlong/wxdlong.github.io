---
title: "Top 命令高级用法实例"
date: 2018-12-24T10:31:14+08:00
draft: false
Keywords: ["top","linux命令"]
categories: [linux]
tags: [shell]
---


## Top
> 监控linux系统状态命令,CPU MEM
top命令是Linux下常用的性能分析工具，能够实时显示系统中各个进程的资源占用状况，类似于Windows的任务管理器。

top显示系统当前的进程和其他状况,是一个动态显示过程,即可以通过用户按键来不断刷新当前状态.如果在前台执行该命令,它将独占前台,直到用户终止该程序为止. 比较准确的说,top命令提供了实时的对系统处理器的状态监视.它将显示系统中CPU最“敏感”的任务列表.该命令可以按CPU使用.内存使用和执行时间对任务进行排序；而且该命令的很多特性都可以通过交互式命令或者在个人定制文件中进行设定. 

```bash
top - 09:09:50 up 21:38,  0 users,  load average: 1.37, 1.02, 0.83
Tasks:  17 total,   2 running,  15 sleeping,   0 stopped,   0 zombie
Cpu(s): 25.9%us,  1.9%sy,  0.0%ni, 71.3%id,  0.4%wa,  0.0%hi,  0.4%si,  0.0%st
Mem:   8907600k total,  7265328k used,  1642272k free,   244412k buffers
Swap:  2097148k total,        0k used,  2097148k free,  5089264k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
 1743 root      20   0  7400  468  404 R 100.0  0.0   4:49.80 md5sum /dev/zero
    1 root      20   0 11360 2380 2164 S  0.0  0.0   0:00.01 /bin/bash /usr/local/bin/docker-entrypoint.sh
   93 root      20   0 66752 5968 5212 S  0.0  0.1   0:00.01 /usr/sbin/sshd -D
   94 root      20   0 14784 3124 2744 S  0.0  0.0   0:00.01 bash
  192 root      20   0 14784 3016 2752 S  0.0  0.0   0:00.01 bash
  209 root      20   0 47836 3320 2968 S  0.0  0.0   0:00.00 su - postgres
  210 postgres  20   0 14780 3068 2708 S  0.0  0.0   0:00.01 -bash
 1185 root      20   0 14784 3120 2724 S  0.0  0.0   0:00.14 bash
 1377 postgres  20   0  227m  19m  18m S  0.0  0.2   0:00.15 /usr/pgsql-9.6/bin/postgres
 1378 postgres  20   0 85356 3828 2748 S  0.0  0.0   0:00.00 postgres: logger process
 1380 postgres  20   0  227m 6536 5412 S  0.0  0.1   0:00.04 postgres: checkpointer process
 1381 postgres  20   0  227m 4356 3252 S  0.0  0.0   0:00.17 postgres: writer process
 1382 postgres  20   0  227m 9404 8288 S  0.0  0.1   0:00.34 postgres: wal writer process
 1383 postgres  20   0  227m 7280 5928 S  0.0  0.1   0:00.11 postgres: autovacuum launcher process
 1384 postgres  20   0 87456 3788 2684 S  0.0  0.0   0:00.02 postgres: archiver process
 1385 postgres  20   0 87600 4796 3640 S  0.0  0.1   0:00.20 postgres: stats collector process
 1741 root      20   0 14952 2028 1800 R  0.0  0.0   0:00.12 top
```

1. 第一行 `top - 09:09:50 up 21:38,  0 users,  load average: 1.37, 1.02, 0.83`   
   top - 当前时间,系统运行时间,多少个用户登陆,1,5,15分钟平均负载

2. 第二行 `Tasks:  17 total,   2 running,  15 sleeping,   0 stopped,   0 zombie`  
   任务数： 总共数，运行数，休眠数，停止数，僵尸数

3. 第三行，`Cpu(s): 25.9%us,  1.9%sy,  0.0%ni, 71.3%id,  0.4%wa,  0.0%hi,  0.4%si,  0.0%st`  
   CPU:  用户空间占用CPU的百分比  
         内核空间占用CPU的百分比  
         改变过优先级的进程占用CPU的百分比  
         空闲CPU百分比    
         IO等待占用CPU的百分比   
        硬中断（Hardware IRQ）占用CPU的百分比  
        软中断（Software Interrupts）占用CPU的百分比

4. 第四行 `Mem:   8907600k total,  7265328k used,  1642272k free,   244412k buffers`
    内存： 总内存量， 使用量， 空闲数， 缓存量

5. 第五行 `Swap:  2097148k total,        0k used,  2097148k free,  5089264k cached` 
   交换内存： 总量， 用量， 空闲数, 缓存量

6. 第六行 ` PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND`   
   进程，  
   所属用户，  
   优先级，  
   nice值， 负值表示高优先级，正值表示低优先级  
   VIRT — 进程使用的虚拟内存总量，单位kb。VIRT=SWAP+RES  
    RES — 进程使用的、未被换出的物理内存大小，单位kb。RES=CODE+DATA 
    SHR — 共享内存大小，单位kb   
    S — 进程状态。D=不可中断的睡眠状态 R=运行 S=睡眠 T=跟踪/停止 Z=僵尸进程
    %CPU — 上次更新到现在的CPU时间占用百分比   
    %MEM — 进程使用的物理内存百分比   
    TIME+ — 进程使用的CPU时间总计，单位1/100秒   
    COMMAND — 进程名称（命令名/命令行）   


## 多核CPU监控

> 在top基本视图中，按键盘数字“1”，可监控每个逻辑CPU的状况
```bash
   top - 09:48:36 up 22:17,  0 users,  load average: 0.55, 0.50, 0.90
Tasks:  16 total,   1 running,  15 sleeping,   0 stopped,   0 zombie
Cpu0  :  0.7%us,  1.0%sy,  0.0%ni, 97.6%id,  0.7%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu1  :  0.7%us,  1.4%sy,  0.0%ni, 97.6%id,  0.3%wa,  0.0%hi,  0.0%si,  0.0%st
Cpu2  :  4.0%us,  3.7%sy,  0.0%ni, 91.9%id,  0.0%wa,  0.0%hi,  0.3%si,  0.0%st
Cpu3  :  1.3%us,  1.0%sy,  0.0%ni, 97.3%id,  0.0%wa,  0.0%hi,  0.3%si,  0.0%st
```

## 进程字段排序

> 默认以CPUh占用量来排序
1. 按`b` 打开关闭加粗效果
2. 按`x` 打开排序列的高亮
3. 按`shift+> , shift+<` 可以向右，向左改变排序列。

## 彩色显示
> 按`z` 


## 改变进程显示列
> 按`f` 编辑显示列,下面是所有可显示列名

```bash
Current Fields:  AEHIOQTWKNMbcdfgjplRSuvyzX  for window 1:Def
Toggle fields via field letter, type any other key to return

* A: PID        = Process Id
* E: USER       = User Name
* H: PR         = Priority
* I: NI         = Nice value
* O: VIRT       = Virtual Image (kb)
* Q: RES        = Resident size (kb)
* T: SHR        = Shared Mem size (kb)
* W: S          = Process Status
* K: %CPU       = CPU usage
* N: %MEM       = Memory usage (RES)
* M: TIME+      = CPU Time, hundredths
  b: PPID       = Parent Process Pid
  c: RUSER      = Real user name
  d: UID        = User Id
  f: GROUP      = Group Name
  g: TTY        = Controlling Tty
  j: P          = Last used cpu (SMP)
  p: SWAP       = Swapped size (kb)
  l: TIME       = CPU Time
* R: CODE       = Code size (kb)
* S: DATA       = Data+Stack size (kb)
  u: nFLT       = Page Fault count
  v: nDRT       = Dirty Pages count
  y: WCHAN      = Sleeping in Function
  z: Flags      = Task Flags <sched.h>
* X: COMMAND    = Command name/line
```

## 帮助
> 按h

```bash
Help for Interactive Commands - procps version 3.2.8
Window 1:Def: Cumulative mode On.  System: Delay 3.0 secs; Secure mode Off.

  Z,B       Global: 'Z' change color mappings; 'B' disable/enable bold
  l,t,m     Toggle Summaries: 'l' load avg; 't' task/cpu stats; 'm' mem info
  1,I       Toggle SMP view: '1' single/separate states; 'I' Irix/Solaris mode

  f,o     . Fields/Columns: 'f' add or remove; 'o' change display order
  F or O  . Select sort field
  <,>     . Move sort field: '<' next col left; '>' next col right
  R,H     . Toggle: 'R' normal/reverse sort; 'H' show threads
  c,i,S   . Toggle: 'c' cmd name/line; 'i' idle tasks; 'S' cumulative time
  x,y     . Toggle highlights: 'x' sort field; 'y' running tasks
  z,b     . Toggle: 'z' color/mono; 'b' bold/reverse (only if 'x' or 'y')
  u       . Show specific user only
  n or #  . Set maximum tasks displayed

  k,r       Manipulate tasks: 'k' kill; 'r' renice
  d or s    Set update interval
  W         Write configuration file
  q         Quit
          ( commands shown with '.' require a visible task display window )
Press 'h' or '?' for help with Windows,
```