# TCP报头结构-答案

## 基础练习

1. 端口号起到什么作用？

   端口号的作用，主要是区分服务类别和在同一时间进行多个会话。

2. 列举5个TCP应用程序，以及都采用的哪些端口？

   nginx 80

   FTP 21

   ssh 22

   dns 53

   snmp 161 等

3. TCP有哪4种计时器？

   1、重传计时器：Retransmission Timer

   2、坚持计时器：Persistent Timer

   3、保活计时器：Keeplive Timer

   4、时间等待计时器：Timer_Wait Timer

4. TCP是如何做流量控制的？

   TCP滑动窗口技术通过动态改变窗口大小来实现对端到端设备之间的数据传输进行流量控制。

5. TCP位于TCP/IP的哪一层？

   传输层

## 进阶练习

1. 下列哪项不是TCP协议为了确保应用程序之间的可靠通信而使用的（D）

   A. ACK控制位

   B. 序列编号

   C. 校验和

   D. 紧急指针