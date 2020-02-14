# IPSec VPN配置--答案

![](./images/01.png)

1、通过配置IPSec VPN隧道，实现北京总部私网地址和上海分支私网地址互通。

 

2、提交相关配置文档



```
// 创建访问控制规则ACL
acl number 3000
// 设置规则
rule 1 permit ip source 192.168.1.2 0 destination 172.16.1.2 0
(
rule 1 permit ip source 172.16.1.2 0 destination 192.168.1.2 0
)
// 退出
q

// 创建第一阶段ike安全提议
ike proposal 1
// 指定加密算法
encryption-algorithm 3des-cbc
// 指定dh组
dh group5
// 配置哈希算法
authentication-algorithm md5
// 指定isakmp sa的时间限制（60-604800，秒，默认为86400）
sa duration 10000
// 退出
q

// 创建对等体信息(对方信息)(思科的IKEv2具备更好的兼容性（跨平台、跨厂商），更多的新特性（如内建NAT、反DOS攻击和隧道存活状态检测等）)
ike peer huawei2 v2
(ike peer huawei3 v2)
// 指定预共享密钥
pre-shared-key simple huawei123
// 指定本地对等体的IP地址
local-address 100.1.1.2
(local-address 200.1.1.2)
// 指定远程对等体的IP地址
remote-address 200.1.1.2
(remote-address 100.1.1.2)
// 指定第一阶段安全提议 (指定IKE对等方的IKE建议)
ike-proposal 1
// 退出
q

// 创建第二阶段IPSec安全提议
ipsec proposal 2
// 指定esp加密算法
esp encryption-algorithm aes-128
// 退出
q

// 创建IPSec安全策略（名称/id/使用IKE建立IPSec SA）
ipsec policy my-policy 1 isakmp
// 调用ACL 指定策略应用的数据包
security acl 3000
// 指定IKE对等体
ike-peer huawei2
// 指定第二阶段安全提议
proposal 2
// 退出
q

// 在外网接口调用IPSec安全策略
int g 0/0/0
ipsec policy my-policy 
// 退出
q

// 增加静态路由
ip route-static 172.16.1.2 32 200.1.1.2
(ip route-static 192.168.1.2 32 100.1.1.2)
```

