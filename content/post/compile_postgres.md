---
title: "Linux connect to Smba"
date: 2019-09-08T10:49:34Z
Keywords: ["linux","smba"]
draft: true
---
>天下皆知美之为美，斯恶己；皆知闰之为病，斯不美己。 故有无相生，难易相成， 长短相形，
高下相轻，音声相和，前后相随。   

How to connect to smba in Linux? Let's begin.

`smbclient -L //{IP} -U {userName}`, and you can see the shareName is `Chainedbox`
```bash
configure: error: readline library not found
If you have readline already installed, see config.log for details on the
failure.  It is possible the compiler isn't looking in the proper directory.
Use --without-readline to disable readline support.


checking for inflate in -lz... yes
checking for CRYPTO_new_ex_data in -lcrypto... no
configure: error: library 'crypto' is required for OpenSSL

```

<!--more-->

```bash
[root@bfda43afd5db psql]# yum search readline
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: mirrors.cn99.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.cn99.com
================================================================== N/S matched: readline ==================================================================
readline-devel.i686 : Files needed to develop programs which use the readline library
readline-devel.x86_64 : Files needed to develop programs which use the readline library
readline-static.i686 : Static libraries for the readline library
readline-static.x86_64 : Static libraries for the readline library
perl-Term-UI.noarch : Term::ReadLine user interface made easy
readline.i686 : A library for editing typed command lines
readline.x86_64 : A library for editing typed command lines

  Name and summary matches only, use "search all" for everything.

```

smba commands

```bash
smb: \> help
?              allinfo        altname        archive        backup         
blocksize      cancel         case_sensitive cd             chmod          
chown          close          del            dir            du             
echo           exit           get            getfacl        geteas         
hardlink       help           history        iosize         lcd            
link           lock           lowercase      ls             l              
mask           md             mget           mkdir          more           
mput           newer          notify         open           posix          
posix_encrypt  posix_open     posix_mkdir    posix_rmdir    posix_unlink   
posix_whoami   print          prompt         put            pwd            
q              queue          quit           readlink       rd             
recurse        reget          rename         reput          rm             
rmdir          showacls       setea          setmode        scopy          
stat           symlink        tar            tarmode        timeout        
translate      unlock         volume         vuid           wdel           
logon          listconnect    showconnect    tcon           tdis           
tid            logoff         ..             !  

```
Install `apt-get install cifs-utils`

```bash
wxdlong@wxdlong:~$ sudo apt-get install cifs-utils
[sudo] wxdlong 的密码：
正在读取软件包列表... 完成
正在分析软件包的依赖关系树       
正在读取状态信息... 完成       
建议安装：
  keyutils winbind
下列【新】软件包将被安装：
  cifs-utils
升级了 0 个软件包，新安装了 1 个软件包，要卸载 0 个软件包，有 15 个软件包未被升级。
需要下载 75.8 kB 的归档。
解压缩后会消耗 236 kB 的额外空间。
获取:1 http://packages.deepin.com/deepin lion/main amd64 cifs-utils amd64 2:6.7-1 [75.8 kB]
已下载 75.8 kB，耗时 9秒 (7,659 B/s)                                                                                   
正在选中未选择的软件包 cifs-utils。
(正在读取数据库 ... 系统当前共安装有 190299 个文件和目录。)
正准备解包 .../cifs-utils_2%3a6.7-1_amd64.deb  ...
正在解包 cifs-utils (2:6.7-1) ...
正在设置 cifs-utils (2:6.7-1) ...
update-alternatives: 使用 /usr/lib/x86_64-linux-gnu/cifs-utils/idmapwb.so 来在自动模式中提供 /etc/cifs-utils/idmap-plugin (idmap-plugin)
正在处理用于 man-db (2.7.6.1-2) 的触发器 ...
```

mount to /mnt `sudo mount -t cifs //{IP}/{shareName} -o username={userName},password={password},vers=${version} /{mountPoint}`

```bash
sudo mount -t cifs //192.168.31.166/Chainedbox -o username=yhtx,password=5913,vers=1.0 /mnt

```

>If not identify version(`vers=1.0`), may be `mount error(112): Host is down`


mount at boot  `//{IP}/{ShareName}   /{mountPoint}   cifs  credentials=/etc/samba/user.cred 0 0`

```bash

echo "//192.168.31.166/Chainedbox                     /mnt            cifs            credentials=/etc/samba/user.cred 0 0" >> /etc/fstab
```

create `user.cred` like below
```bash
root@wxdlong:/mnt# vi /etc/samba/user.cred
root@wxdlong:/mnt# cat /etc/samba/user.cred
username=yhtx
password=5913
```

`chmod 600 `to the file `user.cred`
```bash
root@wxdlong:/mnt# chmod 600 /etc/samba/user.cred 
root@wxdlong:/mnt# ls -lth /etc/samba/user.cred 
-rw------- 1 root root 29 Sep  8 19:08 /etc/samba/user.cred
```