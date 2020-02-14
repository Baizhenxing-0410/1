# **VRRP简介**

随着网络的快速普及和相关应用的日益深入，各种增值业务（如IPTV、视频会议等） 已经开始广泛部署，基础网络的可靠性日益成为用户关注的焦点，**能够保证网络传输不中断对于终端用户非常重要。**

**通常**，**同一网段内**的所有主机上都设置一条相同的、以**网关为下一跳的缺省路由**。主机发往其他网段的报文将通过缺省路由发往网关，再由网关进行转发，**从而实现主机与外部网络的通信**。当**网关发生故障时**，本网段内所有以网关为缺省路由的主机将**无法与外部网络通信**。**增加出口网关**是提高系统**可靠性的常见方法**，此时如何在多个出口之间进行选路就成为需要解决的问题。

**VRRP**（Virtual Router Redundancy Protocol）**虚拟路由冗余协议**的出现很好的解决了这个问题。VRRP能够在不改变组网的情况下，采用将**多台**路由设备**组成一个虚拟路由器**，通过**配置虚拟路由器的IP地址为默认网关，实现默认网关的备份**。**当网关设备发生故障时，VRRP机制能够选举新的网关设备承担数据流量，从而保障网络的可靠通信。**



![wps1](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps1.jpg)

# VRRP协议的基本概念：

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)运行VRRP协议的设备，它可能属于一个或多个虚拟路由器，如SwitchA和SwitchB。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**虚拟路由器**（Virtual Router）：又称VRRP备份组，由一个Master设备和多个Backup设备组成，被当作一个共享局域网内主机的缺省网关。如SwitchA和SwitchB 共同组成了一个虚拟路由器。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**Master路由器**（Virtual Router Master）：承担转发报文任务的VRRP设备， 如SwitchA。



![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**Backup路由器**（Virtual Router Backup）：一组没有承担转发任务的VRRP设备，当Master设备出现故障时，它们将通过竞选成为新的Master设备，如  SwitchB。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**虚拟路由器的标识**。如SwitchA和SwitchB组成的虚拟路由器的VRID为

1。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**虚拟IP地址**(Virtual IP Address)：虚拟路由器的IP地址，一个虚拟路由器可以有

一个或多个IP地址，由用户配置。如SwitchA和SwitchB组成的虚拟路由器的虚拟IP 地址为10.1.1.10/24。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**IP地址拥有者**（IP Address Owner）：如果一个VRRP设备将虚拟路由器IP地址作为真实的接口地址，则该设备被称为IP地址拥有者。如果IP地址拥有者是可用的， 通常它将成为Master。如SwitchA，其接口的IP地址与虚拟路由器的IP地址相同，均为10.1.1.10/24，因此它是这个VRRP备份组的IP地址拥有者。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)**虚拟MAC地址**（Virtual MAC Address）：虚拟路由器根据虚拟路由器ID生成的MAC地址。一个虚拟路由器拥有一个虚拟MAC地址，格式为：00-00-5E-00-01-

{VRID}(VRRP for IPv4)；00-00-5E-00-02-{VRID}(VRRP for IPv6)。当虚拟路由器

回应ARP请求时，使用虚拟MAC地址，而不是接口的真实MAC地址。如SwitchA和SwitchB组成的虚拟路由器的VRID为1，因此这个VRRP备份组的MAC地址为00-00- 5E-00-01-01。

VRRP协议报文用来将Master设备的优先级和状态通告给同一备份组的所有Backup设备。VRRP协议报文封装在IP报文中，发送到分配给VRRP的IP组播地址。在IP报文头中，源地址为发送报文接口的主IP地址（不是虚拟IP地址），目的地址是224.0.0.18，TTL是255，协议号是112。

目前，VRRP协议包括两个版本：VRRPv2和VRRPv3。

VRRPv2仅适用于IPv4网络，VRRPv3适用于IPv4和IPv6两种网络。

VRRPv3不支持认证功能，而VRRPv2支持认证功能。VRRP认证方式有3种：无认证、简单字符认证、MD5认证。

 

# **VRRP工作过程**

VRRP的工作过程如下：

\1. VRRP备份组中的设备根据优先级选举出Master。Master设备通过发送免费ARP 报文，将虚拟MAC地址通知给与它连接的设备或者主机，从而承担报文转发任务。

\2. Master设备周期性向备份组内所有Backup设备发送VRRP通告报文，以公布其配置信息（优先级等）和工作状况。

\3. 如果Master设备出现故障，VRRP备份组中的Backup设备将根据优先级重新选举新的Master。



\4. VRRP备份组状态切换时，Master设备由一台设备切换为另外一台设备，新的Master设备会立即发送携带虚拟路由器的虚拟MAC地址和虚拟IP地址信息的免费ARP报文，刷新与它连接的主机或设备中的MAC表项，从而把用户流量引到新的Master设备上来，整个过程对用户完全透明。

\5. 原Master设备故障恢复时，若该设备为IP地址拥有者（优先级为255），将直接切换至Master状态。若该设备优先级小于255，将首先切换至Backup状态，且其优先级恢复为故障前配置的优先级。

\6. Backup设备的优先级高于Master设备时，由Backup设备的工作方式（抢占方式和非抢占方式）决定是否重新选举Master。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)抢占模式：在抢占模式下，如果Backup设备的优先级比当前Master设备的优先级高，则主动将自己切换成Master。

![wps2](E:\班里\网络基础\网络基础整理与答案\第十章 LACP及VRRP协议-day5\images\wps2.png)非抢占模式：在非抢占模式下，只要Master设备没有出现故障，Backup设备即使随后被配置了更高的优先级也不会成为Master设备。

 

# **课后练习题：**

1、VRRP主要用来做什么？

2、使用VRRP技术后，终端PC的网关设置为哪个地址？

3、简述VRRP工作原理

4、VRRP中的抢占模式作用是什么？