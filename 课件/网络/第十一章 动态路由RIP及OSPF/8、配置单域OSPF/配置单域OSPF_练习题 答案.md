# 课后练习题：

1、 在广播网络中，DR和BDR用来接收链路状态更新报文的目的地址是哪个？用来发送链路状态更新报文的目的地址是哪个？

2、 实验拓扑：

![clip_image002](C:/Users/李/Desktop/网络课件/李子恒_第十一章 动态路由RIP及OSPF 05天/8、配置单域OSPF/image/clip_image002.jpg)

 要求如下:

1） 使用OSPF路由协议实现全网路由互通

2） 配置使用10000M做为OSPF cost计算的参照带宽

3） 配置AR1连接AR2的接口上hello发送间隔为5秒，然后查看AR1的邻居列表；把AR1接口Hello报文间隔改回默认10秒，再查看其邻居列表

4） 查看3台路由器的OSPF链路状态数据库是否一致？

5） 提交实验中的关键配置

含有如下命令即可：

```
[R1-ospf-10]bandwidth-reference 10000  //使用参考带宽分配OSPF
[R1-GigabitEthernet0/0/0]ospf timer hello 5
[R1]display ospf lsdb 
```

