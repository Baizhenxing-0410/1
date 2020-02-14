# 课后练习题：

## 1、DHCP报文类型都有哪些？

DHCP Discover、DHCP Offer、DHCP Request、DHCP ACK 、DHCP NAK、DHCP Release、DHCP Decline、DHCP Inform

## 2、配置DHCP的时候，除了配置地址池外，一般还需要配置哪些常用选项？

确定ip地址所在的网络
	设置需要分配的ip地址范围
	设置分配ip地址的子网掩码
	设置网关ip地址
	设置dns地址
	设置租期时间(默认租期时间和最大租期时间)

## 3、将路由器配置成一个DHCP服务器，为局域网提供地址分配： 路由器接口g0/0/0的地址是：192.168.100.254/24分配给局域网主机的网关地址为192.168.100.254 分配的DNS服务器地址为：202.106.0.20地址租期为5天该如何配置呢？写出配置命令

```
[Huawei]dhcp enable 
Info: The operation may take a few seconds. Please wait for a moment.done.	
[Huawei]ip pool pool1
Info: It's successful to create an IP address pool.
[Huawei-ip-pool-pool1]network 192.168.100.254 mask 24
[Huawei-ip-pool-pool1]gateway-list 192.168.100.254
[Huawei-ip-pool-pool1]dns-list 202.106.0.20
[Huawei-ip-pool-pool1]lease day 5
[Huawei]interface g 0/0/0
[Huawei-GigabitEthernet0/0/0]ip address 192.168.100.254 24
[Huawei-GigabitEthernet0/0/0]dhcp select global 
```

