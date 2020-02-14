# IPSec VPN配置

## 需要考虑的问题

### 管理连接建立要考虑的问题：

 

- 设备验证是如何执行的


- 使用哪种加密算法


-  使用哪种散列算法


- 
  使用哪种DH密钥组


- 
  连接的生存周期




### 数据连接要考虑的问题：

- 使用哪种安全协议

- 对于ESP，使用哪种加密算法和/或散列算法

- 对于AH，使用哪种散列算法


- 隧道模式还是传输模式


- 数据连接的生存周期

## 配置IPSec VPN

1. 配置网络可达。首先需要检查报文发送方和接收方之间的网络层可达性，确保双方只有建立IPSec VPN隧道才能进行IPSec通信。

2. 配置ACL识别兴趣流。第二步是定义数据流。因为部分流量无需满足完整性和机密性要求，所以需要对流量进行过滤，选择出需要进行IPSec处理的兴趣流。可以通过配置ACL来定义和区分不同的数据流。

3. 创建IPSec安全提议。 IPSec提议定义了保护数据流所用的安全协议、认证算法、加密算法和封装模式。安全协议包括AH和ESP，两者可以单独使用或一起使用。 AH支持MD5和SHA-1认证算法； ESP支持两种认证算法（ MD5和SHA-1）和三种加密算法（DES、 3DES和AES）。为了能够正常传输数据流，安全隧道两端的对等体必须使用相同的安全协议、认证算法、加密算法和封装模式。如果要在两个安全网关之间建立IPSec隧道，建议将IPSec封装模式设置为隧道模式，以便隐藏通信使用的实际源IP地址和目的IP地址。

4. 创建IPSec安全策略。 IPSec策略中会应用IPSec提议中定义的安全协议、认证算法、加密算法和封装模式。每一个IPSec安全策略都使用唯一的名称和序号来标识。 IPSec策略可分成两类：手工建立SA的策略和IKE协商建立SA的策略

5. 在一个接口上应用IPSec安全策略。

## **具体配置样例**

定义感兴趣的数据流 

```
acl number 3000
	rule 5 permit ip source 192.168.1.10 0 destination 192.168.2.10 0
```

定义第一阶段安全提议

```
ike proposal 2 

	encryption-algorithm 3des-cbc

	dh group5

	sa duration 10000

```

定义对等体信息

```
ike peer shanghai v1 

	pre-shared-key simple huawei123

	remote-address 200.1.1.1

```

定义第二阶段安全提议

```
ipsec proposal 10 

	esp encryption-algorithm aes-128
```

定义安全策略

```
ipsec policy my-policy 20 isakmp 
调用ACL
	security acl 3000 
调用对等体	
	ike-peer shanghai 
调用第二阶段策略
	proposal 10	
```

 在外网接口调用ipsec策略

```
interface GigabitEthernet0/0/1

ip address 100.1.1.1 255.255.255.0

ipsec policy my-policy
```

