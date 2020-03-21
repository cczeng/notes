# Nmap 

> Nmap是一款开源免费的网络发现（Network Discovery）和安全审计（Security Auditing）工具。软件名字Nmap是Network Mapper的简称。  



## 一、Nmap 基本扫描方法  

>  Nmap主要包括四个方面的扫描功能，主机发现、端口扫描、应用与版本侦测、操作系统侦测。  



### 简单扫描特定主机端口开放  

```bash
nmap 192.168.1.103(目标端口)
```



### 完整全面的扫描

```bash
nmap -T4 -A -v 192.168.1.103
```

- -A 选项用于使用进攻性方式扫描；
- -T4 指定扫描过程使用的时序(一共6个级别 0-5)，级别越高，扫描速度越快，但也容易被防火墙或IDS检测并屏蔽掉
- -v 表示显示冗余信息，显示扫描过程中的细节



## 二、主机发现

> 主机发现，既用于发现目标主机是否在线(Alive, 处于开启状态)   

### 原理

>主机发现发现的原理与Ping命令类似，发送探测包到目标主机，如果收到回复，那么说明目标主机是开启的。Nmap支持十多种不同的主机探测方式，比如发送ICMP ECHO/TIMESTAMP/NETMASK报文、发送TCPSYN/ACK包、发送SCTP INIT/COOKIE-ECHO包，用户可以在不同的条件下灵活选用不同的方式来探测目标机。  

默认情况下，nmap会发送四种不同类型的数据包来探测目标主机是否在线。  

- ICMP echo request
- a TCP SYN packet to port 443
- a TCP ACK packet to port 80
- an ICMP timestamp request

依次发送四个报文探测目标主机是否开启。只要收到其中一个包的回复，那就证明目标机开启  

### 主机发现用法

> 通常主机发现不单独使用，而只是作为端口扫描、版本检测、OS侦测先行步骤

参数： 

- `-sL`: List Scan 列表扫描，仅将制定的目标的IP例举出来，不进行主机发现。
- `-sn`: Ping Scan 只进行主机发现，不进行端口扫描
- `-Pn`: 将所有指定的主机视作开启的，跳过主机发现的过程
- `-PS/PA/PU/PY[portlist]`： 使用TCPSYN/ACK或SCTP INIT/ECHO 方式进行发现
- `-PE/PP/PM`: 使用 ICMP echo，timestamp，and netmask 请求包发现主机。
- `-PO`：使用IP协议包探测对方主机是否开启
- `-n/-R`: -n 表示不进行DNS解析；-R 表示总是进行dns解析
- `--dns-servers <serv1[,serv2]>`: 指定DNS服务器
- `--system-dns`: 指定使用系统的DNS服务器
- `--traceroute`: 追踪每个路由节点

其中，比较常用的使用的是 `-sn`，表示只单独进行主机发现过程；



### 探测局域网内活动主机

扫描局域网 `192.168.1.100-192.168.1.120`范围内哪些ip的主机是活动的。 

命令如下：  

```bash
nmap -sn 192.168.1.100-120
```



## 三、端口扫描

- `open`: 端口是开放的
- `closed`: 端口是关闭的
- `filtered`: 端口被防火墙IDS/IPS屏蔽，无法确定其状态
- `unfiltered`: 端口没有被屏蔽，但是否开放需要进一步确定
- `open|filtered`: 端口是开放的或被屏蔽
- `closed|filtered`: 端口是关闭的或被屏蔽

### 端口扫描方法

nmap提供丰富的命令行参数来指定扫描方式和扫描端口  



#### 扫描方式选项

```bash
-sS/sT/sA/sW/sM:指定使用 TCP SYN/Connect()/ACK/Window/Maimon scans的方式来对目标主机进行扫描。

-sU: 指定使用UDP扫描方式确定目标主机的UDP端口状况。

-sN/sF/sX: 指定使用TCP Null, FIN, and Xmas scans秘密扫描方式来协助探测对方的TCP端口状态。

--scanflags <flags>: 定制TCP包的flags。

-sI <zombiehost[:probeport]>: 指定使用idle scan方式来扫描目标主机（前提需要找到合适的zombie host）

-sY/sZ: 使用SCTP INIT/COOKIE-ECHO来扫描SCTP协议端口的开放的情况。

-sO: 使用IP protocol 扫描确定目标机支持的协议类型。

-b <FTP relay host>: 使用FTP bounce scan扫描方式
```



#### 端口参数与扫描顺序  

```bash
-p <port ranges>:扫描指定的端口
# -p22;-p-65535;-p U:53,111,137,T: 21-25,80,139,8080,S:9（其中T代表TCP协议、U代表UDP协议、S代表SCTP协议）

-F: Fast mode - 快速模式，仅扫描TOP 100的端口

-r: 不进行端口随机打乱的操作（如无该参数，nmap会将要扫描的端口以随机顺序方式扫描，以让nmap的扫描不易被对方防火墙检测到）

--top-ports<number>: 扫描开放概率最高的number个端口

--port-ratio<ratio>: 扫描指定频率以上的端口。
```



#### 端口扫描演示

```bash
nmap -sS -sU -T4 --top-ports 300 192.168.1.100
```

- -sS 表示使用 TCP SYN 方式扫描
- -sU 表示扫描UDP端口
- -T4 表示时间级别配置4级
- --top-ports 300 表示扫描最有可能开放的300个端口



## 四、版本侦测

> 版本侦测，用于确定目标主机开放端口上运行的具体的应用程序及版本信息。

- 高速。并行地进行套接字操作，实现一组高效的探测匹配定义语法。
  尽可能地确定应用名字与版本名字。
- 支持TCP/UDP协议，支持文本格式与二进制格式。
  支持多种平台服务的侦测，包括Linux/Windows/Mac OS/FreeBSD等系统。
- 如果检测到SSL，会调用openSSL继续侦测运行在SSL上的具体协议（如HTTPS/POP3S/IMAPS）。
- 如果检测到SunRPC服务，那么会调用brute-force RPC grinder进一步确定RPC程序编号、名字、版本号。
- 支持完整的IPv6功能，包括TCP/UDP，基于TCP的SSL。
- 通用平台枚举功能（CPE）
- 广泛的应用程序数据库（nmap-services-probes）。目前Nmap可以识别几千种服务的签名，包含了180多种不同的协议。
  

### 版本侦测的用法

```bash
-sV: 指定让nmap进行版本侦测

--version-intensity<level>: 指定版本侦测强度（0-9），默认为7。数值越高，探测出的服务越准确，但是运行时间会比较长

--version-light: 指定使用轻量侦测方式

--version-all: 尝试使用所有的probes进行侦测

--version-trace: 显示出详细的版本侦测过程信息
```



### 版本侦测演示

```bash
nmap -sV 192.168.1.103
```



## OS侦测

> 操作系统侦测用于检测目标主机运行的操作系统类型及设备类型等信息



#### OS侦测用法

```bash
-O： 指定Nmap进行OS侦测

--osscan-limit: 限制Nmap只对确定的主机进行OS侦测(至少需确定该主机分别有一个open和closed的端口)

--osscan-guess: 大胆猜测对方的主机的系统类型
```



#### 演示

```bash
nmap -O 192.168.1.103
```





## Nmap 高级用法

只摘选部分   



## 规避用法

```bash
-f;--mtu<val>: 指定使用分片、指定数据表的MTU

-D<decoy1,decoy2[,ME],...>: 用一组IP地址掩盖真实地址，启用ME填入自己的IP地址

-S<IP_Address>: 伪装成其他IP地址

-e<iface>: 使用特定的网络接口

-g/--source-port<portnum>: 使用指定源接口

--data-length<num>: 填充随机数让数据包长度达到Num

--ip-options<options>: 使用指定的IP选项来发送数据包

--ttl<val>: 设置time-to-live时间

--spoof-mac <mac address/prefix/vendor name>: 伪装Mac地址

--badsum: 使用错误的checksum来发送数据包
```



### 规避演示

```bash
nmap -v -F -Pn -D 192.168.1.103,192.168.1.110,ME -e eth0 -g 3355 192.168.1.1
```

- -F 表示快速扫描100个端口
- -Pn 表示不进行Ping扫描
- -D 表示使用IP诱骗方式掩盖自己真实IP(其中ME表示自己的IP)
- -e eth0 表示使用eth0网卡发送数据包
- -g 3355 表示自己的源端口使用3355
- 192.168.1.1 是被扫描的目标地址

