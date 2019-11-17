---
title: "编译全静态无依赖GO可执行程序"
date: 2019-08-04T22:54:59+08:00
Keywords: ["go","静态","Docker"]
draft: false
---


　　[Go](!https://golang.org)非常强悍的新兴语言， 未来大数据，云计算的天下，非GO莫属。 编译一个无任何动态链接库依赖的程序是非常有用的，因为你不知道你的程序将会在什么环境上运行。而且结合Docker这个运行平台，静态可执行程序有你意想不到的效果！


## 编译命令

`go install -v -ldflags '-extldflags -static -s -w'`

<!--more-->

## 依赖库问题
　　也许这术编译的时候还是会依赖`libstdc++.so.6`, `libgcc_s.so.1`,`ld-musl-x86_64.so.1`. 那么就用下面的命令    
`go install -v -ldflags '-linkmode external -extldflags -static -s -w'`


```bash
---> Running in d9b0ea224dcc
/lib/ld-musl-x86_64.so.1 (0x7fae564f4000)
libstdc++.so.6 => /usr/lib/libstdc++.so.6 (0x7fae5639f000)
libgcc_s.so.1 => /usr/lib/libgcc_s.so.1 (0x7fae5638b000)
libc.musl-x86_64.so.1 => /lib/ld-musl-x86_64.so.1 (0x7fae564f4000)

```

## Docker 例子
1. 最小Go APP Docker镜像1.4MB
```bash
$ docker images | grep wxdlong/gocker
wxdlong/gocker  latest   f8d551e4c9af  8 days ago    1.41MB
```

2. Dockerfile ->  https://github.com/wxdlong/gocker
```Dockerfile
from golang:alpine as builder
Add . .
RUN go build -a -ldflags "-s -w -X 'main.goVersion=$(go version)' -X 'main.date=$(date)'" hello.go
from scratch
copy  --from=builder /go/hello /hello
CMD ["/hello"]
```

3. RUN
```bash
$ docker run --rm wxdlong/gocker
hello gocker!
Sat Jul 27 04:40:11 UTC 2019
go version go1.12.7 linux/amd64
```

 




