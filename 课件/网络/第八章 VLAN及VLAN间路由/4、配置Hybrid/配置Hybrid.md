# 配置Hybrid

华为交换机默认的端口类型就是Hybrid端口，它类似于trunk端口，可以允许多个

VLAN信息通过，还可以规定哪些VLAN打标签，哪些VLAN不打标签，合理的配置好Hybrid端口，可以起到安全隔离的作用。

1 、 配 置 端 口 类 型 为 Hybrid

```
[sw1] interface GigabitEthernet 0/0/1
[sw1-GigabitEthernet0/0/1] port link-type hybrid
```

2、配置Hybrid端口的PVID

```
[sw1-GigabitEthernet0/0/1] port hybrid pvid vlan 2
```

3、配置Hybrid端口上打标签的VLAN和不打标签的VLAN

```
[sw1-GigabitEthernet0/0/1]port hybrid untagged vlan 2 3
[sw1-GigabitEthernet0/0/1]port hybrid tagged vlan 4 6
```