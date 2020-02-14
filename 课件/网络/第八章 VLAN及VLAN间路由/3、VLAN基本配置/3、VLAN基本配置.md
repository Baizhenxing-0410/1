## vlanVLAN基本配置

### 概述

VLAN的划分有很多种方法，基于交换机端口的静态VLAN方法是最常用的。

根据交换机的端口编号来划分VLAN。 通过为交换机的每个端口配置不同的PVID， 来将不同端口划分到VLAN中。

### 配置操作

#### 1、在交换机上划分VLAN时，需要首先创建VLAN。

```shell
[switch]vlan 10
[switch-vlan10]description caiwubu
// 	可以批量创建
[switch]vlan batch 2 to 9
```

+ 执行display vlan [ vlan-id [ verbose ] ]命令， 可以查看指定VLAN的详细信息， 包括VLAN  ID、  类型、  描述、  VLAN的状态、  VLAN中的端口、以及VLAN中端口的模式等。

+ 执行display vlan vlan-id statistics命令， 可以查看指定VLAN中的流量统计信息。需要开启vlan统计功能才能查看。

+ 在VLAN视图下: statistic enable 执行display vlan summary命令， 可以查看系统中所有VLAN的汇总信息。



#### 2、将交换机接口配置为access接口。

```shell
[switch]interface  GigabitEthernet  0/0/2
[switch-GigabitEthernet0/0/2]port link-type access
```



#### 3、将交换机端口加入到相应的VLAN中

```shell
[switch]interface GigabitEthernet 0/0/2
[switch-GigabitEthernet0/0/2]port default vlan 2

===================或者=========================

[switch]vlan 2
[switch-vlan2]port GigabitEthernet 0/0/2
```



### 配置接口到trunk端口

```shell
[switch]interface GigabitEthernet 0/0/3
[switch-GigabitEthernet0/0/3]port link-type trunk
[switch-GigabitEthernet0/0/3]port trunk allow-pass vlan 2 to 5
```

