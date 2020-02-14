# Eth-Trunk链路聚合--答案

# 基础练习

1. 交换机堆叠有什么好处？ 

   提高整网可靠性

2. Eth-Trunk链路捆绑有哪两种模式？都有什么特点？

   - **手工模式链路聚合**：手工模式下，Eth-Trunk的建立、成员接口的加入由手工配置,没有链路聚合控制协议LACP的参与。该模式下所有活动链路都参与数据的转发，平均分担流量。如果某条活动链路故障，链路聚合组自动在剩余的活动链路中平均分担流量。
   - **LACP模式链路聚合**：链路聚合控制协议LACP（Link Aggregation Control  Protocol），是基于IEEE802.3ad标准的一种实现链路动态聚合与解聚合的协议，以供设备  根据自身配置自动形成聚合链路并启动聚合链路收发数据，LACP模式就是采用LACP的一种链路聚合模式。聚合链路形成以后，LACP负责维护链路状态，在聚合条件发生变化时，自动调整链路聚合。LACP模式链路聚合由LACP确定聚合组中的活动和非活动链路，又称为M:N模式，即M条活动链路与N条备份链路的模式。这种模式提供了更高的链路可靠性，并且可以在M条链路中实现不同方式的负载均衡。

## 进阶练习

1. 实验：完成以下链路聚合配置 

   ![](./images/04.png)

   1）先采用手动模式进行链路聚合，成功后查看聚合端口情况，以及聚合端口带宽  

   2）采用LACP模式进行链路聚合，并配置最大活跃链路为3，成功后查看聚合端口情况以及 聚合端口带宽  

   3）将其中的某条链路断开，查看聚合链路情况

```
1）[LSW1]interface Eth-Trunk  
[LSW1-Eth-Trunk1]mode manual load-balance 
[LSW1-Eth-Trunk1]trunkport GigabitEthernet 0/0/1 to 0/0/3 
[LSW1-Eth-Trunk1]port link-type trunk 
[LSW1-Eth-Trunk1]port trunk allow-pass vlan 2 3 
LSW2同上
display eth-trunk 1 

2）[LSW1]interface Eth-Trunk 1   
[LSW1-Eth-Trunk1]mode lacp-static  
[LSW1-Eth-Trunk1]max active-linknumber 2 

[LSW1] interface gigabitethernet 0/0/1
[LSW1-GigabitEthernet0/0/1] eth-trunk 1
[LSW1-GigabitEthernet0/0/1] quit
[LSW1] interface gigabitethernet 0/0/2
[LSW1-GigabitEthernet0/0/2] eth-trunk 1
[LSW1-GigabitEthernet0/0/2] quit
[LSW1] interface gigabitethernet 0/0/3
[LSW1-GigabitEthernet0/0/3] eth-trunk 1
[LSW1-GigabitEthernet0/0/3] quit
[LSW1] interface gigabitethernet 0/0/4
[LSW1-GigabitEthernet0/0/4] eth-trunk 1
[LSW1-GigabitEthernet0/0/4] quit

LSW2 同上
display eth-trunk 1 

3）display eth-trunk 1 
```

