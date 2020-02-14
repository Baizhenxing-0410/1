## 课后习题答案

### 实验环境如下：

![图片1](../../../../%E7%BD%91%E7%BB%9C%E8%AF%BE%E7%A8%8B/%E9%9F%A9%E5%AE%97%E4%BD%90-04/%E7%AC%AC%E5%85%AB%E7%AB%A0%20VLAN%E5%8F%8AVLAN%E9%97%B4%E8%B7%AF%E7%94%B1/3%E3%80%81VLAN%E5%9F%BA%E6%9C%AC%E9%85%8D%E7%BD%AE/images/%E5%9B%BE%E7%89%871.png)

 

要求：通过配置vlan，让相同VLAN的主机可以互相通信提交文件：关键的配置命令整理后，提交到答题框。

```shell
LSW2
[Huawei]int g  0/0/1
[Huawei-GigabitEthernet0/0/3]port link-type trunk 
[Huawei-GigabitEthernet0/0/3]port trunk allow-pass vlan 2 to 10
[Huawei]vlan batch 2 to 10
Info: This operation may take a few seconds. Please wait for a moment...done.
[Huawei]interface g 0/0/1
[Huawei-GigabitEthernet0/0/1]port link-type access
[Huawei-GigabitEthernet0/0/1]port default vlan 2
[Huawei]interface g 0/0/2 
[Huawei-GigabitEthernet0/0/2]port link-type access
[Huawei-GigabitEthernet0/0/2]port default vlan 3


LSW3
Huawei]interface g  0/0/1
[Huawei-GigabitEthernet0/0/3]port link-type trunk 
[Huawei-GigabitEthernet0/0/3]port trunk allow-pass vlan 2 to 10
[Huawei]vlan batch 2 to 10
Info: This operation may take a few seconds. Please wait for a moment...done.
[Huawei]interface g 0/0/1
[Huawei-GigabitEthernet0/0/1]port link-type access 
[Huawei-GigabitEthernet0/0/1]port default vlan 2
[Huawei]interface g 0/0/2
[Huawei-GigabitEthernet0/0/2]port link-type access 
[Huawei-GigabitEthernet0/0/2]port default vlan 3

```



