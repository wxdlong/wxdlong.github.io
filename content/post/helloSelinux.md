---
title: "Hello Selinux!"
date: 2019-08-31T11:46:54+08:00
draft: true
---

> 道可道，非常道。名可名，非常名。无名，天地之始。有名，万物之母。

查看进程SELlinux 标签 `ps -eZ`

``` bash
[root@localhost html]# ps -eZ | grep httpd
system_u:system_r:unconfined_service_t:s0 7415 ? 00:00:00 httpd
system_u:system_r:unconfined_service_t:s0 7416 ? 00:00:00 httpd
system_u:system_r:unconfined_service_t:s0 7417 ? 00:00:00 httpd
system_u:system_r:unconfined_service_t:s0 7418 ? 00:00:00 httpd
system_u:system_r:unconfined_service_t:s0 7419 ? 00:00:00 httpd
system_u:system_r:unconfined_service_t:s0 7420 ? 00:00:00 httpd
```

还原默认标签

``` bash
[root@localhost tmp]# restorecon -v /usr/sbin/httpd 
restorecon reset /usr/sbin/httpd context system_u:object_r:bin_t:s0->system_u:object_r:httpd_exec_t:s0
```

确认 `/usr/sbin/httpd` 己经打上标签 `httpd_exec_t` 

``` bash
[root@localhost tmp]# ls -Z /usr/sbin/httpd 
-rwxr-xr-x. root root system_u:object_r:httpd_exec_t:s0 /usr/sbin/httpd
```

`systemctl restart httpd` 重启Httpd服务, 查看httpd进程selinux context

``` bash
[root@localhost tmp]# ps -eZ | grep httpd
system_u:system_r:httpd_t:s0    15738 ?        00:00:00 httpd
system_u:system_r:httpd_t:s0    15739 ?        00:00:00 httpd
system_u:system_r:httpd_t:s0    15740 ?        00:00:00 httpd
system_u:system_r:httpd_t:s0    15741 ?        00:00:00 httpd
system_u:system_r:httpd_t:s0    15742 ?        00:00:00 httpd
system_u:system_r:httpd_t:s0    15743 ?        00:00:00 httpd
```

``` bash

[root@localhost tmp]# semanage login -l

Login Name           SELinux User         MLS/MCS Range        Service

__default__          unconfined_u         s0-s0:c0.c1023       *
root                 unconfined_u         s0-s0:c0.c1023       *
system_u             system_u             s0-s0:c0.c1023       *

```

查看用户SeLinux context

``` bash
[newuser@localhost tmp]$ id -Z
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

|  User   | Role  | Domain | XWindow System | su or sudo |  Execute in home or /tmp | Networking
|  ----  | ----  | ----  |  ----  |  ----  |  ----  | ----  | 
| sysadm_u  | sysadm_r | sysadm_t | yes | su and sudo |  yes | yes 
| staff_u  | staff_r | staff_t | yes | only sudo | yes | yes
| user_u  | user_r | user_t | yes | no | yes | yes 
| guest_u  | guest_r | guest_t | no | no | no | no
| xguest_u | xguest_r | xguest_t | yes | no | no | Firefox_only


安装setools `yum install -y setools-console` 

``` bash
[root@localhost tmp]# seinfo -r

Roles: 14
   auditadm_r
   dbadm_r
   guest_r
   staff_r
   user_r
   logadm_r
   object_r
   secadm_r
   sysadm_r
   system_r
   webadm_r
   xguest_r
   nx_server_r
   unconfined_r
```

检测selinux标签正确性
```bash
[root@localhost sudoers.d]# matchpathcon -V /var/www/html/*
/var/www/html/testfile has context unconfined_u:object_r:samba_share_t:s0, should be system_u:object_r:httpd_sys_content_t:s0
```

修改标签 `chcon -t bin_t /usr/sbin/httpd`