---
title: "发布Action到Github市场!"
date: 2019-09-08T10:49:34Z
Keywords: ["github","action","deploy"]
draft: false
---
>生而不有,为而不恃,功成而弗居. 无唯弗居,是以不去. 

独乐乐不如众乐乐, Github Action的一个创举, 共用!    
当你开发了一个好用的Action之后,可以发布到[Github Action市场](https://github.com/marketplace?type=actions)上以供千万人享用.实在是完美.    
看似简单的[发布教程](!https://developer.github.com/marketplace/actions/publishing-an-action-in-the-github-marketplace/), 却用了近两天时间才搞定我的第一个Action- [Hello Action](https://github.com/marketplace/actions/hello-action). 这里列出关键步骤.希望后来者能不要掉坑里.    
目前Action不需要审核,当你看到这张图时,说明己经成功! 

![IO](/jpg/201908/actionViewonMarket.png)
<!--more-->


## 发布Action入口
进入Repo code选项.点击`draft release`. 前提是你的项目里有`action.yml`    
![IO](/jpg/201908/draft_release.png)

## 配置双重身份认证.
一定要先配置双重身份认证,双重身份认证,[身份认证](https://help.github.com/cn/articles/configuring-two-factor-authentication ).重要的事情说三遍. 不然你点了`publish release`,github不报错,也不成功.   
![IO](/jpg/201908/releaseAction.png)

APP认证和短信认证我们选APP, 不为什么,因为人家SMS不会发给天朝.
APP推荐[Authy](https://authy.com/guides/github/),其它两款要Money,而且界面复杂.下载这个软件需要翻城.  
![IO](/jpg/201908/2fa.png)

## 完善Action信息
看到下面的图片,说明离成功只差一步!


![IO](/jpg/201908/publish_Ok.png)   
如果有错,会有红色提醒   
![IO](/jpg/201908/conflictAction.png)

## Repo名字不能有大写字母.
也许你会看到莫名奇妙的错,说下载不到Action. 比如之前我用了`helloAction`这个名字,死活不能成功.Github市场上也能看到发布成功.一开始看别人的名字都是用`-`命名.觉得不能是大小写的问题吧.后来只能报着试一试的态度,改了个名字居然成功了.    

所以大家也可以用 `https://api.github.com/repos/wxdlong/hello-action/tarball/v7` 这种链接打开看看Action是不是正常能用.
```bash
##[warning]Failed to download action 'https://api.github.com/repos/wxdlong/helloAction/tarball/master'. Error Response status code does not indicate success: 404 (Not Found).
##[warning]Back off 24.709 seconds before retry.
##[error]Response status code does not indicate success: 404 (Not Found).
```

![IO](/jpg/201908/action404.png)




## Docker镜像中脚本权限问题

Git上传时需要将脚本权限也上传上去.`starting container process caused "exec: \"/entrypoint.sh\": permission denied": unknown.`

```bash
total 16K
-rw-r--r-- 1 wxdlong wxdlong 371 Sep 14 13:31 action.yml
-rw-r--r-- 1 wxdlong wxdlong  80 Sep 14 13:31 Dockerfile
-rw-r--r-- 1 wxdlong wxdlong  77 Sep 14 13:31 entrypoint.sh
-rw-r--r-- 1 wxdlong wxdlong 316 Sep 14 13:31 README.md
```

## 终成功
如果Action里用Dockerfile这种方式Build镜像.最好不要有耗时的动作,否则会很慢哟.因为每次调用时都会Build一次.   
![IO](/jpg/201908/final_success.png)
 
## 引用

官方文档: https://help.github.com/en/articles/workflow-syntax-for-github-actions   
配置双重身份认证: https://help.github.com/cn/articles/configuring-two-factor-authentication  
推荐使用Authy认证: https://authy.com/guides/github/   
发布Action到github市场: https://developer.github.com/marketplace/actions/publishing-an-action-in-the-github-marketplace/    
我的第一个Action: https://github.com/wxdlong/hello-action