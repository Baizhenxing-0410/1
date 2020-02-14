# STP状态和计时器-答案

## 基础练习

1. STP有哪5种状态？哪些状态是稳定状态？

   Forwarding：转发状态。端口既可转发用户流量也可转发BPDU报文，只有根端口或指定端口才能进入Forwarding状态。

    

   Learning：学习状态。端口可根据收到的用户流量构建MAC地址表，但不转发用户流量。增加Learning状态是为了防止临时环路。

    

   Listening：侦听状态。端口可以转发BPDU报文，但不能转发用户流量。Blocking：阻塞状态。端口仅仅能接收并处理BPDU，不能转发BPDU，也不能转发

    

   用户流量。此状态是预备端口的最终状态。

    

   Disabled：禁用状态。端口既不处理和转发BPDU报文，也不转发用户流量。


   Forwarding，Blocking

2. blocking和forwarding状态有哪些不同？

   Forwarding：转发状态。端口既可转发用户流量也可转发BPDU报文

   Blocking：阻塞状态。端口仅仅能接收并处理BPDU，不能转发BPDU，也不能转发用户流量。

3. Max Age是指什么？

   Max Age是指BPDU报文的老化时间

## 进阶练习

