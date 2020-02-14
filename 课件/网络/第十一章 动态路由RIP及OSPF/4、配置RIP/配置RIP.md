# 配置RIP

## 1、RIP基本配置

`rip [process-id]`命令用来使能RIP进程。如果未指定process-id，命令将使用1作为缺省进程ID。

```
[AR1]rip
```

`network <network-address>`命令可用于在RIP中通告网络，network-address必须是一个自然网段的地址。只有处于此网络中的接口，才能进行RIP报文的接收和发送。

```
[AR1-rip-1]network 192.168.1.0
[AR1-rip-1]network 192.168.2.0
```

 `version 2`可用于使能RIPv2以支持扩展能力，比如支持VLSM、认证等。 

[AR1-rip-1]version 2

## 2、RIP配置Metric度量值

```
[AR1]interface GigabitEthernet 0/0/0
```

配置进入的RIP报文的度量增加值，默认为0

```
[AR1-GigabitEthernet0/0/0]rip metricin 2
```

配置发出的RIP报文的度量增加值，默认为增加1

```
[AR1-GigabitEthernet0/0/0]rip metricout 3 
```

## 3、配置水平分割和毒性逆转

```
[AR1]interface GigabitEthernet 0/0/0 
```

配置水平分割

```
[AR1-GigabitEthernet0/0/0]rip split-horizon 
```

配置毒性逆转

```
[AR1-GigabitEthernet0/0/0]rip poison-reverse 
```

验证命令

```
[AR1]display rip 1 interface GigabitEthernet 0/0/0 verbose
```

## 4、配置接口RIP报文收发

```
[AR1]interface GigabitEthernet 0/0/0 
```

配置接口禁止RIP报文接收 

```
[AR1-GigabitEthernet0/0/0]undo rip input 
```

配置接口禁止RIP报文发送.