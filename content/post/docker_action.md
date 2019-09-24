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

## 映射工作区至容器内
`/home/runner/work/_temp/_github_home: /github/home`,并且设置容器内默认工作区. `--workdir /github/workspace`.这样的话,容器内部可以访问其它Action的内容.比如`checkout`出来的源码    


## 传入各种环境变量 

变量 | 含义 | 示例
--  | --  |  --
INPUT_WHO-TO-GREET | action定义的变量 | `wxdlong`
HOME   | HOME path | `/github/home`
GITHUB_REF |   | EMPTY
GITHUB_SHA | git commit  | `359f48e3252`
GITHUB_REPOSITORY |  repo name | `wxdlong/hello-action`
GITHUB_ACTOR | repo owner | `wxdlong`
GITHUB_WORKFLOW | workflow name | `Hello Action`
GITHUB_HEAD_REF | git ref | EMPTY
GITHUB_BASE_REF |         | EMPTY
GITHUB_REF |   git ref      | `refs/heads/master`
GITHUB_EVENT_NAME |  事件名称 | `push`
GITHUB_ACTION |  action name | `wxdlonghello-action`
GITHUB_EVENT_PATH |        | `/github/workflow/event.json`
RUNNER_OS |  运行的操作系统 | `Linux`
RUNNER_TOOL_CACHE |      | `/opt/hostedtoolcache`
RUNNER_WORKSPACE | 运行空间 | `/home/runner/work/hello-action`

## Docker Image

Action中`image`类型可以是`Dockerfile`,``

```yml
runs:
  using: 'docker'
  image: 'Dockerfile'
  args:
    - ${{ inputs.who-to-greet }}
```

## 引用
创建Docker容器Action: https://help.github.com/cn/articles/creating-a-docker-container-action   