# SElinux



## 概述

Security [sɪˈkjʊrəti] Enhanced [ɪnˈhænst] Linux 的缩写，也就是安全强化的 Linux，是由美国国家安全局（NSA）联合其他安全机构（比如 SCC 公司）共同开发的，旨在增强传统 Linux 操作系统的安全性，解决传统 Linux 系统中自主访问控制（DAC）系统中的各种权限问题（如 root 权限过高等）。

只要进程和文件的安全上下文匹配，该进程就可以访问该文件资源。



## 常用查看

1. 查看所有的fcontext
   `semanage fcontext --list | more`

2. 查看所有OBJECTS的授权端口号
   `semanage port --list | more`

3. 查看文件SElinux属性

   `ls -Z`

5. 查看SELinux中的身份、角色、类型、策略

   `yum install setools-console`
   seinfo 命令格式如下：
   `seinfo [选项]`
   选项：
   -u： 列出SELinux中所有的身份（user）；
   -r： 列出SELinux中所有的角色（role）；
   -t： 列出SELinux中所有的类型（type）；
   -b： 列出所有的布尔值（也就是策略中的具体规则名称）；
   -x： 显示更多的信息；

6. 查看规则具体内容

   sesearch 命令格式如下：
   `sesearch [选项] [规则类型] [表达式]`

   选项：
   
- -h：显示帮助信息；
  
   规则类型：
   
   - --allow：显示允许的规则；
- --neverallow：显示从不允许的规则；
   - --all：显示所有的规则；
   
   表达式：
   
   - -s 主体类型：显示和指定主体的类型相关的规则（主体是访问的发起者，这个 s 是 source 的意思，也就是源类型）；
   - -t 目标类型：显示和指定目标的类型相关的规则（目标是被访问者，这个 t 是 target 的意思，也就是目标类型）；
   - -b 规则名：显示规则的具体内容（b 是 bool，也就是布尔值的意思，这里是指规则名）；
   
   demo
   
   - 如果我们知道的是规则的名称，则应该如何查询具体的规则内容。命令如下：
   
     ```shell
     # 查询和apache相关的规则，有httpd_manage_ipa规则
     seinfo -b | grep http
     httpd_manage_ipa
     …省略部分输出…
     
     # 查询httpd_manage_ipa规则中具体定义了哪些规则内容
     sesearch --all -b httpd_manage_ipa
     Found 4 semantic av rules：
     allow httpd_t var_run_t：dir { getattr search open } ;
     allow httpd_t memcached_var_run_t：file { ioctl read write create getattr setattr lock append unlink link rename open } ;
     allow httpd_t memcached_var_run_t：dir { ioctl read write getattr lock add_name remove_name search open } ;
     allow httpd_t var_t：dir { getattr search open } ;
     Found 20 role allow rules：
     allow system_r sysadm_r;
     allow sysadm_r system_r;
     …省略部分输出…
     
     # 每个规则中都定义了大量的具体规则内容，这些内容比较复杂，一般不需要修改，会查询即可。
     ```
   
   - 可是我们有时知道的是安全上下文的类型，而不是规则的名称。比如，我们已知 apache 进程的域是 httpd_t，而 /var/www/html/ 目录的类型是 httpd_sys_content_t。而 apache 之所以可以访问 /var/www/html/ 目录，是因为 httpd_t 域和 httpd_sys_content_t 类型匹配。那么，该如何查询这两个类型匹配的规则呢？命令如下：
   
     ```shell
     # apache进程的域是httpd_t
     ps auxZ | grep httpd
     unconfined_u:system_r:httpd_t:s0 root 25620 0.0 0.5 11188 36X6 ? Ss
     
     # /var/www/html/ 目录的类型是 httpd_sys_content_t
     ls -Zd /var/www/html/
     drwxr-xr-x. root root system_u:object_r:httpd_sys_content_t:s0 /var/www/html/
     
     # 查看规则明细
     sesearch --all -s httpd_t -t httpd_sys_content_t
     ...省略部分输出...
     allow httpd_t httpd_sys_content_t : file { ioctl read getattr lock open };
     allow httpd_t httpd_sys_content_t : dir { ioctl read getattr lock search open };
     allow httpd_t httpd_sys_content_t : lnk_file { read getattr };
     allow httpd_t httpd_sys_content_t : file { ioctl read getattr lock open };
     ...省略部分输出...
     # 可以清楚地看到httpd_t域是允许访间和使用httpd_sys_content_t类型的
     ```



## 安全上下文

安全上下文看起来比较复杂，它使用“：”分隔为 4 个字段，其实共有 5 个字段，只是最后一个“类别”字段是可选的，例如：
system_u：object_r：httpd_sys_content_t：s0：[类别]
#身份字段：角色：类型：灵敏度：[类别]

下面对这 5 个字段的作用进行说明。



### 1.  身份字段（user)

用于标识该数据被哪个身份所拥有，相当于权限中的用户身份。这个字段并没有特别的作用，知道就好。常见的身份类型有以下 3 种：

- root：表示安全上下文的身份是 root。
- system_u：表示系统用户身份，其中“_u”代表 user。
- user_u：表示与一般用户账号相关的身份，其中“_u”代表 user。

user 字段只用于标识数据或进程被哪个身份所拥有，一般系统数据的 user 字段就是 system_u，而用户数据的 user 字段就是 user_u。



### 2. 角色（role）

主要用来表示此数据是进程还是文件或目录。这个字段在实际使用中也不需要修改，所以了解就好。

常见的角色有以下两种：
- object_r：代表该数据是文件或目录，这里的“_r”代表 role。
- system_r：代表该数据是进程，这里的“_r”代表 role。



### 3. 类型（type）

类型字段是安全上下文中最重要的字段，进程是否可以访问文件，主要就是看进程的安全上下文类型字段是否和文件的安全上下文类型字段相匹配，如果匹配则可以访问。

注意，类型字段在文件或目录的安全上下文中被称作类型（type），但是在进程的安全上下文中被称作域（domain）。也就是说，在主体（Subject）的安全上下文中，这个字段被称为域；在目标（Object）的安全上下文中，这个字段被称为类型。域和类型需要匹配（进程的类型要和文件的类型相匹配），才能正确访问。



### 4. 灵敏度

灵敏度一般是用 s0、s1、s2 来命名的，数字代表灵敏度的分级。数值越大，代表灵敏度越高。

### 5. 类别
类别字段不是必须有的，所以我们使用 ls 和 ps 命令查询的时候并没有看到类别字段。但是我们可以通过 seinfo 命令来查询：`seinfo -u -x`

# Samba文件服务器



## 概述

在windows网络环境中，主机之间进行文件和打印机共享是通过微软公司自己的SMB/CIFS网络协议实现的。SMB（Server Message Block，服务消息块）和CIFS（Common Internet File System，通用互联网文件系统）协议是微软的私有协议，在Samba  [ˈsæmbə] 项目出现之前，并不能直接与Linux/UNIX系统进行
通信。

Samba是著名的开源软件项目之一，它在Linux/UNIX系统中实现了微软的SMB/CIFS网络协议，从而使得跨平台的文件共享变得更加容易。在部署Windows、Linux/UNIX混合平台的企业环境时，选用Samba可以很好地解决不同系统之间的文件互访问题。

要将Samba配置为作为工作组成员来提供SMB文件共享需执行以下基本步骤：

1. 安装Samba软件包

2. 准备要共享的目录的权限

3. 配置/etc/samba/smb.conf文件

4. 使用NTLMv2密码设置相应的Linux用户

   NTLM = WindowsNT LAN Manager = WindowsNT挑战/响应验证机制

5. 启动Samba并打开本地防火墙

6. 验证是否可以从客户端挂载共享



## 安装Samba

安装samba软件包，它是服务器端的软件包

```shell
yum install samba
```



### 准备要共享的目录

如果某个目录本身便是NFS导出或者挂载的NFS文件系统，请勿使用Samba来共享此目录。这可能导致文件损坏、文件锁过时或共享方面的其他文件访问问题。

```shell
# -p, --parents 需要时创建上层目录，如目录早已存在则不当作错误
mkdir -p /pro/share
```



### 用户和常规权限

对目录设置的权限取决于需要访问此目录的用户以及客户端挂载此目录的方式。

客户端一般是通过作为特定用户来验证对SMB服务器的访问权限，从而挂载共享。共享中的所有文件需要能够由用于挂载共享的用户进行读取（并可能写入）。



### SELinux上下文及布尔值

要在SELinux处于强制模式时候使Samba正确运行，目录需要具有正确的SELinux上下文并且可能需要设置某些SELinux布尔值。如果将仅通过Samba来访问共享目录，则目录及其所有子目录和文件都应以samba_share_t来标记，这将为Samba提供读写访问权限。

```shell
# -d 列出目录本身，而不是目录内容
# -Z显示安全上下文 仅显示模式、用户、组、安全上下文和文件名
ll -Zd /pro/share

# semanage 命令是用来查询与修改SELinux默认目录的安全上下文
# fcontext 主要用在安全上下文方面

# -a：增加，可以增加一些目录的默认安全上下文类型设置
# -m：修改
# -d：删除

# -l：列举
# -n：不打印说明头
# -D：全部删除
# -f：文件
# -s：用户
# -t：类型
# -r：角色

semanage fcontext -a -t samba_share_t '/pro/share(/.*)?' 

# estorecon命令用来恢复SELinux文件属性即恢复文件的安全上下文
# 它使用SELinux策略中的规则来确定应该是哪种文件上下文
# -i：忽略不存在的文件。
# -f：infilename 文件 infilename 中记录要处理的文件。
# -e：directory 排除目录。
# -R/-r：递归处理目录。
# -n：不改变文件标签。
# -o/outfilename：保存文件列表到 outfilename，在文件不正确情况下。
# -v：将过程显示到屏幕上。
# -F：强制恢复文件安全语境。
restorecon -vFR /pro/share/

# 查看结果
ls -lZd /pro/share
drwxr-xr-x. root root system_u:object_r:samba_share_t:s0 /pro/share/

# Samba还可以提供使用SELinux类型public_content_t（只读）和public_content_rw_t(读写)标记   的文件。要允许对标记为public_content_rw_t的文件和目录进行读写访问，必须还要启用SELinux  布尔值smbd_anon_write。
```



## 主配置文件

Smba的主配置文件是/etc/samba/smb.conf文件。

此文件分为多个节。每节以节名称（括在方括号中）开头，后面是设置为特定值的参数的列表。任何以分号或井号字符开头的行都将被注释掉

### [global] 节

定义Samba服务器的基本配置。

- workgroup： 用于指定服务器的Windows工作组

- security： 控制Samba对客户端进行身份验证的方式。centos6之前可用值有share、user、server、domain，centos7之后不再支持share，如果配置匿名共享时，需要在全局参数中添加  “map to guest = bad user”这一行内容

- passwd backend：设置共享账户文件的类型，默认使用tdbsam（TDB数据库文件）

- hosts allow：是允许访问Samba服务的主机列表（以逗号、空格或制表符分割）。如果未指定， 则所有主机均可访问Samba。如果[global]节中未指定此设置，则可以单独在每个共享中设置。如  果在[global]节中指定，将适用于所有共享，无论每个共享是否具有不同的设置。设置形式多样：

  - 172.25.0.0/24
  - 172.25.0.0/255.255.255.0
  - 172.25.0. 
  - [2001:db8:0:1::/64]
  - desktop.example.com
  - example.com

- demo

  ```shell
  hosts allow = 172.25.
  hosts allow = 172.25.	
  hosts allow = example.com
  ```



### [homes] 节

定义了一个默认启用的特殊文件共享，即共享了用户家目录.



### [printers] 节

打印机共享设置：若需要共享打印机设备，可以在这部分进行配置。



### 文件共享节

要创建文件共享，要在/etc/samba/smb.conf的末尾，将共享名称放在方括号中以启动共享的新节

1. 必须设置path以指定要共享的目录
   `path = /pro/share`

2. 如果所有经过身份验证的用户对共享具有读写访问权限，应设置writable=yes。默认writable=no。如果设置了writable=no，则可以提供对共享具有读写权限的用户的write  list(以逗号分隔)。不在列表中的用户将具有只读权限，如果指定本地组成员，可以写成：write list = @management

3. valid [ˈvælɪd]  users(如果设置)指定允许访问共享的用户的列表。不允许列表以外用户访问共享。如果列表为空，则所有用户均可访问共享。仅允许用户fred和management组的成员具有只读访问权限， 应为

   ```shell
   [myshare]
   path = /pro/share
   writable = no
   valid users = fred, @management
   ```

4. browseable：该共享目录在“网上邻居”中是否可见

5. guest ok：是否允许所有人访问。等效于“public”

6. comment：对共享目录的注释、说明信息



### 验证配置文件语法

使用testparm命令，不带任何参数允许即可



## 准备Samba用户

1. security=user设置需要一个包含Samba账户（具有有效的NTLM密码）的Linux账户。建议锁定Linux账户密码，并将登录shell设置为/sbin/nologin

   `useradd -s /sbin/nologin fred`
   
2. samba-client包含smbpasswd命令，其可以创建Samba用户并设置密码

   ```shell
   yum install samba-client
   # -a : 添加Samba用户
   # -x : 删除Samba用户和密码
   # -d : 禁用Samba用户
   # -e : 启用Samba用户
   smbpasswd -a fred
   # pdbedt比smbpasswd更强大的工具
   # pdbedit -L
   ```

   

## 启动Samba

1. Samba依赖2个服务程序：smbd和nmbd

   - smbd：通常使用TCP 445端口进行SMB连接，为了向后兼容，还侦听TCP 139端口

   - nmbd: 使用UDP 137和138端口，提供基于NetBIOS名称解析 （NetBIOS（Network Basic Input/Output System）协议是一种在局域网上的程序可以使用的应用程序编程接口（API））

     ```shell
     systemctl start smb nmb
     systemctl enable smb nmb
     netstat -antp|egrep '(445|139)'
     netstat -anup|egrep '(137|138)'
     ```

2. 要在ﬁrewalld防火墙中放行Samba服务

   ```shell
   firewall-cmd --permanent --add-service=samba
   firewall-cmd --reload
   ```



## 挂载SMB文件系统 



### 安装客户端工具

cifs-utils软件包可用于在本地系统上挂载SMB文件共享，无论是来自Samba服务器还是本机Microsoft Windows服务器。

`yum install samba-client cifs-utils`



### 常规SMB挂载

1. 默认情况下，SMB挂载使用单一一组用户凭据（挂载凭据）来挂载共享及确定对共享中文件的访问权限。使用挂载的Linux系统上的所有用户均使用系统凭据来确定文件访问权限。smbclient 命令可以列出服务器上的共享目录：`smbclient -L //192.168.215.3 -U fred`

2. smbclient命令也可以登录服务器的共享目录（类似于ftp登录方式）

   ```shell
   smbclient //192.168.215.3/myshare -U fred
   EnterSAMBA\fred'spassword:
   smb:\>ls
   smb:\> exit
   ```

3. mount命令用于挂载共享。默认情况下，用于对用户进行身份验证的协议是封装在原始NTLMSSP消息中的NTLMv2密码散列（security=ntlmssp（NT LM安全性支持提供者服务））。可以通过两种方法来提供挂载凭据：

   - 交互式挂载，可以使用username=选项来指定要进行身份验证的SMB用户，将提示此用户输入密码。
   - 自动挂载，可以通过credentials=选项来提供凭据文件，此文件只能由root用户读取，且包含用户名和密码。

   ```shell
   # 创建挂载路径
   mkdir /mnt/share
   
   # 挂载
   mount -o username=fred //192.168.215.3/myshare /mnt/share/
   Password for fred@//192.168.215.3/myshare:  ******
   
   ll /mnt/share/
   ```



### 使用Samba的多用户挂载

当挂载Samba共享时，默认情况下挂载凭据确定对挂载点的访问权限。新的multiuser（多用户）挂载选项将挂载凭据与用于确定每个用户的文件访问权限的凭据进行隔离，可以与security=ntlmssp身份验证一起使用。

root用户使用multiuser选项以及一个SMB用户名（对共享内容具有最低访问权限）挂载共享。常规用户随后可以使用cifscreds命令将其自己的SMB用户名和密码存储在当前会话的内核密钥环中，其对共享的访问权限是通过来自密钥环的自身凭据而非挂载凭据来验证。用户可以随时清除或更改其针对该登录会话的凭据，并且在会话结束后将会清除。文件访问权限完全由SMB服务器根据当前使用的访问凭据强制实施。



#### 实现方案

```shell
#####################服务端配置##############################
# 配置服务器上可读写用户mary
useradd -s /sbin/nologin mary

# 创建smb密码
smbpasswd -a mary
# 增加mary路径权限
setfacl -m u:mary:rwx /pro/share/

# 编辑配置文件增加可写用户列表
vim /etc/samba/smb.conf

# 修改以下内容
[share]
	path = /pro/share
	writable = no
	valid users = fred,mary,@management
	write list = mary
	
# 重启服务
systemctl restart smb nmb


##################客户端进行多用户测试##########################
# 重新挂载
mount -o multiuser,sec=ntlmssp,username=fred,password=000000 //192.168.215.3/myshare /mnt/share
df -h

# 增加用户
useradd mary
useradd fred

su - fred

# 需要命令cifscreds以将身份验证凭据存储在本地用户的密钥环中。这些身份验证凭据将转发到多用户挂载上的Samba服务器。cifs-utils软件包提供cifscreds命令，客户端系统上需要安装。
# cifscreds 命令具有各种操作
# add：可向用户的会话密钥环中添加SMB凭证，此选项后跟SMB文件服务器的主机名
# update：可更新用户的会话密钥环中的现有凭据，此选项后跟SMB文件服务器的主机名
# clear：可从用户的会话密钥环中删除特定条目，此选项后跟Samba文件服务器的主机名
# clearall：可从用户的会话密钥环中清除所有现有凭据。

cifscreds add 192.168.215.3
Password：
cd /mnt/share/
ls
touch file

su - mary
cifscreds add 192.168.215.3
Password：
cd /mnt/share/
touch file1
ll
```
