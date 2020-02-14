# jumpserver



## 认识 jumpserver



### 名词解释

- 堡垒机 Access Gateway （访问网关 接入网关）

  ![](./images/jumpserver01.png)

  堡垒机的实质是报警、记录、分析、处理等技术手段，主要功能是核心系统运维和安全审计管控，以实现保障网络和数据不受破坏的目的。
  堡垒机可作为跳板批量操作远程设备的网络设备，是系统管理员或运维人员常用的操作平台之一。

- 4A机制

  包括统一用户账号（Account [əˈkaʊnt]帐户）管理、统一认证（Authentication [ɔːˌθentɪˈkeɪʃn] 身份验证） 管理、统一授权（Authorization[ˌɔːθərəˈzeɪʃn] 授权）管理和统一安全审计（Audit [ˈɔːdɪt] 审计）四要素。
  
- 等保合规

  等级保护制度符合相关法律规定。

- web terminal（[ˈtɜːrmɪnl] 终端）

  页面终端



### 概述

- Jumpserver是全球首款完全开源的堡垒机（也称为跳板机），使用 GNU GPL v2.0 开源协议，是符合4A 的专业运维审计系统。官方网站：http://www.jumpserver.org
- Jumpserver 使用 Python /Django 进行开发，遵循 Web 2.0 规范，配备了业界领先的 Web Terminal 解决方案，交互界面美观、用户体验好。
- Jumpserver 采纳分布式架构，支持多机房跨区域部署，中心节点提供 API，各机房部署登录节点，可横向扩展、无并发访问限制。



### 特点

- 完全开源 ，GPL授权
- Python编写，方便二次开发
- 实现了跳板机基本功能，管理账号、认证、授权、审计
- 集成了Ansible（ 自动化运维工具 ），批量命令等
- 支 持 Web Terminal
- Bootstrap [ˈbutˌstræps]独自创立/引导程序）编写，界面美观
- 自动收集硬件信息
- 录像回放
- 命令搜索
- 实时监控
- 批量上传下载



### 组件说明

- Jumpserver 为管理后台，管理员可以通过Web页面进行资产管理、用户管理、资产授权等操作。 是核心组件（Core）, 使用 Django Class Based View 风格（模板视图）开发，支持 Restful（[ˈrestfl]） API（ 一种网络应用程序的设计风格和开发方式 ）。 

- Coco为SSH Server 和 Web Terminal Server 。 提供 SSH 和 WebSocket 接口, 使用 Paramiko（帕拉米科 Python模块 ） 和 Flask（ [flæsk] 瓶） 开发。 用户可以通过使用自己的账户登录SSH或者WebSocket接口直接访问被授权的资产。不需要知道服务器的账户密码。

  - paramiko包含两个核心组件：SSHClient和SFTPClient。 
    - SSHClient的作用类似于Linux的ssh命令，是对SSH会话的封装 ，该类封装了传输(Transport[ˈtrænspɔːrt , trænˈspɔːrt])，通道(Channel [ˈtʃænl])及SFTPClient建立的方法(open_sftp)，通常用于执行远程命令。  
    - SFTPClient的作用类似与Linux的sftp命令，是对SFTP客户端的封装，用以实现远程文件操作，如文件上传、下载、修改文件权限等操作。 

  - Flask是一个使用 Python 编写的轻量级 Web 应用框架。

  - Jumpserver-Python-SDK

    Coco 目前使用该 SDK 与 Jumpserver API 交互。 

- Luna（[ˈlunə] 月亮;月神;银;女子名）为Web Terminal Server 前端页面，用户使用Web Terminal方式登录所需要的组件。 前端页面都由该项目提供，Jumpserver 只提供 API，不再负责后台渲染html等。 

- Guacamole（[ˌɡwækəˈmoʊleɪ] 鳄梨酱） 为 RDP 协议和 VNC 协议资产组件, 用户可以通过 Web Terminal 来连接 RDP 协议和 VNC 协议资产 (暂时只能通过 Web Terminal 来访问) 。Guacamole 是 Apache 的跳板机项目，Jumpserver 使用其组件实现 RDP（ 远程桌面协议 remote desktop protocol） 功能，Jumpserver 并没修改其代码而是添加了额外的插件使其支持 Jumpserver 调用。



### 端口说明

- Jumpserver 默认 Web 端口为 8080/tcp, 默认 WS 端口为 8070/tcp, 配置文件 jumpserver/config.yml

  WS =  WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。允许服务端主动向客户端推送数据。

- koko 默认 SSH 端口为 2222/tcp, 默认 Web Terminal 端口为 5000/tcp 配置文件在 koko/config.yml

- Guacamole 默认端口为 8081/tcp, 配置文件 /config/tomcat9/conf/server.xml

- Nginx 默认端口为 80/tcp

- Redis 默认端口为 6379/tcp

- Mysql 默认端口为 3306/tcp



### 核心功能列表

<table class="subscription-level-table">
    <tr class="subscription-level-tr-border">
        <td class="features-first-td-background-style" rowspan="4">身份验证 Authentication</td>
        <td class="features-second-td-background-style" rowspan="3" >登录认证
        </td>
        <td class="features-third-td-background-style">资源统一登录和认证
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">LDAP 认证
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">支持 OpenID，实现单点登录
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">多因子认证
        </td>
        <td class="features-third-td-background-style">MFA（Google Authenticator）
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-first-td-background-style" rowspan="9">账号管理 Account</td>
        <td class="features-second-td-background-style" rowspan="2">集中账号管理
        </td>
        <td class="features-third-td-background-style">管理用户管理
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">系统用户管理
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style" rowspan="4">统一密码管理
        </td>
        <td class="features-third-td-background-style">资产密码托管
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">自动生成密码
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">密码自动推送
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">密码过期设置
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-outline-td-background-style" rowspan="2">批量密码变更(X-PACK)
        </td>
        <td class="features-outline-td-background-style">定期批量修改密码
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-outline-td-background-style">生成随机密码
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-outline-td-background-style">多云环境的资产纳管(X-PACK)
        </td>
        <td class="features-outline-td-background-style">对私有云、公有云资产统一纳管
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-first-td-background-style" rowspan="9">授权控制 Authorization</td>
        <td class="features-second-td-background-style" rowspan="3">资产授权管理
        </td>
        <td class="features-third-td-background-style">资产树
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">资产或资产组灵活授权
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">节点内资产自动继承授权
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-outline-td-background-style">RemoteApp(X-PACK)
        </td>
        <td class="features-outline-td-background-style">实现更细粒度的应用级授权
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-outline-td-background-style">组织管理(X-PACK)
        </td>
        <td class="features-outline-td-background-style">实现多租户管理，权限隔离
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">多维度授权
        </td>
        <td class="features-third-td-background-style">可对用户、用户组或系统角色授权
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">指令限制
        </td>
        <td class="features-third-td-background-style">限制特权指令使用，支持黑白名单
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">统一文件传输
        </td>
        <td class="features-third-td-background-style">SFTP 文件上传/下载
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">文件管理
        </td>
        <td class="features-third-td-background-style">Web SFTP 文件管理
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-first-td-background-style" rowspan="6">安全审计 Audit</td>
        <td class="features-second-td-background-style" rowspan="2">会话管理
        </td>
        <td class="features-third-td-background-style">在线会话管理
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">历史会话管理
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style" rowspan="2">录像管理
        </td>
        <td class="features-third-td-background-style">Linux 录像支持
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-third-td-background-style">Windows 录像支持
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">指令审计
        </td>
        <td class="features-third-td-background-style">指令记录
        </td>
    </tr>
    <tr class="subscription-level-tr-border">
        <td class="features-second-td-background-style">文件传输审计
        </td>
        <td class="features-third-td-background-style">上传/下载记录审计
        </td>
    </tr>
</table>



## 安装 jumpserver



### 说明

- \# 开头的行表示注释
- \> 开头的行表示需要在 mysql 中执行
- $ 开头的行表示需要执行的命令



### 环境

- 系统: CentOS 7
- IP: 192.168.244.144
- 目录: /opt
- 数据库: mariadb
- 代理: nginx



### 开始安装

```shell
$ yum update -y

# 防火墙 与 selinux 设置说明, 如果已经关闭了 防火墙 和 Selinux 的用户请跳过设置
$ systemctl start firewalld
$ firewall-cmd --zone=public --add-port=80/tcp --permanent  # nginx 端口
$ firewall-cmd --zone=public --add-port=2222/tcp --permanent  # 用户SSH登录端口 koko 
# --permanent  永久生效, 没有此参数重启后失效

$ firewall-cmd --reload  # 重新载入规则

$ setenforce 0
$ sed -i "s/SELINUX=enforcing/SELINUX=disabled/g" /etc/selinux/config

# 安装依赖包
$ yum -y install wget gcc epel-release git

# 安装 Redis, Jumpserver 使用 Redis 做 cache 和 celery（[ˈseləri] 芹菜） broker（ [ˈbroʊkər] 经纪人）
# Celery 是一个简单、灵活且可靠的，处理大量消息的分布式系统（Python语言开发），并且提供维护这样一个系统的必需工具。
# Celery需要一种解决消息的发送和接受的方式，我们把这种用来存储消息的的中间装置叫做message broker, 也可叫做消息中间人。 可选择使用redis和RabbitMQ （Rabbit [ˈræbɪt] 兔子）
$ yum -y install redis
$ systemctl enable redis
$ systemctl start redis

# 安装 MySQL, 如果不使用 Mysql 可以跳过相关 Mysql 安装和配置, 支持sqlite3, mysql, postgres（gpsql）等
# MariaDB-server 服务端和服务端工具。
# MariaDB-devel 指的是包含开发首要的文件和一些静态库。
# MariaDB-shared 指的是包含一些动态客户端的库。
# 官方介绍：https://mariadb.com/kb/en/library/about-the-mariadb-rpm-files/
$ yum -y install mariadb-server mariadb-devel MariaDB-shared
$ systemctl enable mariadb
$ systemctl start mariadb

# 创建数据库 Jumpserver 并授权
# /dev/random和/dev/urandom是Linux系统中提供的随机伪设备，这两个设备的任务，是提供永不为空的随机字节数据流。
# 这两个文件记录Linux下的熵池，所谓熵池就是当前系统下的环境噪音，描述了一个系统的混乱程度，环境噪音由这几个方面组成，如内存的使用，文件的使用量，不同类型的进程数量等等，刚开机的时候系统噪音会较小。
# 在这两个设备的差异在于：/dev/random的random pool依赖于系统中断，因此在系统的中断数不足时，/dev/random设备会一直封锁，尝试读取的进程就会进入等待状态，直到系统的中断数充分够用。/dev/urandom不依赖系统的中断，也就不会造成进程忙等待。
# /dev/random 是真随机数生成器，它会消耗熵值来产生随机数，同时在熵耗尽的情况下会阻塞，直到有新的熵生成.
# 而 /dev/urandom 是伪随机数生成器，它根据一个初始的随机种子(这个种子来源就是熵池中的熵)来产生一系列的伪随机数，而并不会在熵耗尽的情况下阻塞。
# 但是 若在系统启动阶段使用 /dev/urandom 则可能存在熵池中还不存在任何熵的情况，这时用 /dev/urandom 产生的随机数是可预测的！
# 结合这些特点，可以看出，除非要在启动启动阶段产生随机数，否则绝大多数情况下还是使用 /dev/urandom 来产生随机数，这样才不会引起程序莫名的挂起。
# /dev/urandom 才是类 Unix 操作系统下推荐的加密种子。

# tr 用于转换或删除文件中的字符
# tr 指令从标准输入设备读取数据，经过字符串转译后，将结果输出到标准输出设备。
# -d, --delete：删除指令字符
# -c, --complement：反选设定字符

# head 显示文件的开头至标准输出中（默认文件开头的前10行）
# -c n 显示文件的前n个字节
$ DB_PASSWORD=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 24`  # 生成随机数据库密码

# -e 启用反斜杠转义的解释
# \033 是ASCII字表里的 {ESC}
# {ESC}[八进制数字m 表示不同的颜色 31-37, 41-47
# 31红色 0默认值
$ echo -e "\033[31m 你的数据库密码是 $DB_PASSWORD \033[0m"

# 使用mysql的-e参数可以执行各种sql的操作
$ mysql -uroot -e "create database jumpserver default charset 'utf8'; grant all on jumpserver.* to 'jumpserver'@'127.0.0.1' identified by '$DB_PASSWORD'; flush privileges;"

# 安装 Nginx, 用作代理服务器整合 Jumpserver 与各个组件
$ vi /etc/yum.repos.d/nginx.repo

[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1

$ yum -y install nginx
$ systemctl enable nginx

# 安装 Python3.6
# python36-devel 或 python-dev 是python的开发包
# 其中包括了一些用C/Java/C#等编写的python扩展在编译的时候依赖的头文件等信息。
$ yum -y install python36 python36-devel

# 配置并载入 Python3 虚拟环境
$ cd /opt

# -m 将模块当做脚本去启动
# python xxx.py  -----直接运行的方式启动(此时脚本__name__为"__main__")
# python -m xxx.py  ------以模块的方式启动（此时脚本的__name__属性值不再是"__main__"而是"xxx"
# 区别1
# import sys
# print(sys.path)
# 以模块方式运行会多一个脚本所在路径。
# 区别2
# 当加上-m参数时，Python会先将模块或者包导入，然后再执行。
# 即 先执行 __init__ ，再执行 _main__

# Python3.3以上的版本通过venv模块原生支持虚拟环境，可以代替Python之前的virtualenv。
# env模块提供了创建轻量级“虚拟环境”，提供与系统Python的隔离支持。每一个虚拟环境都有其自己的Python二进制（允许有不同的Python版本创作环境），并且可以拥有自己独立的一套Python包。他最大的好处是，可以让每一个python项目单独使用一个环境，而不会影响python系统环境，也不会影响其他项目的环境。
$ python3.6 -m venv py3  # py3 为虚拟环境名称, 可自定义
$ source /opt/py3/bin/activate  # 退出虚拟环境可以使用 deactivate 命令

# 看到下面的提示符代表成功, 以后运行 Jumpserver 都要先运行以上 source 命令, 载入环境后默认以下所有命令均在该虚拟环境中运行
(py3) [root@localhost py3]

# 为了防止运行 Jumpserver 时忘记载入Python虚拟环境导致程序无法运行，可以使用 autoenv
$ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
# 配置用户环境变量
$ echo 'source ~/.autoenv/activate.sh' >> ~/.bashrc
# 载入用户环境变量
$ source ~/.bashrc

# 下载 Jumpserver
$ cd /opt/
# depth用于指定克隆深度，为1即表示只克隆最近一次commit.
$ git clone --depth=1 https://github.com/jumpserver/jumpserver.git

# 为了进入jumpserver目录时将自动载入python虚拟环境，建立.env文件
# /opt/py3/bin/activate 代表python的虚拟环境位置，/opt/jumpserver/表示项目文件夹，需要手动修改
$ echo "source /opt/py3/bin/activate" > /opt/jumpserver/.env

# 安装依赖 RPM 包
$ yum -y install $(cat /opt/jumpserver/requirements/rpm_requirements.txt)

# 安装 Python 库依赖
# pip 是一个现代的，通用的 Python 包管理工具。提供了对 Python 包的查找、下载、安装、卸载的功能。
# Distutils //,di:s'teɪtoʊs//可以用来在Python环境中构建和安装额外的模块。新的模块可以是纯Python的，也可以是用C/C++写的扩展模块，或者可以是Python包，包中包含了由C和Python编写的模块。
# setuptools是Python distutils增强版的集合，它可以帮助我们更简单的创建和分发Python包，尤其是拥有依赖关系的。用户在使用setuptools创建的包时，并不需要已安装setuptools，只要一个启动模块即可。
# -i, --index-url 指定库的安装源
# --upgrade 升级包
# --trusted-host 将此主机或主机端口标记为受信任，即使它没有有效的或任何HTTPS。
$ pip install -i https://mirrors.aliyun.com/pypi/simple/ --upgrade pip setuptools
# -r 从给定的需求文件安装 此选项可以多次使用
$ pip install -i https://mirrors.aliyun.com/pypi/simple/ -r /opt/jumpserver/requirements/requirements.txt

# 修改 Jumpserver 配置文件
$ cd /opt/jumpserver
$ cp config_example.yml config.yml

# 生成随机SECRET_KEY
$ SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`
$ echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc

# 生成随机BOOTSTRAP_TOKEN
$ BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`
$ echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc

$ sed -i "s/SECRET_KEY:/SECRET_KEY: $SECRET_KEY/g" /opt/jumpserver/config.yml
$ sed -i "s/BOOTSTRAP_TOKEN:/BOOTSTRAP_TOKEN: $BOOTSTRAP_TOKEN/g" /opt/jumpserver/config.yml
$ sed -i "s/# DEBUG: true/DEBUG: false/g" /opt/jumpserver/config.yml
$ sed -i "s/# LOG_LEVEL: DEBUG/LOG_LEVEL: ERROR/g" /opt/jumpserver/config.yml
$ sed -i "s/# SESSION_EXPIRE_AT_BROWSER_CLOSE: false/SESSION_EXPIRE_AT_BROWSER_CLOSE: true/g" /opt/jumpserver/config.yml
$ sed -i "s/DB_PASSWORD: /DB_PASSWORD: $DB_PASSWORD/g" /opt/jumpserver/config.yml

$ echo -e "\033[31m 你的SECRET_KEY是 $SECRET_KEY \033[0m"
$ echo -e "\033[31m 你的BOOTSTRAP_TOKEN是 $BOOTSTRAP_TOKEN \033[0m"

$ vi config.yml  # 确认内容有没有错误
# SECURITY WARNING: keep the secret key used in production secret!
# 加密秘钥 生产环境中请修改为随机字符串, 请勿外泄, PS: 纯数字不可以
SECRET_KEY:

# SECURITY WARNING: keep the bootstrap token used in production secret!
# 预共享Token koko和guacamole用来注册服务账号, 不在使用原来的注册接受机制
BOOTSTRAP_TOKEN:

# Development env open this, when error occur display the full process track, Production disable it
# DEBUG 模式 开启DEBUG后遇到错误时可以看到更多日志
DEBUG: false

# DEBUG, INFO, WARNING, ERROR, CRITICAL can set. See https://docs.djangoproject.com/en/1.10/topics/logging/
# 日志级别
LOG_LEVEL: ERROR
# LOG_DIR:

# Session expiration setting, Default 24 hour, Also set expired on on browser close
# 浏览器Session过期时间, 默认24小时, 也可以设置浏览器关闭则过期
# SESSION_COOKIE_AGE: 86400
SESSION_EXPIRE_AT_BROWSER_CLOSE: true

# Database setting, Support sqlite3, mysql, postgres ....
# 数据库设置
# See https://docs.djangoproject.com/en/1.10/ref/settings/#databases

# SQLite setting:
# 使用单文件sqlite数据库
# DB_ENGINE: sqlite3
# DB_NAME:

# MySQL or postgres setting like:
# 使用Mysql作为数据库
DB_ENGINE: mysql
DB_HOST: 127.0.0.1
DB_PORT: 3306
DB_USER: jumpserver
DB_PASSWORD:
DB_NAME: jumpserver

# When Django start it will bind this host and port
# ./manage.py runserver 127.0.0.1:8080
# 运行时绑定端口
HTTP_BIND_HOST: 0.0.0.0
HTTP_LISTEN_PORT: 8080

# Use Redis as broker for celery and web socket
# Redis配置
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379
# REDIS_PASSWORD:
# REDIS_DB_CELERY: 3
# REDIS_DB_CACHE: 4

# Use OpenID authorization
# 使用OpenID 来进行认证设置
# BASE_SITE_URL: http://localhost:8080
# AUTH_OPENID: false  # True or False
# AUTH_OPENID_SERVER_URL: https://openid-auth-server.com/
# AUTH_OPENID_REALM_NAME: realm-name
# AUTH_OPENID_CLIENT_ID: client-id
# AUTH_OPENID_CLIENT_SECRET: client-secret

# OTP settings
# OTP/MFA 配置
# OTP_VALID_WINDOW: 0
# OTP_ISSUER_NAME: Jumpserver
# 运行 Jumpserver
$ cd /opt/jumpserver
$ ./jms start -d  # 后台运行使用 -d 参数./jms start -d
# 新版本更新了运行脚本, 使用方式./jms start|stop|status all  后台运行请添加 -d 参数

$ wget -O /usr/lib/systemd/system/jms.service https://demo.jumpserver.org/download/shell/centos/jms.service
$ chmod 755 /usr/lib/systemd/system/jms.service
$ systemctl enable jms  # 配置自启

# 安装 docker 部署 koko 与 guacamole
# yum-utils 管理 repository （[rɪˈpɑːzətɔːri] 存储库）及扩展包的工具
# yum-utils 提供了 yum-config-manager
# device-mapper-persistent-data （ 存储驱动程序持久数据） 用于管理存储驱动程序的元数据
# lvm2 逻辑卷管理工具
# device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2
$ yum install -y yum-utils device-mapper-persistent-data lvm2
$ yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# makecache
# 用于下载当前启用的yum存储库的所有元数据并使之可用。
# 如果传递了参数“fast”，那么我们仅尝试确保存储库是最新的。
$ yum makecache fast
# 导入GPG公钥
$ rpm --import https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
$ yum -y install docker-ce
$ systemctl enable docker

# 修改docker镜像源
# curl -s/--silent	静默模式。不输出任何东西
# curl -S/--show-error	显示错误
# curl -L/--location	让 HTTP 请求跟随服务器的重定向。curl 默认不跟随重定向。
# sh -s 用于从标准输入中读取命令,命令在子shell中执行
# 当sh -s 后面跟的参数,从第一个非 - 开头的参数，就被赋值为子shell的$1,$2,$3....

# 该脚本可以将 --registry-mirror 加入到你的 Docker 配置文件 /etc/docker/daemon.json 中。适用于 Ubuntu14.04、Debian、CentOS6 、CentOS7、Fedora、Arch Linux、openSUSE Leap 42.1，其他版本可能有细微不同。 https://www.daocloud.io/mirror#accelerator-doc
$ curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://f1361db2.m.daocloud.io
$ systemctl restart docker

# 允许 容器ip 访问宿主 8080 端口, (容器的 ip 可以进入容器查看)
$ firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.17.0.0/16" port protocol="tcp" port="8080" accept"
$ firewall-cmd --reload
# 172.17.0.x 是docker容器默认的IP池, 这里偷懒直接授权ip段了, 可以根据实际情况单独授权IP

# 获取当前服务器 IP
# grep
# 	-A 查询匹配内容的一行之外，后n行的显示
# 	-v 不包括
# tr -d, --delete：删除指令字符
# head -n 显示文件的前n行
# cut 
# 	-d 指定字段的分隔符，默认的字段分隔符为“TAB”
# 	-f 显示指定字段的内容
$ Server_IP=`ip addr | grep 'state UP' -A2 | grep inet | egrep -v '(127.0.0.1|inet6|docker)' | awk '{print $2}' | tr -d "addr:" | head -n 1 | cut -d / -f1`
$ echo -e "\033[31m 你的服务器IP是 $Server_IP \033[0m"

# http://<Jumpserver_url> 指向 jumpserver 的服务端口, 如 http://192.168.244.144:8080
# BOOTSTRAP_TOKEN 为 Jumpserver/config.yml 里面的 BOOTSTRAP_TOKEN

# --name 为容器指定一个名称
# -d 后台运行容器，并返回容器ID
# -p 指定端口映射，格式为：主机(宿主)端口:容器端口
# -P 随机端口映射，容器内部端口随机映射到主机的高端口
# -e 设置环境变量

# Docker容器的重启策略
#	Docker容器的重启策略是面向生产环境的一个启动策略，在开发过程中可以忽略该策略。
#	Docker容器的重启都是由Docker守护进程完成的，因此与守护进程息息相关。
#	Docker容器的重启策略如下：
#	no，默认策略，在容器退出时不重启容器
#	on-failure，在容器非正常退出时（退出状态非0），才会重启容器
#	on-failure:3，在容器非正常退出时重启容器，最多重启3次
#	always，在容器退出时总是重启容器
#	unless-stopped，在容器退出时总是重启容器，但是不考虑在Docker守护进程启动时就已经停止了的容器

# docker run的退出状态码如下：
#	0，表示正常退出
#	非0，表示异常退出（退出状态码采用chroot标准）
#	125，Docker守护进程本身的错误
#	126，容器启动后，要执行的默认命令无法调用
#	127，容器启动后，要执行的默认命令不存在
#	其他命令状态码，容器启动后正常执行命令，退出命令时该命令的#返回状态码作为容器的退出状态码

# --restart设置容器的重启策略，以决定在容器退出时Docker守护进程是否重启刚刚退出的容器。
# --restart选项通常只用于detached([dɪˈtætʃt])独立的模式的容器。
# --restart选项不能与--rm选项同时使用。显然，--restart选项适用于detached模式的容器，而--rm选项适用于foreground([ˈfɔːrɡraʊnd]前景)模式的容器。
# --rm 对于foreground模式启动时设置--rm选项，这样在容器退出时就能够自动清理容器内部的文件系统。
$ docker run --name jms_koko -d -p 2222:2222 -p 127.0.0.1:5000:5000 -e CORE_HOST=http://$Server_IP:8080 -e BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN --restart=always jumpserver/jms_koko:1.5.4
$ docker run --name jms_guacamole -d -p 127.0.0.1:8081:8080 -e JUMPSERVER_SERVER=http://$Server_IP:8080 -e BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN --restart=always jumpserver/jms_guacamole:1.5.4

# 安装 Web Terminal[ˈtɜːrmɪnl] 前端: Luna  需要 Nginx 来运行访问 访问(https://github.com/jumpserver/luna/releases)下载对应版本的 release 包, 直接解压, 不需要编译
$ cd /opt
$ wget https://github.com/jumpserver/luna/releases/download/1.5.4/luna.tar.gz

# 如果网络有问题导致下载无法完成可以使用下面地址
$ wget https://demo.jumpserver.org/download/luna/1.5.4/luna.tar.gz

$ tar xf luna.tar.gz
$ chown -R root:root luna
# 配置 Nginx 整合各组件
$ rm -rf /etc/nginx/conf.d/default.conf
$ vi /etc/nginx/conf.d/jumpserver.conf

server {
    listen 80;

    # 设置允许客户端请求的最大的单个文件字节数
    client_max_body_size 100m;  # 录像及文件上传大小限制

    # location块用于匹配网页位置
    location /luna/ {
    
# 按顺序检查文件或路径是否存在，返回第一个找到的文件。如果所有的文件都找不到，会进行一个内部重定向到最后一个参数。
# $request_uri 这个变量等于从客户端发送来的原生请求URI，包括参数。它不可以进行修改。$uri变量反映的是重写后/改变的URI。不包括主机名。例如："/foo/bar.php?arg=baz"
# $uri 这个变量指当前的请求URI，不包括任何参数(见$args)。这个变量反映任何内部重定向或index模块所做的修改。注意，这和$request_uri不同，因$request_uri是浏览器发起的不做任何修改的原生URI。不包括协议及主机名。例如："/foo/bar.html"
        try_files $uri / /index.html;
        
# root和alias都可以定义在location模块中，都是用来指定请求资源的真实路径
# 区别
# 1. alias 只能作用在location中，而root可以存在server、http和location中。
# 2. alias 后面必须要用 “/” 结束，否则会找不到文件，而 root 则对 ”/” 可有可无。
# 3. 
# location /i/ {
#   root /data/w3;
# }
# 请求 http://foofish.net/i/top.gif 这个地址时，那么在服务器里面对应的真正的资源是 /data/w3/i/top.gif文件。真实的路径是root指定的值加上location指定的值。
# 而 alias 正如其名，alias指定的路径是location的别名，不管location的值怎么写，资源的真实路径都是 alias 指定的路径。比如：
# location /i/ {
#   alias /data/w3/;
# }
# 同样请求 http://foofish.net/i/top.gif 时，在服务器查找的资源路径是： /data/w3/top.gif

        alias /opt/luna/;  # luna 路径, 如果修改安装目录, 此处需要修改
    }

    location /media/ {
        # add_header 设置response header
        # Content-Encoding 服务器对响应数据的编码方式，但这里的编码方式不同于编码字符集(GB2312,UTF-8等)，而是(通常)指压缩方式
        add_header Content-Encoding gzip;
        root /opt/jumpserver/data/;  # 录像位置, 如果修改安装目录, 此处需要修改
    }

    location /static/ {
        root /opt/jumpserver/data/;  # 静态资源, 如果修改安装目录, 此处需要修改
    }

    location /koko/ {
    
# nginx中有两个模块都有proxy_pass指令 proxy [ˈprɑːksi] 代理人
# ngx_http_proxy_module的proxy_pass
# 语法: proxy_pass URL;
# 场景: location, if in location, limit_except
# 说明: 设置后端代理服务器的协议(protocol)和地址(address),以及location中可以匹配的一个可选的URI。协议可以是"http"或"https"。地址可以是一个域名或ip地址和端口，或者一个 unix-domain socket 路径。  
# 详见官方文档: http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass

# ngx_stream_proxy_module的proxy_pass
# 语法: proxy_pass address;
# 场景: server
# 说明: 设置后端代理服务器的地址。这个地址(address)可以是一个域名或ip地址和端口，或者一个 unix-domain socket路径。  
# 详见官方文档: http://nginx.org/en/docs/stream/ngx_stream_proxy_module.html#proxy_pass

# 两个proxy_pass的关系和区别
# 在两个模块中，两个proxy_pass都是用来做后端代理的指令。
# ngx_stream_proxy_module模块的proxy_pass指令只能在server段使用使用, 只需要提供域名或ip地址和端口。可以理解为端口转发，可以是tcp端口，也可以是udp端口。
# ngx_http_proxy_module模块的proxy_pass指令需要在location段，location中的if段，limit_except段中使用，处理需要提供域名或ip地址和端口外，还需要提供协议，如"http"或"https"，还有一个可选的uri可以配置。

        # 反向代理 代理转发
        proxy_pass       http://localhost:5000;
        
        # 语法：proxy_buffering on|off 
        # buffering [ˈbʌfərɪŋ] 缓冲存储
        # 默认值：proxy_buffering on
        # 上下文：http,server,location
        # 开启从后端被代理服务器的响应body缓冲
        # 如果proxy_buffering开启,nginx假定被代理的后端服务器会以最快速度响应,并把内容保存在由指令 proxy_buffer_size 和 proxy_buffers 指定的缓冲区里边。
        # 如果响应body无法放在内存里边,那么部分内容会被写到磁盘上。
        # 如果proxy_buffering被关闭了,那么响应body会按照获取body的多少立刻同步传送到客户端。nginx不尝试计算被代理服务器整个响应body的大小,nginx能从服务器接受的最大数据,是由指令 proxy_buffer_size指定的。
        proxy_buffering off;
        
        # 设置代理的HTTP协议版本。默认情况下，使用版本1.0
        proxy_http_version 1.1;
        
        # proxy_set_header用来重定义发往后端服务器的请求头
        
        # 向服务器指定某种传输协议以便服务器进行转换（如果支持）
        # HTTP的Upgrade协议头机制可以用于将连接从HTTP连接升级到WebSocket连接
        # 允许客户端从HTTP 1.1更改为HTTP 2.0
        
        # https://tools.ietf.org/html/rfc7230#section-6.7
        # demo
        # GET /hello.txt HTTP/1.1
        # Host: www.example.com
        # Connection: upgrade
        # Upgrade: HTTP/2.0, SHTTP/1.3, IRC/6.9, RTA/x11
        proxy_set_header Upgrade $http_upgrade;
        
        # 表示是否需要持久连接。（HTTP 1.1默认进行持久连接）
        # https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection
        proxy_set_header Connection "upgrade";
        
        # X-Real-IP，一般只记录真实发出请求的客户端IP
        # 最后一跳是正向代理，可能会保留真实客户端IP
        # 最后一跳是反向代理，比如Nginx，一般会是与之直接连接的客户端IP
        # $remote_addr 客户端ip地址
        proxy_set_header X-Real-IP $remote_addr;
        
        # 指定请求的服务器的域名和端口号
        # 优先级如下：HTTP请求行的主机名>”HOST”请求头字段>符合请求的服务器名
        proxy_set_header Host $host;
        
        # X-Forwarded-For是用于记录代理信息的，每经过一级代理(匿名代理除外)，代理服务器都会把这次请求的来源IP追加在X-Forwarded-For中。
        # $proxy_add_x_forwarded_for变量包含客户端请求头中的"X-Forwarded-For"，与$remote_addr用逗号分开，如果没有"X-Forwarded-For" 请求头，则$proxy_add_x_forwarded_for等于$remote_addr。$remote_addr变量的值是客户端的IP。
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # 关闭访问日志
        access_log off;
    }

    location /guacamole/ {
        proxy_pass       http://localhost:8081/;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $http_connection;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /ws/ {
        proxy_pass http://localhost:8070;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }
}
# 运行 Nginx
# -t 测试nginx.conf配置文件中是否存在语法错误
# -T 与-t相同，但另外将配置文件输出到标准输出。
$ nginx -t   # 确保配置没有问题, 有问题请先解决
$ systemctl start nginx

# 访问 http://192.168.244.144 (注意 没有 :8080 通过 nginx 代理端口进行访问)
# 默认账号: admin 密码: admin  到会话管理-终端管理 接受 koko Guacamole 等应用的注册
# 测试连接
$ ssh -p2222 admin@192.168.244.144
$ sftp -P2222 admin@192.168.244.144
  密码: admin

# 如果是用在 Windows 下, Xshell Terminal 登录语法如下
$ ssh admin@192.168.244.144 2222
$ sftp admin@192.168.244.144 2222
  密码: admin
  如果能登陆代表部署成功

# sftp默认上传的位置在资产的 /tmp 目录下
# windows拖拽上传的位置在资产的 Guacamole RDP上的 G 目录下
```