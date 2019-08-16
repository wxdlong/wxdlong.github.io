---
title: "Jconsole,Jvisualvm远程连接Wildfly"
date: 2019-08-16T22:43:28+08:00
Keywords: ["wildfly","jconsole","jvisualvm"]
draft: false
---
>道可道，非常道！   

　　Wildfly运行在远程机器上，想要调试性能问题怎么办？ 用JDK自带的Jconsole,JvisualVM连上去查看最方便不过了。
不过单纯连是连不上的。必须要在ClassPath上加上Wildfly的一个jboss-cli-client.jar包，然后就能连上了。本人己经
封装好一个[wildfly Docker container](!),现在来Demo一下。
<!--more-->

##Run Wildfly Docker
　　运行Wildfly时映射本地盘`/d/code/temp`到Container host路径上。以便container Copy Wildfly相关Jar包出来。否则也可以手动Copy到本地。
`docker run -itd --rm -p 9990:9990 -v /d/code/temp:/host wxdlong/wildfly:17`

1. 检查Wildfly是否启动成功。打开`http://localhost:9990/console/index.html`,默认用户名：wxdlong,密码：12345678  
![IO](/jpg/201908/wildflyIndex.jpg)

2. 打开映射到本地的`/d/code/temp/bin/jconsole.bat`(双击). 链接地址：service:jmx:http-remoting-jmx://localhost:9990    
![IO](/jpg/201908/jconsoleLogin.jpg)

3. 看，连上了哟。  
![IO](/jpg/201908/jconsoleDetailjpg)

4. 连接jvisualVM, `jvisualvm.exe --cp:a ./jboss-cli-client.jar`
    ```bash
    wxdlong@wxdl MINGW64 /d/code/temp/bin/client
    $ ls
    jboss-cli-client.jar  jboss-client.jar  README-CLI-JCONSOLE.txt  README-EJB-JMS.txt

    wxdlong@wxdl MINGW64 /d/code/temp/bin/client
    $ /d/libs/jdk/bin/jvisualvm.exe --cp:a ./jboss-cli-client.jar
    The launcher has determined that the parent process has a console and will reuse it for its own console output.
    Closing the console will result in termination of the running program.
    Use '--console suppress' to suppress console output.
    Use '--console new' to create a separate console window.
    ```

5. 看，又连上了哟。
![IO](/jpg/201908/jvisualVmDetail.jpg)