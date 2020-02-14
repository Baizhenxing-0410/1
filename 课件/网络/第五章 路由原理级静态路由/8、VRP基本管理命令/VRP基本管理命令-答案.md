# VRP基本管理命令-答案

## 基础练习

1. 华为设备接口认证模式有哪三种？

   AAA，密码认证，不认证

2. 缺省情况下命令级别分为多少级？

   0-3

3. 缺省情况下用户级别分为多少级？

   0-15

4. 设备一般最多支持 多少个用户同时通过 VTY方式访问？

   15

5. display命令默认一页输出的行数为多少？

   24

6. display命令一页最多输出的行数为多少？

   512

7. 如何修改历史缓冲区可存储的命令数？

   history-command max-size

8. Loopback是什么接口？

   Loopback接口是一个逻辑接口，可用来虚拟一个网络或者一个IP主机。

## 进阶练习

1. Loopback接口的状态一般是怎样的？

   up

2. 打开eNSP模拟器，拖拽2个路由器设备，并将其g0/0/0接口相连，开启路由器电源，然后做以下配置：

   - 设置两台设备的名称分别为R1和R2
   - 设置两台设备的时区到东8区，并配置时间为当前北京时间
   - 配置两台设备的console接口认证为密码认证，密码为:test123456;
   - 配置两台设备的登录前标题信息：大意是公司设备，不能随便未授权登录，违者需承担法律责任
   - 配置R1地址为192.168.1.1/24,配置R2地址为192.168.1.2/24,并测试可以通信

      ```
      sysname R1
      sysname R2
      ```

      ```
      clock timezone bj add 08:00:00 
      ```

      

      ```
      user-interface console 0
      authentication-mode password
      输入test123456
      ```

      

      ```
      header login information "Company equipment, can not be arbitrarily unauthorized login, violators should bear legal responsibility"
      ```

      ```
      ip add 192.168.1.1/24
      
      ip add 192.168.1.2/24
      ```

      ```
      ping 192.168.1.2
      
      测试通过
      ```