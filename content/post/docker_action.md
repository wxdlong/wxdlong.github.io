---
title: "Github Action of Docker研究"
date: 2019-09-15T00:28:57+08:00
Keywords: ["github","docker","action"]
draft: false
---
>不尚贤,使民不争; 不贵难得之货,使民不为盗；不见可欲,使民心不乱.
官方[Docker Action](https://help.github.com/cn/articles/creating-a-docker-container-action)教程创建了一个最基本的`Hello world`脚本,并没有和Repo代码有任何交互. 但是如果是想要引用代码,引用Github环境变量怎么办?   
仔细看这个Docker运行日志. 有惊奇的发现.



```bash
Run wxdlong/hello-action@v7
/usr/bin/docker run --name df7dc7a61e20181bb466a9f0ee3e0ddc3e675_091ef4 --label 0df7dc --workdir /github/workspace --rm -e INPUT_WHO-TO-GREET -e HOME -e GITHUB_REF -e GITHUB_SHA -e GITHUB_REPOSITORY -e GITHUB_ACTOR -e GITHUB_WORKFLOW -e GITHUB_HEAD_REF -e GITHUB_BASE_REF -e GITHUB_EVENT_NAME -e GITHUB_WORKSPACE -e GITHUB_ACTION -e GITHUB_EVENT_PATH -e RUNNER_OS -e RUNNER_TOOL_CACHE -e RUNNER_TEMP -e RUNNER_WORKSPACE -v "/var/run/docker.sock":"/var/run/docker.sock" -v "/home/runner/work/_temp/_github_home":"/github/home" -v "/home/runner/work/_temp/_github_workflow":"/github/workflow" -v "/home/runner/work/hello-action/hello-action":"/github/workspace" 0df7dc:7a61e20181bb466a9f0ee3e0ddc3e675  "wxdlong"
Hello wxdlong
 ::set-output name=time::Sat Sep 14 10:09:07 UTC 2019

```



<!--more-->

## 自动映射主机文件夹到容器内
1. `/var/run/docker.sock":"/var/run/docker.sock"`  这个`docker.sock`映射进去的话,Docker container里面可以看到主机上的Docker信息.如果主机把docker bin也映射到容器里的话. 就是一个[Docker in Docker]({{< ref "/post/dockerInDocker" >}})了


```bash
echo "pwd:  ${PWD}"
echo "ls -lth"
echo "Hello $1"
time=$(date)
echo ::set-output name=time::$time
```

```bash
Run wxdlong/hello-action@v7
/usr/bin/docker run --name df7dc7a61e20181bb466a9f0ee3e0ddc3e675_091ef4 --label 0df7dc --workdir /github/workspace --rm -e INPUT_WHO-TO-GREET -e HOME -e GITHUB_REF -e GITHUB_SHA -e GITHUB_REPOSITORY -e GITHUB_ACTOR -e GITHUB_WORKFLOW -e GITHUB_HEAD_REF -e GITHUB_BASE_REF -e GITHUB_EVENT_NAME -e GITHUB_WORKSPACE -e GITHUB_ACTION -e GITHUB_EVENT_PATH -e RUNNER_OS -e RUNNER_TOOL_CACHE -e RUNNER_TEMP -e RUNNER_WORKSPACE -v "/var/run/docker.sock":"/var/run/docker.sock" -v "/home/runner/work/_temp/_github_home":"/github/home" -v "/home/runner/work/_temp/_github_workflow":"/github/workflow" -v "/home/runner/work/hello-action/hello-action":"/github/workspace" 0df7dc:7a61e20181bb466a9f0ee3e0ddc3e675  "wxdlong"
Hello wxdlong
 ::set-output name=time::Sat Sep 14 10:09:07 UTC 2019

```

## 引用
创建Docker容器Action: https://help.github.com/cn/articles/creating-a-docker-container-action   