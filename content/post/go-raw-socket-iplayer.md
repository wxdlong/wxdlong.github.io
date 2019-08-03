---
title: "Go Raw Socket Iplayer"
date: 2017-09-23T10:31:14+08:00
draft: false
---

## 起因 ##
   最近有个项目,需要转发UDP数据包,并且增加额外的数据包头, 于是在同事的指导下,对网络协议这个抽象的概念有了全新的认识.结合Wireshark这个神器,可以清晰的看到数据包格式长什么样.然后你会发现数据包也不是什么神秘的事情.

## Socket Raw 和TCP Socket ##

    Socket 分为两种类型,普通socket只能构赵TCP层之上的数据包
    Raw socket 可以构赵Ethernet层的数据包,也就是说可以随意更改SrcMac,DestMac....等, 注意需要Root权限.

## Receive ICMP packet on localhost

Let’s read the first ICMP packet on localhost:

``` go
package main

import (
	"net"
	"fmt"
	"github.com/google/gopacket/layers"
	"github.com/google/gopacket"
)

func main() {
	protocol := "tcp"
	netaddr, _ := net.ResolveIPAddr("ip4", "127.0.0.1")
	conn, _ := net.ListenIP("ip4:"+protocol, netaddr)

	buf := make([]byte, 1024)
	numRead, _, _ := conn.ReadFrom(buf)
	//open terminal, send cmd: ping 127.0.0.1
	fmt.Printf("% X\n", buf[:numRead])

	/*我是分割线, gopacket 解析ICMP packet*/
	var icmpv4 layers.ICMPv4
	var tcpv4 layers.IPv4
	decoded := []gopacket.LayerType{}
	parser := gopacket.NewDecodingLayerParser(layers.LayerTypeICMPv4, &icmpv4,&tcpv4)
	parser.DecodeLayers(buf[:numRead], &decoded)
	for _, layerType := range decoded {
		switch layerType {
		case layers.LayerTypeICMPv4:
			fmt.Printf("    icmpv4: typecode->%s,Seq->%x,Checksum->%d,id->%d,Contents:%b", icmpv4.TypeCode.String(), icmpv4.Seq, icmpv4.Checksum, icmpv4.Id, icmpv4.Contents)
		case layers.LayerTypeIPv4:
			fmt.Printf("tcp layer protocol->%s, content->%s,ttl->%d,length->%d",tcpv4.Protocol.String(),tcpv4.Contents,tcpv4.TTL,tcpv4.Length)
		}
	}

	/*08 00 61 68 5B 9B 00 01 7D D0 C5 59 00 00 00 00 38 FE 00 00 00 00 00 00 10 11 12 13 14 15 16 17 18 19 1A 1B 1C 1D 1E 1F 20 21 22 23 24 25 26 27 28 29 2A 2B 2C 2D 2E 2F 30 31 32 33 34 35 36 37
icmpv4: typecode->EchoRequest,Seq->1,Checksum->24936,id->23451,Contents:[1000 0 1100001 1101000 1011011 10011011 0 1]*/

}

```

1. 启动程序后, 打开控制台: 发送Ping命令后,GOlang可以接收到消息,PING Msg第一个byte就8,代表Echo Request

```shell
wxdl@ong:~$ ping 127.0.0.1 -c 1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.115 ms

--- 127.0.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.115/0.115/0.115/0.000 ms

```

2. 用Wireshark 查看刚刚发送的数据包,对比程序截取的数据.
![icmp](http://owpu4mhp9.bkt.clouddn.com/image/jpg/icmpData.png)

3. 修改程序的proctol 为tcp.抓取tcp层数据, 然后打开terminal: 发送wget 命令

```go
package main

import (
	"net"
	"fmt"
	"github.com/google/gopacket/layers"
	"github.com/google/gopacket"
)

func main() {
	protocol := "tcp"
	netaddr, _ := net.ResolveIPAddr("ip4", "127.0.0.1")
	conn, _ := net.ListenIP("ip4:"+protocol, netaddr)

	buf := make([]byte, 1024)
	numRead, _, _ := conn.ReadFrom(buf)
	//open terminal, send cmd: ping 127.0.0.1
	fmt.Printf("% X\n", buf[:numRead])

	/*我是分割线, gopacket 解析ICMP packet*/
	var icmpv4 layers.ICMPv4
	var iPv4 layers.IPv4
	var tcp layers.TCP
	decoded := []gopacket.LayerType{}
	parser := gopacket.NewDecodingLayerParser(layers.LayerTypeTCP,&tcp)
	parser.DecodeLayers(buf[:numRead], &decoded)
	for _, layerType := range decoded {
		switch layerType {
		case layers.LayerTypeICMPv4:
			fmt.Printf("    icmpv4: typecode->%s,Seq->%x,Checksum->%d,id->%d,Contents:%b", icmpv4.TypeCode.String(), icmpv4.Seq, icmpv4.Checksum, icmpv4.Id, icmpv4.Contents)
		case layers.LayerTypeIPv4:
			fmt.Printf("ipv4 layer protocol->%s, content->%s,ttl->%d,length->%d", iPv4.Protocol.String(), iPv4.Contents, iPv4.TTL, iPv4.Length)
		case layers.LayerTypeTCP:
			fmt.Printf("tcp layer: srcPort->%d,dstPort->%d,Seq->%d,SYN->%d,checksum->%d,contents:%s",tcp.SrcPort,tcp.DstPort,tcp.Seq,tcp.SYN,tcp.Checksum,tcp.Contents)
		}
	}
	/*E3 06 00 50 3B AE 3F 38 00 00 00 00 A0 02 AA AA FE 30 00 00 02 04 FF D7 04 02 08 0A 2C A0 52 5B 00 00 00 00 01 03 03 07
	tcp layer: srcPort->58118,dstPort->80,Seq->1001275192,SYN->%!d(bool=true),checksum->65072,contents:� P;�?8    ����0  ��
	,�R[    
	*/
	
}
```

4. Terminal, 因为没有后台服务,wget当然拒绝链接,不过数据包已经发出去了.

```shell
wxdl@ong:~$ wget 127.0.0.1
--2017-09-23 13:28:58--  http://127.0.0.1/
Connecting to 127.0.0.1:80... failed: Connection refused.

```

5. Wireshark 查看数据包,可以看到SrcPort:58118,DestPort:80 和上面程序输出一致!
![tcppacket](http://owpu4mhp9.bkt.clouddn.com/image/jpg/tcppacket.png)

### 发送数据包 ### 
代码请转:https://gitee.com/bb/go/blob/master/src/com/packet/goPacket.go

```go
//打开RAW socket.
	if sockfd, err = syscall.Socket(syscall.AF_PACKET, syscall.SOCK_RAW, syscall.ETH_P_IP); err != nil {
		fmt.Print("Socket() error:", err.Error())
		return
	}

	defer syscall.Shutdown(sockfd, syscall.SHUT_RDWR)
	//获取本机回路网络信息
	if_info, err := net.InterfaceByName("lo")
	if err != nil {
		fmt.Println("=============================================================================")
		fmt.Println("= Error 2                                                                   =")
		fmt.Println("=============================================================================")
		fmt.Println(err)
	}

	addr := syscall.SockaddrLinklayer{
		Protocol: syscall.ETH_P_IP,
		Ifindex:  if_info.Index,
	}
	syscall.Bind(sockfd,&addr)
	if n,err = syscall.Write(sockfd, buffer.Bytes()); err != nil {
		fmt.Print("Sendto() error: ", err.Error())
		return
	}
```

>[ICMP报文格式和种类](http://sdbaby.blog.51cto.com/149645/318380)

>[GO packet lit](https://godoc.org/github.com/google/gopacket)