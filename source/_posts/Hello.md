---
author: XiaoBei
categories:
- - Liunx
date: XiaoBei
excerpt: '基本工具   fping   可以ping多个地址  ```   Usage: fping [options] [targets...] Probing
  options:      -4, --ipv4         only ping IPv4 addresses      -6, --ipv6         only
  ping IPv6 addresses      -b, --size=...'
layout: post
tags:
- Liunx
- Kali
title: Kali Liunx 扫描工具
type:  type3
updated: '2022-12-07 17:59:15'
---
## 基本工具

- #### fping

  - 可以ping多个地址

  ```
  Usage: fping [options] [targets...]

  Probing options:
     -4, --ipv4         only ping IPv4 addresses
     -6, --ipv6         only ping IPv6 addresses
     -b, --size=BYTES   amount of ping data to send, in bytes (default: 56)
     -B, --backoff=N    set exponential backoff factor to N (default: 1.5)
     -c, --count=N      count mode: send N pings to each target
     -f, --file=FILE    读取ip列表文件
     -g, --generate     指定ip范围,不能和-f同时使用 (fping -g 192.168.1.0 192.168.1.255 或者 fping -g 192.168.1.0/24)
     -H, --ttl=N        set the IP TTL value (Time To Live hops)
     -I, --iface=IFACE  bind to a particular interface
     -l, --loop         loop mode: send pings forever
     -m, --all          use all IPs of provided hostnames (e.g. IPv4 and IPv6), use with -A
     -M, --dontfrag     set the Don't Fragment flag
     -O, --tos=N        set the type of service (tos) flag on the ICMP packets
     -p, --period=MSEC  interval between ping packets to one target (in ms)
  					  (in loop and count modes, default: 1000 ms)
     -r, --retry=N      number of retries (default: 3)
     -R, --random       random packet data (to foil link data compression)
     -S, --src=IP       set source address
     -t, --timeout=MSEC individual target initial timeout (default: 500 ms,
  					  except with -l/-c/-C, where it's the -p period up to 2000 ms)

  Output options:
     -a, --alive        仅仅显示活跃的主机
     -A, --addr         show targets by address
     -C, --vcount=N     same as -c, report results in verbose format
     -d, --rdns         show targets by name (force reverse-DNS lookup)
     -D, --timestamp    print timestamp before each output line
     -e, --elapsed      show elapsed time on return packets
     -i, --interval=MSEC  interval between sending ping packets (default: 10 ms)
     -n, --name         show targets by name (reverse-DNS lookup for target IPs)
     -N, --netdata      output compatible for netdata (-l -Q are required)
     -o, --outage       show the accumulated outage time (lost packets * packet interval)
     -q, --quiet        不显示无法链接的报错内容
     -Q, --squiet=SECS  same as -q, but add interval summary every SECS seconds
     -s, --stats        print final stats
     -u, --unreach      show targets that are unreachable
     -v, --version      show version
     -x, --reachable=N  shows if >=N hosts are reachable or not

  ```
- #### nping

  - 允许发送多种协议（TPC、UDP、ICMP和ARP）的数据包。可以调整协议头的字段，例如可以设置TCP和UDP的源端口和目的端口
  - 主要功能：
    - 发送ICMP echo请求
    - 对网络进行压力测试
    - ARP毒化攻击
    - DOS攻击
    - 支持多种探测模式
    - 可以探测多个主机的多个端口

  ```
  Usage: nping [Probe mode] [Options] {target specification}

  目标说明:
    目标可以指定为主机名、IP地址、网络等。
    例如: scanme.nmap.org, microsoft.com/24, 192.168.0.1; 10.0.*.1-24
  PROBE MODES:
    --tcp-connect                    : 非特权TCP连接探测模式。
    --tcp                            : TCP探测模式.
    --udp                            : UDP探测模式.
    --icmp                           : ICMP探测模式.
    --arp                            : ARP/RARP探测模式.
    --tr, --traceroute               : 路由跟踪模式（只能与 TCP/UDP/ICMP 模式）
  TCP CONNECT MODE:
     -p, --dest-port <port spec>     : 设置目标端口
     -g, --source-port <portnumber>  : 尝试使用自定义源端口。
  TCP PROBE MODE:
     -g, --source-port <portnumber>  : 设置源端口
     -p, --dest-port <port spec>     : 设置目标端口
     --seq <seqnumber>               : Set sequence number.
     --flags <flag list>             : Set TCP flags (ACK,PSH,RST,SYN,FIN...)
     --ack <acknumber>               : Set ACK number.
     --win <size>                    : Set window size.
     --badsum                        : Use a random invalid checksum. 
  UDP PROBE MODE:
     -g, --source-port <portnumber>  : Set source port.
     -p, --dest-port <port spec>     : Set destination port(s).
     --badsum                        : Use a random invalid checksum. 
  ICMP PROBE MODE:
    --icmp-type <type>               : ICMP type.
    --icmp-code <code>               : ICMP code.
    --icmp-id <id>                   : Set identifier.
    --icmp-seq <n>                   : Set sequence number.
    --icmp-redirect-addr <addr>      : Set redirect address.
    --icmp-param-pointer <pnt>       : Set parameter problem pointer.
    --icmp-advert-lifetime <time>    : Set router advertisement lifetime.
    --icmp-advert-entry <IP,pref>    : Add router advertisement entry.
    --icmp-orig-time  <timestamp>    : Set originate timestamp.
    --icmp-recv-time  <timestamp>    : Set receive timestamp.
    --icmp-trans-time <timestamp>    : Set transmit timestamp.
  ARP/RARP PROBE MODE:
    --arp-type <type>                : Type: ARP, ARP-reply, RARP, RARP-reply.
    --arp-sender-mac <mac>           : Set sender MAC address.
    --arp-sender-ip  <addr>          : Set sender IP address.
    --arp-target-mac <mac>           : Set target MAC address.
    --arp-target-ip  <addr>          : Set target IP address.
  IPv4 OPTIONS:
    -S, --source-ip                  : Set source IP address.
    --dest-ip <addr>                 : Set destination IP address (used as an 
  									 alternative to {target specification} ). 
    --tos <tos>                      : Set type of service field (8bits).
    --id  <id>                       : Set identification field (16 bits).
    --df                             : Set Don't Fragment flag.
    --mf                             : Set More Fragments flag.
    --evil                           : Set Reserved / Evil flag.
    --ttl <hops>                     : Set time to live [0-255].
    --badsum-ip                      : Use a random invalid checksum. 
    --ip-options <S|R [route]|L [route]|T|U ...> : Set IP options
    --ip-options <hex string>                    : Set IP options
    --mtu <size>                     : Set MTU. Packets get fragmented if MTU is
  									 small enough.
  IPv6 OPTIONS:
    -6, --IPv6                       : Use IP version 6.
    --dest-ip                        : Set destination IP address (used as an
  									 alternative to {target specification}).
    --hop-limit                      : Set hop limit (same as IPv4 TTL).
    --traffic-class <class> :        : Set traffic class.
    --flow <label>                   : Set flow label.
  ETHERNET OPTIONS:
    --dest-mac <mac>                 : Set destination mac address. (Disables
  									 ARP resolution)
    --source-mac <mac>               : Set source MAC address.
    --ether-type <type>              : Set EtherType value.
  PAYLOAD OPTIONS:
    --data <hex string>              : Include a custom payload.
    --data-string <text>             : Include a custom ASCII text.
    --data-length <len>              : Include len random bytes as payload.
  ECHO CLIENT/SERVER:
    --echo-client <passphrase>       : Run Nping in client mode.
    --echo-server <passphrase>       : Run Nping in server mode.
    --echo-port <port>               : Use custom <port> to listen or connect.
    --no-crypto                      : Disable encryption and authentication.
    --once                           : Stop the server after one connection.
    --safe-payloads                  : Erase application data in echoed packets.
  TIMING AND PERFORMANCE:
    Options which take <time> are in seconds, or append 'ms' (milliseconds),
    's' (seconds), 'm' (minutes), or 'h' (hours) to the value (e.g. 30m, 0.25h).
    --delay <time>                   : Adjust delay between probes.
    --rate  <rate>                   : Send num packets per second.
  MISC:
    -h, --help                       : Display help information.
    -V, --version                    : Display current version number. 
    -c, --count <n>                  : <n>轮后停止
    -e, --interface <name>           : 使用提供的网络接口.
    -H, --hide-sent                  : Do not display sent packets.
    -N, --no-capture                 : Do not try to capture replies.
    --privileged                     : Assume user is fully privileged.
    --unprivileged                   : Assume user lacks raw socket privileges.
    --send-eth                       : Send packets at the raw Ethernet layer.
    --send-ip                        : Send packets using raw IP sockets.
    --bpf-filter <filter spec>       : Specify custom BPF filter.
  OUTPUT:
    -v                               : Increment verbosity level by one.
    -v[level]                        : Set verbosity level. E.g: -v4
    -d                               : Increment debugging level by one.
    -d[level]                        : Set debugging level. E.g: -d3
    -q                               : Decrease verbosity level by one.
    -q[N]                            : Decrease verbosity level N times
    --quiet                          : Set verbosity and debug level to minimum.
    --debug                          : Set verbosity and debug to the max level.
  EXAMPLES:
    nping scanme.nmap.org
    nping --tcp -p 80 --flags rst --ttl 2 192.168.1.1
    nping --icmp --icmp-type time --delay 500ms 192.168.254.254
    nping --echo-server "public" -e wlan0 -vvv 
    nping --echo-client "public" echo.nmap.org --tcp -p1-1024 --flags ack

  SEE THE MAN PAGE FOR MANY MORE OPTIONS, DESCRIPTIONS, AND EXAMPLES

  ```
