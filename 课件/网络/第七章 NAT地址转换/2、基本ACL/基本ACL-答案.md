# 基本ACL-答案

## 基础练习

1. 如果我想拒绝掉源是192.168.1.0/24，192.168.2.0/24的IP数据包，允许源是192.168.1.100和192.168.2.100这两台主机的IP数据包通过，然后允许源IP是其它网段的IP数据包通过，这个ACL应该如何写？

   ```
   rule permit source 192.168.1.100 0.0.0.0
   rule permit source 192.168.2.100 0.0.0.0
   rule deny source 192.168.1.0 0.0.0.255
   rule deny source 192.168.2.0 0.0.0.255
   rule permit source any
   ```

## 进阶练习

1. 公司的管理员希望员工只有在上班时间能够使用网关路由器访问互联网，它应该在网关路由器的入口上如何配置ACL呢？公司员工的网段为：172.16.1.0/24，172.16.2.0/24， 172.16.3.0/24

   ```
   time-range work-time 08:00 to 18:00 working-day 
   rule permit source 172.16.1.0 0.0.0.255 time-range work-time
   rule permit source 172.16.2.0 0.0.0.255 time-range work-time
   rule permit source 172.16.3.0 0.0.0.255 time-range work-time
   ```

   