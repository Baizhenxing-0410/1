# 基本ACL

基本ACL可以使用报文的源IP地址、分片标记和时间段信息来匹配报文，其编号取值范围是2000-2999

## 1、编写规则

`acl [ number ]` 命令用来创建一个ACL，并进入ACL视图。

 

`rule [ rule-id ] { deny | permit } source { source-address sourcewildcard | any }` 命令

 

用来增加或修改ACL的规则。deny用来指定拒绝符合条件的数据包，permit用来指定允许符合条件的数据包，source用来指定ACL规则匹配报文的源地址信息，any表示任意源地址。

例如：

 

```
创建acl2100
[Huawei]acl 2100
```

 

```
拒绝源ip为192.168.1.0/24的数据通过
[Huawei-acl-basic-2100]rule deny source 192.168.1.0 0.0.0.255
```

 

```
允许源ip为192.168.2.0/24的数据通过
[Huawei-acl-basic-2100]rule permit source 192.168.2.0 0.0.0.255
```

 

```
拒绝源ip为192.168.3.100的数据通过
[Huawei-acl-basic-2100]rule deny source 192.168.3.100 0
```

 

```
查看这个acl
[Huawei-acl-basic-2100]dis this

 

[V200R003C00]

#
acl number 2100
rule 5 deny source 192.168.1.0 0.0.0.255 rule 10 permit source 192.168.2.0 0.0.0.255 rule 15 deny source 192.168.3.100 0
#
return
```

## 2、将规则应用到接口上

`traffic-filter { inbound | outbound }acl{ acl-number }`命令用来在接口上配置基于ACL对报文进行过滤。

例如：

 

```
进入 g 0/0/0 接口
[Huawei]interface GigabitEthernet 0/0/0
```

```
绑定入站规则
[Huawei-GigabitEthernet0/0/0]traffic-filter inbound acl 2100
```

## 3、检查确认

执行`display acl <acl-number>`命令可以验证配置的基本ACL。

 

执行`display traffic-filter applied-record`命令可以查看设备上所有基于ACL进行报文过滤的应用信息，这些信息可以帮助用户了解报文过滤的配置情况并核对其是否正确，同时也有助于进行相关的故障诊断与排查。 

```
[Huawei]display traffic-filter applied-record

-----------------------------------------------------------

Interface	           Direction     AppliedRecord
-----------------------------------------------------------
GigabitEthernet0/0/1	outbound	acl 2000
GigabitEthernet0/0/0	inbound	    acl 2100

-----------------------------------------------------------
```

```
[Huawei]display acl 2100

Basic ACL 2100, 3 rules

Acl's step is 5

rule 5 deny source 192.168.1.0 0.0.0.255 
rule 10 permit source 192.168.2.0 0.0.0.255 
rule 15 deny source 192.168.3.100 0
```

## 4、可以添加ACL生效时间

先建立一个时间范围

 

```
[Huawei]time-range work-time 09:00 to 17:00 working-day 
```

写ACL时进行时间段调用

```
[Huawei]acl 2500

[Huawei-acl-basic-2500]rule deny source 192.168.5.0 0.0.0.255 time-range work-time
```



