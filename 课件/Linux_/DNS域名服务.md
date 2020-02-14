# DNS域名服务



## DNS 概述

DNS是互联网的一个基础服务。

我们知道用户在与互联网上的主机通信时，必须 知道对方的 IP 地址。但是每个 IP 地址都是由 32 位的二进制组成，即使是十进制的 IP 地址表示形式，用户想要记住也是很难的一件事，况且互联网有那么多的主机。

互联网中的主机通常不仅仅只有IP地址，还有对应的便于用户记忆的主机名字，比如www\.baidu.com。产生于应用层上的域名系统 DNS(Domain Name System)就可以用来把互联网上的主机名转换成 IP 地址。

互联网中的域名系统DNS被设计成一个层次树状结构的联机分布式数据库系统，并且采取的是客户服务器的方式。DNS使大多数名字都在本地进行解析，只有少量的解析需要在互联网通信，因此效率很高。采取分布式的一个好处是，即使单个计算机出了故障，也不会妨碍DNS系统的正常运行。

域名到 IP 地址的解析是通过许多分布在互联网上的域名服务器完成的。解析的主要过程如下：当一个主机中的进程需要把域名解析为 IP 地址时，该进程就会调用解析程序，并成为DNS的一个客户，把待解析的域名放在 DNS 的请求报中，以UDP用户数据报方式发送给本地域名服务器。本地域名服务器在查找域名后，把对应的IP地址放在回答报文中返回。获得IP地址后的主机即可进行通信。



## DNS系统结构

- DNS是一个层次结构的分布式的数据库，如下所示：

![](images/dns1.png)



## 域名服务器



### 区

一个服务器所负责管辖的（或有权限的）范围叫做区(zone)。各单位根据具体情况来划分自己管辖范围的区。但在一个区中的所有节点必须是能够连通的。每一个区设置相应的权限域名服务器，用来保存该区中的所有主机的域名到IP地址的映射。DNS 服务器的管辖范围不是以“域”为单位，而是以“区”为单位。区 <= 域。区是域的子集。

![](images/dns3.png)

树状结构的 DNS 域名服务器

![](images/dns4.png)



### 分类

域名服务器有以下四种类型

- 根域名服务器
- 顶级域名服务器
- 权限域名服务器
- 本地域名服务器



#### 根域名服务器

根域名服务器(root name server)是最高层次的域名服务器，也是最重要的域名服务器，全球共设有 13个根域名服务器(这13台IPv4根域名服务器名字分别为“A”至“M”）。1个为主根服务器在美国。其余12个均为辅根服务器，其中9个在美国，欧洲2个，位于英国和瑞典，亚洲1个位于日本。

数量限制：DNS协议使用了端口上的UDP和TCP协议，UDP通常用于查询和响应，TCP用于主服务器和从服务器之间的传送。由于在所有UDP查询和响应中能保证正常工作的最大长度是512字节，512字节限制了根服务器的数量和名字。（要让所有的根服务器数据能包含在一个512字节的UDP包中，IPv4根服务器只能限制在13个）

所有的根域名服务器都知道所有的顶级域名服务器的域名和 IP地址。当其他的域名服务器无法解析域名时，会首先求助于根域名服务器。

这些根域名服务器相应的域名分别是
a.rootservers.net
b.rootservers.net
…
m.rootservers.net



#### 顶级域名服务器

顶级域名服务器(top-level-domain[doʊˈmeɪn] ) 负责管理在该顶级域名服务器上注册的所有二级域名。
当收到 DNS 查询请求时，就给出相应的回答（可能是最后的结果，也可能是下一步应当找的域名服务器的 IP 地址）。
每个国家都有自己的顶级域名服务器。

| 域名 | 含义                   |
| ---- | ---------------------- |
| com  | 商业机构               |
| net  | 网络服务机构           |
| org  | 非营利性组织、社会团体 |
| gov  | 政府机构               |
| edu  | 教育机构               |
| mil  | 军事机构               |
| cn   | 中国                   |
| jp   | 日本                   |
| uk   | 英国                   |
| us   | 美国                   |



#### 权限域名服务器

它是负责一个区的域名服务器，当一个权限域名服务器没有给出最后的查询结果时，就会告诉发出查询请求的DNS客户，下一步应当查询哪一个权限域名服务器。



#### 本地域名服务器

本地域名服务器(local name server)并不属于上面图示的服务器层次结构，但是它在域名服务系统却发挥着至关重要的作用。当一台主机发出 DNS 查询请求时，这个查询请求报文就会发送给本地域名服务器。每一个互联网提供者，或者一个大学，甚至小到一个学院，都可以拥有一台本地域名服务器，这种域名服务器也被称为默认域名服务器。我们本地网络服务连接的域名服务器指的就是本地域名服务器。



### 提高域名服务器的可靠性

DNS 域名服务器都把数据复制到几个域名服务器来保存，其中的一个是主域名服务器，其他的是辅助域名服务器。 当主域名服务器出故障时，辅助域名服务器可以保证 DNS 的查询工作不会中断。 主域名服务器定期把数据复制到辅助域名服务器中，而更改数据只能在主域名服务器中进行。这样就保证了数据的一致性。



## DNS查询过程

DNS查询过程如下图所示：

![](images/dns2.png)



### 递归查询

主机向本地域名服务器的查询一般都采用递归查询(recursive[rɪˈkɜːrsɪv] query)。所谓的递归查询就是：如果主机所询问的本地域名服务器不知道被查出来的域名的 IP 地址，那么本地域名服务器就以 DNS 客户的身份，向其他根域名服务器继续发出查询请求报文(替代该主机继续查询)，而不是主机自己进行下一步的查询。因此，递归查询返回的结果要么是所查询的 IP 地址，要么报错，表示无法查到所需要的 IP。
本地域名服务器采用递归查询（比较少用）。

![](images/dns6.png)



### 迭代查询

本地域名服务器向根域名服务器的查询方式通常采取迭代查询(iterative query)。迭代查询有以下的特点：当根域名服务器收到本地域名服务器发出的迭代查询请求报时，要么给出所要查询的 IP 地址，要么告诉本地域名服务器：“我这里没有你要的查询结果，你需要向哪一台域名服务器进行查询”。然后本地域名服务器进行后续的查询(不替代本地域名服务器)。

![](images/dns5.png)



### 正向查询

根据主机名称（域名）查找对应的 IP 地址



### 反向查询

根据 IP 地址查找对应的主机域名



## DNS服务器功能分类



### 缓存域名服务器

- 也称为高速缓存服务器
- 通过向其他域名服务器查询获得域名 -> IP地址记录
- 将域名查询结果缓存到本地，提高重复查询时的速度



### 主域名服务器

- 特定 DNS 区域的官方服务器，具有唯一性。
- 负责维护该区域内所有域名 -> IP 地址的映射记录



### 从域名服务器

- 也称为辅助域名服务器
- 其维护的域名 -> IP地址记录来源于主域名服务器



## 搭建DNS服务器

DNS服务器软件为BIND（Berkeley Internet Name Domain，伯克利互联网域名），BIND是美国加利福尼亚大学伯克利分校开发的一个域名服务软件包，Linux使用这个软件包来提供域名服务，该软件实现了DNS协议。BIND的服务端软件是被称作named的守护进程。



### 安装软件

```shell
yum install bind bind-chroot bind-utils
```

- bind：提供了域名服务的主要程序及相关文件；

- bind-chroot：为BIND服务提供一个伪装的根目录（将/var/named/chroot/文件夹作为BIND的根目录），以提高安全性。

- bind-utils：（utils工具类）提供了对DNS服务器的测试工具程序，如nslookup等。

- 备注：

  CentOS7不同于6，只需要安装bind-chroot，就会自动安装主程序包bind和库bind-libs。

  CentOS7下安装了bind-chroot之后，若要使用named-chroot.service，则需要关闭named.service。两者只能运行一个



### 配置文件介绍

使用BIND软件构建域名服务器，主要涉及两种类型的配置文件：主配置文件和区域数据文件。其中，主配置文件用于设置named服务的全局选项、注册区域及访问控制等各种运行参数；区域数据文件用于保存 DNS 解析记录的数据文件（正向或反向记录）。

BIND配置文件保存在两个位置

- /etc/named.conf 	BIND服务主配置文件

- /var/named/                zone文件（域的dns信息）

  如果安装了bind-chroot，BIND会被封装到一个伪根目录内，原先的文件配置文件的路径位置变为：

- /var/named/chroot/etc/named.conf          ----BIND服务主配置文件

- /var/named/chroot/var/named/     ---------zone文件



### 复制配置相关文件

bind-chroot安装好之后不会有预制的配置文件，但是在BIND的文档文件夹内（/usr/share/doc/bind-9.11.4），BIND为我们提供了配置文件模板，我们可以直接拷贝过来：

```shell
# 拷贝bind相关文件,准备 bind chroot 环境
cp -R /usr/share/doc/bind-*/sample/var/named/* /var/named/chroot/var/named/

# 将 /etc/named.conf 拷贝到 bind chroot目录
# -p 复制后目标文件保留源文件的属性 （包括所有者、所属组、权限和时间）
cp -p /etc/named.conf /var/named/chroot/etc/named.conf

# 在 bind chroot 的目录中创建相关文件
touch /var/named/chroot/var/named/data/cache_dump.db
touch /var/named/chroot/var/named/data/named_stats.txt
touch /var/named/chroot/var/named/data/named_mem_stats.txt
touch /var/named/chroot/var/named/data/named.run
mkdir /var/named/chroot/var/named/dynamic

# 修改文件属主和属组
chown -R named:named /var/named/chroot/var/named

# 启动服务
systemctl start named-chroot
```



## 构建缓存域名服务器



### 案例介绍

- 缓存域名服务器的IP地址为192.168.215.3。
- 局域网内的PC机将首选DNS服务器设为192.168.215.3。
- 缓存域名服务器能够访问Internet中的其他DNS服务器。
- 负责处理局域网PC机的DNS解析请求，并缓存查询结果。



### 基本步骤

1. 建立named.conf主配置文件，通过根域或者转发器机制指定解析源。
2. 确认建立named.ca根区域数据文件，若使用转发器机制则无需此步骤。
3. 启动named服务。
4. 验证缓存域名服务器。



### 实现方案

```shell
# 0 备份配置文件
cp /var/named/chroot/etc/named.conf /var/named/chroot/etc/named.conf.bak


# 1 修改named.conf主配置文件
vim /var/named/chroot/etc/named.conf

# 修改如下配置

# 指定数据文件的存放目录 默认无需修改
directory 	"/var/named";

# 指定服务器监听的地址和端口
listen-on port 53       { 127.0.0.1; 192.168.215.3; };

# 指定允许内网网段地址主机可以查询授权区域数据
allow-query					{ 192.168.215.0/24; };

# 哪些主机查询非授权区域记录时，为其做递归
allow-query-cache       { 192.168.215.0/24; };

# 开启递归 默认为开启
# 如果你要建立一个 授权域名服务器 服务器, 那么不要开启 recursion（递归） 功能。
# 如果你要建立一个 递归 DNS 服务器, 那么需要开启recursion 功能。
# 如果你的递归DNS服务器有公网IP地址, 你必须开启访问控制功能，
# 只有那些合法用户才可以发询问. 如果不这么做的话，那么你的服
# 服务就会受到DNS 放大攻击。实现BCP38将有效抵御这类攻击。
recursion yes;


########################使用转发器#################################
# 如果不使用根区域，也可以选择设置转发器，即可以指向另外一个DNS服务器。
# forward 可以是first，也可以是only
# forward first设置优先使用forwarders DNS服务器做域名解析，如果查询不到再使用本地DNS服务器做域名解析。
# forward only设置只使用forwarders DNS服务器做域名解析，如果查询不到则返回DNS客户端查询失败。

# 在 options{}; 中加入以下内容
# forward only;
# forwarders {8.8.8.8};
##################################################################


# 2 配置根区域 这样服务器不知道域名记录时，就去找根DNS服务器 在区域文件目录中必须存在"/var/named/named.ca"这个文件，里面记录了根DNS服务器的地址（共13个）

# 常见DNS记录的类型
# A：地址记录，用来指定域名的IPv4地址，如果需要将域名指向一个ip地址，就需要添加A记录。
# AAAA：用来指定主机名（或域名）对应的IPv6地址记录。
# CNAME：如果需要将域名指向另一个域名，再由另一个域名提供ip地址，就需要添加CNAME记录。
# MX：如果需要设置邮箱，让邮箱能够收到邮件，需要添加MX记录。
# NS：域名服务器记录，如果需要把子域名交给其他DNS服务器解析，就需要添加NS记录。
# SOA：这种记录时所有区域性文件中的强制性记录。它必须是一个文件中的第一个记录。 SOA叫做起始授权机构记录，NS用于标识多台域名解析服务器，SOA记录用于在众多NS记录中哪一台是主服务器。
# SRV：添加服务记录服务器服务记录时会添加此项，SRV记录了哪台计算机提供了哪个服务。格式为：服务的名字.协议的类型（例如：_example-server._tcp）。
# TXT：可以写任何东西，长度限制为255。绝大多数的TXT记录是用来做SPF记录（反垃圾邮件）。
# PTR记录： PTR记录是A记录的逆向记录，又称做IP反查记录或指针记录，负责将IP反向解析为域名。
# 显性URL转发记录： 将域名指向一个http(s)协议地址，访问域名时，自动跳转至目标地址。例如：将 www.aaa.cn 显性转发到 www.bbb.com 后，访问 www.aaa.cn 时，地址栏显示的地址为：www.bbb.com。
# 隐性UR转发记录L： 将域名指向一个http(s)协议地址，访问域名时，自动跳转至目标地址，隐性转发会隐藏真实的目标地址。例如：将 www.aaa.cn 隐性转发到 www.bbb.com 后，访问www.aaa.cn 时，地址栏显示的地址仍然是：www.aaa.cn。
cat /var/named/chroot/var/named/named.ca | grep -v '^;'|grep -v '^$'


# 3 检查配置文件语法
named-checkconf /var/named/chroot/etc/named.conf


# 4 重启服务
systemctl restart named-chroot

# bug: bash[8205]: /etc/named.conf:180: bad secret 'bad base64 encoding'
# cd /var/named/chroot/var/
# dnssec-keygen -a HMAC-MD5 -b 128 -n HOST "test_key"
# cat Ktest_key.+157+02552.private # Key: ……
# secret "m86/eOGsJ4cEYvc8VL9PgQ==";


###############################################################
##################客户端验证是否可以解析#########################
###############################################################
# 5 安装客户端命令行工具
yum install bind-utils


# 6 配置首选DNS为192.168.215.3
cp /etc/resolv.conf /etc/resolv.conf.bak
vim /etc/resolv.conf
# 修改以下内容
nameserver 192.168.215.3


# 7 解析域名
# 第一台客户端解析会慢一些，第二台客户端解析相同域名会很快，因为有了缓存。
nslookup www.baidu.com
```



## 构建主域名服务器



### 案例介绍

- 负责abc.com域的解析
- 网站服务器 www\.abc.com，IP地址为192.168.215.10
- 邮件服务器 mail.abc.com,IP地址为192.168.215.20
- 在线视频服务器 video.abc.com，IP地址为192.168.215.30
- 主域名服务器 ns1.abc.com ，IP地址为192.168.215.3
- 从域名服务器 ns2.abc.com，IP地址为192.168.215.5



### 基本步骤

1. 确认本机网络地址、主机映射、默认DNS服务器地址。
2. 建立主配置文件named.conf。
3. 建立正、反向区域数据文件。
4. 启动named服务或重载配置。
5. 验证主域名服务器。



### 实现方案

```shell
# 1 确认本机网络地址、主机映射、DNS服务器地址
# 为了提高域名解析效率，将两个DNS服务器的地址映射直接写入到/etc/hosts文件中
vim /etc/hosts
192.168.215.3	ns1.abc.com		ns1
192.168.215.5	ns2.abc.com		ns2

# 指定两个DNS服务器的地址为首选和备份DNS
vim /etc/resolv.conf

# 写入以下内容
# search 定义域名的搜索列表
nameserver 192.168.215.3
nameserver 192.168.215.5


# 2 修改主配置文件
vim /var/named/chroot/etc/named.conf


# 在 zone "." IN 前面加入以下内容

# 一个zone关键字定义一个域区

# type类型有三种，它们分别是master,slave和hint它们的含义分别是：
# master:表示定义的是主域名服务器
# slave [sleɪv]:表示定义的是辅助域名服务器
# hint [hɪnt]暗示:表示是互联网中根域名服务器

# file 指定具体存放DNS记录的文件

# allow-transfer [trænsˈfɜːr , ˈtrænsfɜːr]指定哪些主机可以从服务器上接收区域传输,未指定将允许传说到所有的主机
zone "abc.com" IN { 
	type master;
	file "abc.com.zone";
	allow-transfer {192.168.215.5;};
};

# 定义一个IP为192.168.215.*的反向域区 
zone "215.168.192.in-addr.arpa" IN { 
	type master;
	file "192.168.215.arpa.zone"; 
	allow-transfer {192.168.215.5;};
};


# 检查配置文件语法
named-checkconf /var/named/chroot/etc/named.conf


# 3 建立正向区域数据文件
cp -p /var/named/named.localhost /var/named/chroot/var/named/abc.com.zone
vim /var/named/chroot/var/named/abc.com.zone

# 修改以下内容
# “@”表示当前的DNS区域名，相当于“abc.com.”	
# $TTL（Time To Live，生存时间）记录
# SOA（Start Of Authority[əˈθɔːrəti]，授权信息开始）记录
# 分号“;” 开始的部分表示注释信息
# NS 域名服务器（Name Server）：记录当前区域的DNS服务器的主机地址
# MX 邮件交换（Mail  Exchange[ɪksˈtʃeɪndʒ]）：记录当前区域的邮件服务器的主机地址，数字10表示优先级。
# A 地址（Address）：记录正向解析条目，只用在正向解析区域中
# CNAME 别名（Canonical[kəˈnɑːnɪkl] Name）：记录某一个正向解析条目的其他名称
# PTR   指针（Point）记录，只用在反向解析区域中

$TTL 1D
@	IN SOA	abc.com. admin.abc.com. (
	2019101701	; serial
	1D	; refresh
	1H	; retry
	1W	; expire
	3H )	; minimum
				
@	IN	NS	ns1.abc.com.
	IN	NS	ns2.abc.com.
		MX 10 mail.abc.com.
www IN	A	192.168.215.10
mail IN	A	192.168.215.20
video IN	A	192.168.215.30
ns1	IN	A	192.168.215.3
ns2	IN	A	192.168.215.5


# 验证区域数据文件
# named-checkzone zonename filename
named-checkzone abc.com /var/named/chroot/var/named/abc.com.zone


# 4 建立反向区域数据文件
cd /var/named/chroot/var/named/
cp -p abc.com.zone 192.168.215.arpa.zone
192.168.215.arpa.zone

# 修改以下内容
$TTL 1D
@	IN SOA	abc.com. admin.abc.com. (
	2019101701	; serial
	1D	; refresh
	1H	; retry
	1W	; expire
	3H )	; minimum
	
@	IN	NS	ns1.abc.com. 
@	IN	NS	ns2.abc.com.
10	IN	PTR www.abc.com.
20	IN	PTR mail.abc.com.
30	IN	PTR study.abc.com.
3 IN	PTR ns1.abc.com.
5 IN	PTR ns2.abc.com.

# 验证区域数据文件
named-checkzone 215.168.192.in-addr.arpa 192.168.215.arpa.zone

# 5 重启服务
systemctl restart named-chroot


###############################################################
##################客户端验证是否可以解析#########################
###############################################################
nslookup
> video.abc.com
Server:		192.168.56.3
Address:	192.168.56.3#53

Name:	video.abc.com
Address: 192.168.56.30
> set type=mx
> abc.com
Server:		192.168.56.3
Address:	192.168.56.3#53

abc.com	mail exchanger = 10 mail.abc.com.
> set type=ptr
> 192.168.56.30
Server:		192.168.56.3
Address:	192.168.56.3#53

30.56.168.192.in-addr.arpa	name = study.abc.com.
> 192.168.56.10
Server:		192.168.56.3
Address:	192.168.56.3#53

10.56.168.192.in-addr.arpa	name = www.abc.com.
```



## 构建从域名服务器

为了降低主域名服务器的压力，可以构建从域名服务器，不仅可以负载分担，还可以起到备份作用。



### 基本步骤

1. 确认本机网络地址、主机映射、默认DNS服务器地址。
2. 建立主配置文件named.conf。

3. 启动named服务，查看区域数据文件是否下载成功。
4. 验证从域名服务器。



### 实现方案

```shell
# 1 安装
yum install bind bind-chroot bind-utils


# 2 启动服务
systemctl start named-chroot.service


# 3 确认本机网络地址、主机映射、DNS服务器地址
# 为了提高域名解析效率，将两个DNS服务器的地址映射直接写入到/etc/hosts文件中
vim /etc/hosts
192.168.215.3	ns1.abc.com		ns1
192.168.215.5	ns2.abc.com		ns2

# 指定两个DNS服务器的地址为首选和备份DNS
vim /etc/resolv.conf

# 写入以下内容
# search 定义域名的搜索列表
nameserver 192.168.215.3
nameserver 192.168.215.5


# 4 建立主配置文件
cd /var/named/chroot/
vim /etc/named.conf

# 修改如下配置
listen-on port 53	{ 127.0.0.1; 192.168.215.5; };


zone "abc.com" IN { 
    type slave;
    file "slaves/abc.com.zone";
    masters {192.168.215.3;};
};
zone "215.168.192.in-addr.arpa" IN { 
    type slave;
    file "slaves/192.168.215.arpa.zone"; 
    masters {192.168.215.3;};
};


# 5 关闭两个机器的防火墙


# 6 重启服务
systemctl restart named-chroot


# 7 查看是否同步成功
ll /var/named/chroot/var/named/slaves/


# 8 使用从域名服务器进行验证
nslookup
> server 192.168.56.2
Default server: 192.168.56.2
Address: 192.168.56.2#53
> www.abc.com
Server:		192.168.56.2
Address:	192.168.56.2#53

Name:	www.abc.com
Address: 192.168.56.10
> set type=ptr
> 192.168.56.30
Server:		192.168.56.2
Address:	192.168.56.2#53

30.56.168.192.in-addr.arpa	name = study.abc.com.

```

