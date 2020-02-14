# 配置Hybrid 答案

------

## 方案规划

### 1. vlan2 - 5

### 2. 考虑报文发出时的标签携带情况

- sw1

  | device |    PVID     |      tagged       |
  | :----: | :---------: | :---------------: |
  | tosw2  | PVID vlan 2 | tagged vlan 2 3 4 |
  |  pc1   | PVID vlan 3 | untagged vlan 2 3 |
  |  pc2   | PVID vlan 4 | untagged vlan 2 4 |

- sw2

  | device | PVID        | tagged                |
  | ------ | ----------- | --------------------- |
  | tosw1  | PVID vlan 2 | tagged vlan 2 3 4     |
  | pc3    | PVID vlan 5 | untagged vlan 2 5     |
  | server | PVID vlan 2 | untagged vlan 2 3 4 5 |
  | route  | PVID vlan 2 | untagged vlan 2 3 4 5 |

------

## 相关配置

### 1. Cloud配置

​	利用环回网卡绑定（双向通道）

### 2. 路由配置

```
// 配置ip地址

<Huawei>sys

[Huawei]interface g 0/0/0
[Huawei-GigabitEthernet0/0/0]ip address 192.168.1.3 255.255.255.0
[Huawei-GigabitEthernet0/0/0]interface g 0/0/1
[Huawei-GigabitEthernet0/0/1]ip address 192.168.2.2 255.255.255.0

<Huawei>save
```



### 3. lsw2交换机配置

```
<Huawei>sys

[Huawei]undo info enable
[Huawei]vlan batch 2 to 5
[Huawei]interface g 0/0/1

// 设置0/0/1端口为hybrid
[Huawei-GigabitEthernet0/0/1]port link-type hybrid
// 设置端口pvid
[Huawei-GigabitEthernet0/0/1]port hybrid pvid vlan 2
// 设置hybrid的untagged属性
[Huawei-GigabitEthernet0/0/1]port hybrid untagged vlan 2 3 4 5
[Huawei-GigabitEthernet0/0/1]interface g 0/0/2

[Huawei-GigabitEthernet0/0/2]port link-type hybrid
[Huawei-GigabitEthernet0/0/2]port hybrid pvid vlan 2
[Huawei-GigabitEthernet0/0/2]port hybrid tagged vlan 2 3 4
[Huawei-GigabitEthernet0/0/2]interface g 0/0/3

[Huawei-GigabitEthernet0/0/3]port link-type hybrid
[Huawei-GigabitEthernet0/0/3]port hybrid pvid vlan 2
[Huawei-GigabitEthernet0/0/3]port hybrid untagged vlan 2 3 4 5
[Huawei-GigabitEthernet0/0/3]interface g 0/0/4

[Huawei-GigabitEthernet0/0/4]port link-type hybrid
[Huawei-GigabitEthernet0/0/4]port hybrid pvid vlan 5
[Huawei-GigabitEthernet0/0/4]port hybrid untagged vlan 2 5

<Huawei>save
```



### 4. lsw1交换机配置

```
<Huawei>sys

[Huawei]undo info enable
[Huawei]vlan batch 2 to 4
[Huawei]interface g 0/0/1

[Huawei-GigabitEthernet0/0/1]port link-type hybrid
[Huawei-GigabitEthernet0/0/1]port hybrid pvid vlan 2
[Huawei-GigabitEthernet0/0/1]port hybrid tagged vlan 2 3 4
[Huawei-GigabitEthernet0/0/1]interface g 0/0/2

[Huawei-GigabitEthernet0/0/2]port link-type hybrid
[Huawei-GigabitEthernet0/0/2]port hybrid pvid vlan 3
[Huawei-GigabitEthernet0/0/2]port hybrid untagged vlan 2 3
[Huawei-GigabitEthernet0/0/2]interface g 0/0/3

[Huawei-GigabitEthernet0/0/3]port link-type hybrid
[Huawei-GigabitEthernet0/0/3]port hybrid pvid vlan 4
[Huawei-GigabitEthernet0/0/3]port hybrid untagged vlan 2 4

<Huawei>save
```