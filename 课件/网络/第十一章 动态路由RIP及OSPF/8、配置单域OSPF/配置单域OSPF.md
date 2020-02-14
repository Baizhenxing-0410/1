# 配置单域OSPF

在配置OSPF时， 需要首先使能OSPF进程。

命令ospf **[process id]**用来使能OSPF， 在该命令中可以配置进程ID。 如果没有配置进程 ID， 则使用1作为缺省进程ID。

命令**ospf [process id] [router-id <router-id>]**既可以使能OSPF进程， 还同时可以用于配置Router ID。 在该命令中， router-id代表路由器的ID。

命令**network**用于指定运行OSPF协议的接口，network 宣告的是<font color =red>ip地址</font>, network IP 后面跟的是<font color =red>通配符掩码</font>,

1、 基本配置：发布自己直连网段

```
[R1]ospf 10 router-id 1.1.1.1 ##配置路由器ID

[R1-ospf-10]area 0 ##选择区域

[R1-ospf-10-area-0.0.0.0]network 192.168.1.0 0.0.0.255 //通告直连网络

[R1-ospf-10-area-0.0.0.0]network 10.1.1.1 0.0.0.0	//通告直连网络
```

命令display ospf peer可以用于查看邻居相关的属性， 包括区域、 邻居的状态、 邻接协商的主从状态以及DR和BDR情况。

2、 查看OSPF邻居列表

```
[R1]display ospf peer

 

                   OSPF Process 10 with Router ID 1.1.1.1

                          Neighbors 

 

 Area 0.0.0.0 interface 192.168.1.1(GigabitEthernet0/0/0)'s neighbors

 Router ID: 2.2.2.2          Address: 192.168.1.2     

   State: Full  Mode:Nbr is  Master  Priority: 1

   DR: 192.168.1.1  BDR: 192.168.1.2  MTU: 0    

   Dead timer due in 38  sec 

   Retrans timer interval: 5 

   Neighbor is up for 00:01:03     

   Authentication Sequence: [ 0 ] 

 Router ID: 3.3.3.3          Address: 192.168.1.3     

   State: Full  Mode:Nbr is  Master  Priority: 1

   DR: 192.168.1.1  BDR: 192.168.1.2  MTU: 0    

   Dead timer due in 31  sec 

   Retrans timer interval: 5 

   Neighbor is up for 00:00:35     

   Authentication Sequence: [ 0 ] 
```

3、 查看OSPF链路状态数据库

```
[R1]display ospf lsdb 

 

                   OSPF Process 10 with Router ID 1.1.1.1

                          Link State Database 

 

                               Area: 0.0.0.0

 Type      LinkState ID    AdvRouter          Age  Len   Sequence   Metric

 Router    2.2.2.2         2.2.2.2            134  48    80000006       1

 Router    1.1.1.1         1.1.1.1            127  48    80000008       1

 Router    3.3.3.3         3.3.3.3            123  48    80000006       1

 Network   192.168.1.1     1.1.1.1            127  36    80000004       0
```

4、 设置接口OSPF优先级

```
[R1]interface GigabitEthernet 0/0/0

[R1-GigabitEthernet0/0/0]ospf dr-priority 100 //设置优先级
```

5、 设置参照带宽，影响cost计算方式

```
[R1]ospf 10 //创建opsf进程

[R1-ospf-10]bandwidth-reference 10000  //使用参考带宽分配OSPF
```


查看接口cost值：

```
[R1]display ospf interface GigabitEthernet 0/0/0
```

6、 修改hello包发送间隔

```
[R1]interface GigabitEthernet 0/0/0

[R1-GigabitEthernet0/0/0]ospf timer hello 5
```

命令debugging ospf packet用来指定调试OSPF报文，以便解决OSPF问题

7、 测试debug消息

```
<R1>terminal debugging 

Info: Current terminal debugging is on.
<R1>debugging ospf packet
```



