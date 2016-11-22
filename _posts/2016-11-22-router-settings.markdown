---
layout: post
slug: router-setting
title: quidway路由器和交换机的配置命令
categories:
- 代码之术
tags:
- 计算机网络
- 路由器配置
- 交换机
---
## VLAN   

**VLAN端口配置**  

```
[S1]vlan 2
[S1-VLAN2]port e 0/1 to e0/5  /设置VLAN端口
[S1-VLAN2]quit
```

**VLAN端口类型配置**  

配置端口vlan属性为trunk
<!--more-->
```
[S1] inter e0/13
[S1-Ethernet0/13]port link-type trunk
[S1-Ethernet0/13]port trunk permit VLAN 2 3
```

配置端口vlan属性为hybrid

```
[S1-Ethernet0/1]int e0/1
[S1-Ethernet0/1]port link-type hybrid
[S1-Ethernet0/1]port hybrid pvid vlan 10 /缺省标签为vlan10
[S1-Ethernet0/1]port hybrid vlan 10 20 untagged /vlan10 和 20 untagged
```

查看vlan配置

```
[S1]display VLAN 2
```

## PPP

配置ppp

```
[R1]interface Serial 0/0
[R1-Serial0/0]link-protocol ppp
```

关闭和开启端口使ppp生效

```
[R1-Serial0/0]shutdown
[R1-Serial0/0]undo shutdown
```

显示debug

```
<R1>debugging ppp all /打开pppdebug开关
<R1>terminal debugging  /显示debug信息
```

### PAP方式身份验证

验证方

```
[R1]local-user RTB				/配置用户列表
[R1-user-RTB]service-type ppp    /配置服务类型
[R1-user-RTB]password simple aaa /配置用户对应密码
[R1]interface Serial0/0 		/进入路由器接口视图
[R1-Serial0/0]ppp authentication pap   /授权pap验证
```

被验证方

```
[R2]interface serial 0/0
[R2-Serial0/0]ppp pap local-user RTB password simple aaa
```

### CHAP方式身份验证

验证方

```
[R1]local-user RTB				/配置用户列表
[R1-user-RTB]service-type ppp    /配置服务类型
[R1-user-RTB]password simple aaa /配置用户对应密码
[R1]interface Serial0/0 		/进入路由器接口视图
[R1-Serial0/0]ppp authentication chap   /授权chap验证
[R1-Serial0/0]chap user RTA		/配置本地名称
```

被验证方

```
[R2]local-user RTA
[R2]local-user RTA service-type ppp
[R2]local-user RTA password simple aaa
[R2]interface serial 0/0
[R2-Serial0/0]ppp chap user RTB
```

## 静态路由

配置静态路由

```
[R1]ip route-static 192.168.1.0 0.0.0.255 10.2.3.2
[R1]ip route-static 192.168.1.0 0.0.0.255 10.2.3.2 preference 80  /设置优先级
```

配置缺省路由

```
[R1]ip route-static 0.0.0.0 0.0.0.0 192.168.2.1
```

## NAT

```
#设置访问控制列表，允许192.168.2.0/24的外出包
[R1]acl number 2001
[R1-acl-2001]rule permit souce 192.168.2.0 0.0.0.255
[R1-acl-2001]rule deny source any
#定义地址池
[R1]nat address-group 1 192.168.5.5 192.168.5.9
#在0/1端口上应用规则，并添加缺省路由
[R1]inter e0/1
[R1-Ethernet0/1]nat outbound 2001 address-group 1
[R1]ip route-static 0.0.0.0 0.0.0.0 192.168.5.1
```

## OSPF

开启OSPF

```
#配置路由器id
[R1]router id 1.1.1.1
#开启ospf
[R1]ospf
[R1-ospf-1]area 0
#设置在路由器上开启ospf的网段
[R1-ospf-1-area-0.0.0.0]network 30.1.1.0 0.0.0.255
```

配置cost

```
/路由器上
[R1]ospf
[R1-ospf-1]inter e0/0
[R1-ospf-1-Ethernet0/0]ospf cost 100
/三层交换机上
[S1]ospf
[S1-ospf-1]inter vlan3
[S1-ospf-1-Ethernet0/0]ospf cost 200
```

引入静态路由到ospf

```
[R1-ospf]import-route static
```

引入直连路由到ospf

```
[R1-ospf]import-route static
```

重启ospf

```
[R1]reset ospf all
```

显示ospf调试信息

```
debugging ospf
```



## IPV6

删除主机IPv6地址和网关

```
ipv6 adu ifindex/ipv6 address life 0
ipv6 rtu prefix ifindex/gateway life 0
```

路由器配置接口ipv6地址

```
ipv6 /全局使能IPv6
interface GigabitEthernet 0/0
ipv6 address 2003::2/64
```

配置静态路由

```
ipv6 route-static 2001::64 2007::1
```
