# **高级ACL**

## 简介

高级ACL能够依据源/目的IP地址、源/目的端口号、网络层及传输层协议以及IP流量分类和TCP标记值等各种参数（SYN|ACK|FIN等）进行报文过滤。

高级ACL控制的参数更多，所以对网络数据控制的更精细，应用的也更广泛。高级ACL的编号范围是3000-3999



## 命令

### 1、编写高级ACL [Huawei]acl 3000

[Huawei-acl-adv-3000]rule permit tcp source 192.168.1.0 0.0.0.255 destination 192.168.100.100 0 destination-port eq 22

[Huawei-acl-adv-3000]rule deny tcp source 192.168.1.0 0.0.0.255 destination 192.168.100.100 0 destination-port eq 23

### 2、应用ACL

[Huawei]interface GigabitEthernet 0/0/0

[Huawei-GigabitEthernet0/0/0]traffic-filter inbound acl 3000

 

# **课后练习题：**

1、有两台主机192.168.1.100/24和192.168.2.100/24,如果我希望192.168.1.100可以ping 通192.168.2.100,但是不允许192.168.2.100ping通192.168.1.100,那么我在网关路由器上如何写ACL来实现需求？

2、公司内部有一台服务器，地址是192.168.100.100/24，提供了网站服务和FTP服务，现在希望公司财务部门只能访问FTP服务，业务部门和技术部门只能访问网站服务，而其它部门都不能访问，应该在核心路由器上如何写ACL呢？

财务部门：192.168.10.0/24 业务部门：192.168.1.0/24 技术部门：192.168.2.0/24