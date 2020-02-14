# vsftp文件服务



## 概述

VSFTP文件服务是一个基于GPL（GNU General[ˈdʒenrəl] Public License[ˈlaɪsns] 通用公共许可证）发布的类Unix系统上使用的FTP服务器软件，它的全称是Very Secure[sɪˈkjʊr] FTP 从此名称可以看出来，编制者的初衷是代码的安全。

安全性是编写VSFTP的初衷，除了这与生俱来的安全特性以外，高速与高稳定性也是VSFTP的两个重要特点。

在速度方面，使用ASCII[ˈæski] 代码的模式下载数据时，VSFTP的速度是Wu-FTP的两倍，如果Linux主机使用2.4.*的内核，在千兆以太网上的下载速度可达86MB/S。

在稳定方面，VSFTP就更加的出色，VSFTP在单机（非集群）上支持4000个以上的并发用户同时连接，根据Red Hat的Ftp服务器的数据，VSFTP服务器可以支持15000个并发用户。

VSFTP市场应用十分广范，很多国际性的大公司和自由开源组织在使用，如：Red Hat, Suse //ˈsuːsə//，Debian // 'dai bɪn//，OpenBSD。



## 软件特点

- 它是一个安全、高速、稳定的FTP服务器；

- 它可以做基于多个IP的虚拟FTP主机服务器；

- 匿名服务设置十分方便；

- 匿名FTP的根目录不需要任何特殊的目录结构，或系统程序或其它的系统文件;

- 不执行任何外部程序，从而减少了安全隐患；

- 支持虚拟用户，并且每个虚拟用户可以具有独立的属性配置；

- 可以设置从Xinetd(扩展internet守护进程)(代替inetd)中启动，或者独立的FTP服务器两种运行方式；

- 支持两种认证方式（PAP（密码认证协议）或xinetd / tcp_wrappers）；

   wrappers[ˈræpərz] 包装纸

  tcp_wrappers 为 由 inetd 生成的服务提供了增强的安全性。提供防止主机名和主机地址欺骗的保护。
  tcp_wrappers 使用访问控制列表 (ACL) 来防止欺骗。ACL 是 /etc/hosts.allow 和 /etc/hosts.deny 文件中的系统列表。在配置为验证主机名到 IP 地址映射，以及拒绝使用 IP 源路由的软件包时，TCP Wrappers 提供某些防止 IP 欺骗的保护。

  tcp_wrappers 工作原理 ：在服务器向外提供的tcp服务商包装一层安全检测机制。外来连接请求首先通过这个安全检测，获得安全认证后才可被系统服务接受。

- 支持带宽限制；

  

## FTP连接及连接模式

- 控制连接：TCP 21，用于发送FTP命令信息
- 数据连接：TCP 20，用于上传、下载数据
- 数据连接的建立类型
  - 主动模式：服务端从 20 端口主动向客户端发起连接
  - 被动模式：服务端在指定范围内某个端口被动等待客户端连接



## FTP传输模式

-  文本模式：ASCII [ˈæski] 模式，以文本序列传输数据
-  二进制模式：Binary  [ˈbaɪnəri] 模式，以二进制序列传输数据



##  FTP 用户的类型 

匿名用户、本地用户、虚拟用户



### 常见的 FTP 服务器程序

* IIS、Serv-U

* wu-ftpd、Proftpd

* vsftpd（Very Secure FTP Daemon）

  

### 常见的 FTP 客户端程序

* ftp 命令
* CuteFTP、FlashFXP、LeapFTP、Filezilla
* gftp、kuftp

## Vsftpd 软件包

* 官方站点：http: //vsftpd.beasts.org/
* 主程序：/usr/sbin/vsftpd
* 服务名：vsftpd
* 用户控制列表文件
  * /etc/vsftpd/ftpusers
  * /etc/vsftpd/user_list

* 主配置文件：/etc/vsftpd/vsftpd.conf



### 常用配置选项

```shell
anonymous_enable=YES #是否允许匿名登陆 anonymous[əˈnɑːnɪməs]

local_enable=YES	 #允许本地登陆

write_enable=YES 	#启用任何形式的ftp写入命令

local_umask=022 	#FTP上本本地的文件权限，默认是077，不过vsftp安装后的配置文件里默认是022

anon_upload_enable=YES  	#允许匿名ftp用户上传文件

anon_mkdir_write_enable=YES  	#允许匿名用户创建新的目录

dirmessage_enables=YES 	 #激活目录消息，向远程用户发送消息，进入某个目录

xferlog_enable=YES 	 #激活上传/下载日志记录

connect_from_port_20=YES 	#确保RORT传输连接来自端口20

chown_uploads=YES
chown_username=whoever
#设置匿名用户上传文件的默认用户，不推荐使用root

xferlog_file=/var/log/xferlog 	#日志文件路径
xferlog_std_format=YES #日志文件使用标准ftpdxferlog格式的日志文件，默认位置/var/log/xferlog
idle_session_timeout=600 	#更改超时空闲会话的默认值  idle[ˈaɪdl]闲置
data_connection_timeout=120	 #超时数据连接的默认值
nopriv_user=ftpsecure	 #创建ftp服务器独立的用户
async_abor_enable=YES 	#启用异步中止(ABOR)请求 async//ei'sɪŋk// = asynchronous [eɪˈsɪŋkrənəs] 异步
ascii_upload_enable=YES 	#允许ASCII [ˈæski] 模式上传文件
ascii_download_enable=YES 	#允许ASCII模式下载文件
ftpd_banner=Welcome to blah FTP service. 	#自定义登陆标题字符串
deny_email_enable=YES 	#指定不允许匿名登陆电子邮件 deny [dɪˈnaɪ]
banned_email_file=/etc/vsftpd/banned_emails 	#email默认路径 banned [bænd] 明令禁止

chroot_local_user=YES #将所有用户限制在主目录
chroot_list_enable=YES #是否启动限制用户的名单
chroot_list_file=/etc/vsftpd/chroot_list #限制用户的名单

ls_recurse_enable=YES 	#启用ls–R选项 recurse//rɪˈkɜːrs//
listen=NO  #启用 listen 指令，vsftp 以独立模式运行侦听ipv4套接字，该指令不能同时使用withlisten_inv6指令
listen_ipv6=YES 	#监听ipv6
pam_service_name=vsftpd 	#虚拟用户使用PAM认证方式
userlist_enable=YES 	#只允许userlist文件中的账户的登录
tcp_wrappers=YES	#是否允许tcp_wrappers管理  wrappers[ˈræpərz] 包装纸
anon_other_write_enable=YES  	#允许匿名用户改名和删除文件
anon_world_readable_only=YES 	#匿名用户可以读文件，反之就是不能读
pasv_min_port=3000
pasv_max_port=3500  #PASV模式下指定端口范围 pasv=Passive [ˈpæsɪv]被动的
```



## 配置VSFTP服务器



### 匿名访问

vsftp装完默认情况下是开启匿名登录的，对应的是 /var/ftp 目录，这时只要服务启动了，就可以直接连上FTP了。默认用户名是ftp，密码是空的。

```shell
anonymous_enable=YES/NO（YES）
#控制是否允许匿名用户登入，YES 为允许匿名登入，NO 为不允许。默认值为YES。

write_enable=YES/NO（YES）
#是否允许登陆用户有写权限。属于全局设置，默认值为YES。

no_anon_password=YES/NO（NO）
#若是启动这项功能，则使用匿名登入时，不会询问密码。默认值为NO。

ftp_username=ftp
#定义匿名登入的使用者名称。默认值为ftp。

anon_root=/var/ftp	 #使用匿名登入时，所登入的目录。默认值为/var/ftp。注意ftp目录不能是777的权限属性，即匿名用户的家目录不能有777的权限。

anon_upload_enable=YES/NO（NO）
#如果设为YES，则允许匿名登入者有上传文件（非目录）的权限，只有在write_enable=YES时，此项才有效。当然，匿名用户必须要有对上层目录的写入权。默认值为NO。

anon_world_readable_only=YES/NO（YES）
#如果设为YES，则允许匿名登入者下载可阅读的档案（可以下载到本机阅读，不能直接在FTP服务器中打开阅读）。默认值为YES。

anon_mkdir_write_enable=YES/NO（NO）
#如果设为YES，则允许匿名登入者有新增目录的权限，只有在write_enable=YES时，此项才有效。当然，匿名用户必须要有对上层目录的写入权。默认值为NO。

anon_other_write_enable=YES/NO（NO）	
#如果设为YES，则允许匿名登入者更多于上传或者建立目录之外的权限，譬如删除或者重命名。（如果anon_upload_enable=NO，则匿名用户不能上传文件，但可以删除或者重命名已经存在的文件；如果anon_mkdir_write_enable=NO，#则匿名用户不能上传或者新建文件夹，但可以删除或者重命名已经存在的文件夹。）默认值为NO。

chown_uploads=YES/NO（NO）
#设置是否改变匿名用户上传文件（非目录）的属主。默认值为NO。

chown_username=username
#设置匿名用户上传文件（非目录）的属主名。建议不要设置为root。

anon_umask=077
#设置匿名登入者新增或上传档案时的umask 值。默认值为077，则新建档案的对应权限为700。

deny_email_enable=YES/NO（NO）
#若是启动这项功能，则必须提供一个档案/etc/vsftpd/banner_emails，内容为email  address。若是使用匿名登入，则会要求输入email address，
#若输入的email address 在此档案内，则不允许进入。默认值为NO。

banned_email_file=/etc/vsftpd/banner_emails
#此文件用来输入email  address，只有在deny_email_enable=YES时，才会使用到此档案。若是使用匿名登入，则会要求输入email address，若输入的email address 在此档案内，则不允许进入
```



### 安装软件

```shell
yum install vsftpd
systemctl enable vsftpd
systemctl start vsftpd
systemctl status vsftpd
```



### 防火墙方向ftp服务

```shell
firewall-cmd --add-service=ftp --permanent 
firewall-cmd --reload
```



### 客户端验证

```shell
yum install ftp
ftp 192.168.215.3
Connected to 192.168.215.3 (192.168.215.3).
220 (vsFTPd 3.0.2)
Name (192.168.215.3:root): ftp
331 Please specify the password.
Password:
230 Login successful. Remote system type is UNIX.
Using binary mode to transfer files. 
ftp> ls
227 Entering Passive Mode (192,168,154,136,54,178).
150 Here comes the directory listing.
226 Directory send OK
```



### 本地用户

```shell
anonymous_enable=NO #不允许匿名用户登入

local_enable=YES/NO（YES）
#控制是否允许本地用户登入，YES 为允许本地用户登入，NO为不允许。默认值为YES。

local_root=/home/username
#当本地用户登入时，将被更换到定义的目录下。默认值为各用户的家目录。

write_enable=YES/NO（YES）
#是否允许登陆用户有写权限。属于全局设置，默认值为YES。

local_umask=022
#本地用户新增档案时的umask 值。默认值为077。

file_open_mode=0755
#本地用户上传档案后的档案权限，与chmod所使用的数值相同。默认值为0666。

chroot_local_user=YES
#用于指定用户列表文件中的用户不允许切换到上级目录。默认值为NO
```



需要创建一个本地用户,只是用于FTP登录，不需要登录系统

```shell
useradd -s /sbin/nologin ftpuser 
echo 000000 | passwd --stdin ftpuser
```



创建一个目录，用于作为ftp用户的根目录

```shell
mkdir /pro/ftp
setfacl -m u:ftpuser:rwx /pro/ftp/
```



调整vsftp的主配置文件，关闭匿名登录，启用本地用户

```shell
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=NO
local_enable=YES 
local_root=/pro/ftp 
chroot_local_user=YES 
allow_writeable_chroot=YES
write_enable=YES
```



调整SELinux的布尔值

```shell
setsebool -P ftpd_full_access=on
getsebool -a | grep ftpd
```



编辑认证文件

```shell
vim /etc/pam.d/vsftpd
# 注释
#auth required pam_shells.so
```



重启vsftpd服务后，客户端进程测试

```shell

ftp 192.168.215.3
Connected to 192.168.215.3 (192.168.215.3).
220 (vsFTPd 3.0.2)
Name (192.168.215.3:root): ftpuser
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (192,168,215,3,207,158).
150 Here comes the directory listing.
226 Directory send OK.
ftp> mkdir aaa
257 "/aaa" created
ftp> ls
227 Entering Passive Mode (192,168,215,3,47,233).
150 Here comes the directory listing.
drwxr-xr-x    2 1005     1005            6 Nov 04 22:33 aaa
226 Directory send OK.
```



增加第二个本地用户lisi

```shell
useradd lisi
echo 123 | passwd --stdin lisi 
setfacl -m u:lisi:rwx /pro/ftp/
```

使用lisi登录，也可以创建文件,但是lisi可以删除ftpuser创建的文件及文件夹，所以为了安全，为ftp根  目录增加Sticky (SBIT)特殊权限

```shell
chmod o+t /pro/ftp/ 
ls -ld /pro/ftp/
```

更多的选项可以:man vsftpd.conf

