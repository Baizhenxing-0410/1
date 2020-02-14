## 根据Dockerfile构建出一个镜像

​	**Dockfile是一种被Docker程序解释的脚本**，Dockerfile由一条一条的指令组成，每条指令对应Linux下面的一条命令。Docker程序将这些Dockerfile指令翻译真正的Linux命令**。Dockerfile**有自己书写格式和支持的命令**，Docker程序解决这些命令间的依赖关系，
类似于Makefile。Docker程序将读取Dockerfile，根据指令生成定制的image。**相比
image这种黑盒子，**Dockerfile**这种**显而易见的脚本**更容易被使用者接受，它**明确**的表明
**image是怎么产生的**。有了Dockerfile，当我们需要定制自己额外的需求时，只需在
Dockerfile上添加或者修改指令，重新生成image即可，省去了敲命令的麻烦。

​	Dockerfile由一行行命令语句组成，**并且支持以# 开头的注释行**。
​	**Dockerfile的指令是忽略大小写的，建议使用大写，每一行只支持一条指令，每条指令**
**可以携带多个参数。**
**Dockerfile的指令**根据**作用**可以分**为两种**：**构建指令和设置指令。**
​	构建指令用于构建image，其指定的操作不会在运行image的容器上执行；
​	设置指令用于设置image的属性，其指定的操作将在运行image的容器中执行。
一般的，**Dockerfile分为四部分**：**基础镜像信息、维护者信息、镜像操作指令和容器启动时**
**执行指令**。



### 下面是一个例子：

\# This dockerfile uses the ubuntu image
\# VERSION 2 - EDITION 1
\# Author: docker_user
\# Command format: Instruction [arguments / command] ..
\# Base image to use, this must be set as the first line

 **第一行必须指明基于的基础镜像**
<font color=Red>FROM ubuntu</font>

\# Maintainer: docker_user<docker_user at email.com> (@docker_user)
\#维护该镜像的用户信息
<font color=Red> MAINTAINER docker_user docker_user@email.com </font>
\# Commands to update the image

**#镜像操作**
<font color=Red>RUN echo "deb http://archive.ubuntu.com/ubuntu/ raring main universe" >> /etc/apt/sources.list</font>
<font color=Red>RUN apt-get update && apt-get install -y nginx</font>
<font color=Red>RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf</font>

#开启80
<font color=Red>EXPOSE </font>80
#Commands when creating a new container

#启动容器时执行的命令
<font color=Red>CMD /usr/sbin/nginx </font>

**在编写dockerfile时，有严格的格式要遵循：**
	其中，**一开始必须使用FROM指令指明所基于的镜像名称**，接下来使用**MAINTAINER指**
**令说明维护者信息**。**后面则是镜像操作指令**，例如RUN指令，RUN 指令将对镜像执行跟随
的命令。每运行一条RUN 指令，都会给基础镜像添加新的一层并提交。最后是CMD指令，
来指定运行容器时的操作命令。



## dockerfile指令

指令的一般格式为INSTRUCTION arguments，**指令包括FROM 、MAINTAINER 、RUN等**

### (1) FROM（指定基础image）

**构建指令，必须指定且需要在Dockerfile其他指令的前面**。后续的指令都依赖于该指令指定的image。FROM指令指定的基础image可以是官方远程仓库中的，也可以位于本地仓库。
**该指令有两种格式：**
**FROM <image>**
指定基础image为该image的最后修改的版本。
**或者：**
**FROM <image>:<tag>**
**指定基础image为该image的一个tag版本。**



### （2）MAINTAINER（用来指定镜像创建者信息）

构建指令，用于将image的制作者相关的信息写入到image中。当我们对该image执行docker inspect命令时，输出中有相应的字段记录该信息。
**格式：**
**MAINTAINER <name>**



### （3）RUN（安装软件用）

**构建指令，RUN可以运行任何被基础image支持的命令**。如基础image选择了ubuntu，那么软件管理部分只能使用ubuntu的命令。
该指令有两种格式：
**RUN <command> (the command is run in a shell - `/bin/sh -c`)**
**RUN ["executable", "param1", "param2" ... ] (exec form)**
**前者将在 shell 终端中运行命令，即/bin/sh -c ；后者则使用exec 执行**。
指定使用其它终端可以通过第二种方式实现，例如 RUN ["/bin/bash", "-c", "echo hello"]。
每条RUN指令将在当前镜像基础上执行指定命令，并提交为新的镜像。当命令较长时可以使用“\”来换行。



### （4）CMD（设置container启动时执行的操作）

**该指令有三种格式：
设置指令，用于container启动时指定的操作。该操作可以是执行自定义脚本，也可以是执行系统命令。**
CMD ["executable","param1","param2"] 使用exec 执行，推荐方式；
CMD command param1 param2 在/bin/sh中执行，提供给需要交互的应用；
当Dockerfile指定了ENTRYPOINT，那么使用下面的格式：
CMD ["param1","param2"] 提供给ENTRYPOINT 的默认参数；
ENTRYPOINT指定的是一个可执行的脚本或者程序的路径，该指定的脚本或者程序将会
param1和param2作为参数执行。所以如果CMD指令使用上面的形式，那么Dockerfile中必须要有配套的ENTRYPOINT。
**指定启动容器时执行的命令，每个Dockerfile只能有一条CMD 命令。如果指定了多条命令**，只有最后一条会被执行。如果用户启动容器时候指定了运行的命令，则会覆盖掉CMD指定的命令**



### 5）ENTRYPOINT（设置container启动时执行的操作）

**设置指令，指定容器启动时执行的命令，可以多次设置，但是只有最后一个有效。**
**两种格式:**
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2 （shell中执行）。
**配置容器启动后执行的命令，并且不可被docker run提供的参数覆盖。**
**每个Dockerfile 中只能有一个ENTRYPOINT,当指定多个时,只有最后一个起效。**
该指令的使用分为两种情况，一种是独自使用，另一种和CMD指令配合使用。
**当独自使用时，如果你还使用了CMD命令且CMD是一个完整的可执行的命令**，那么CMD指令和ENTRYPOINT会互相覆盖只有最后一个CMD或者ENTRYPOINT有效。**
**例如：** CMD指令将不会被执行，只有ENTRYPOINT指令被执行
CMD echo “Hello, World!”
ENTRYPOINT ls -l
另一种用法和CMD指令配合使用来指定ENTRYPOINT的默认参数，这时CMD指令不是一个完整的可执行命令，仅仅是参数部分；ENTRYPOINT指令只能使用JSON方式指定执行命令，而不能指定参数。
**例如：**
FROM ubuntu
CMD ["-l"]
ENTRYPOINT ["/usr/bin/ls"]



### （6）USER（设置container容器的用户，默认是root用户）

**格式为:**
**USER daemon**
指定运行容器时的用户名或UID，后续的RUN 也会使用指定用户。
当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户
例如： RUN groupadd -r postgres&&useradd -r -g postgres postgres**
**例如： 指定memcached的运行用户**
**ENTRYPOINT ["memcached"]**
**USER daemon**
**或**
**ENTRYPOINT ["memcached", "-u", "daemon"]**



### （7）EXPOSE（指定容器需要映射到宿主机器的端口）

**格式为:**
**EXPOSE <port> [<port>...]**
设置指令，该指令会将容器中的端口映射成宿主机器中的某个端口。当你需要访问容器的时候，可以不是用容器的IP地址而是使用宿主机器的IP地址和映射后的端口。要完成整个操作需要两个步骤，首先在Dockerfile使用EXPOSE设置需要映射的容器端口，然后在运行容器的时候指定-p选项加上EXPOSE设置的端口，这样EXPOSE设置的端口号会被随机映射成宿主机器中的一个端口号。也可以指定需要映射到宿主机器的哪个端口，这时要确保宿主机器上的端口号没有被使用。**EXPOSE指令可以一次设置多个端口号**，相应的运行容器的时候，可以配套的**多次使用-p选项**。

**例如： 映射一个端口**
**EXPOSE port1**
#相应的运行容器使用的命令
**docker run -p port1 image**
**例如： 映射多个端口**
**EXPOSE port1 port2 port3**
#相应的运行容器使用的命令
**docker run -p port1 -p port2 -p port3 image**
#还可以指定需要映射到宿主机器上的某个端口号
**docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image**
端口映射是docker比较重要的一个功能，原因在于我们每次运行容器的时候容器的IP地址不能指定而是在桥接网卡的地址范围内随机生成的。宿主机器的IP地址是固定的，我们可以将容器的端口的映射到宿主机器上的一个端口，免去每次访问容器中的某个服务时都要查看容器的IP的地址。对于一个运行的容器，可以使用docker port加上容器中需要映射的端口和容器的ID来查看该端口号在宿主机器上的映射端口。



### （8）ENV（用于设置环境变量）

**构建指令，指定一个环境变量，会被后续RUN指令使用，并在容器运行时保持。**
**格式:**
**ENV <key> <value>**
设置了后，后续的RUN命令都可以使用，container启动后，可以通过docker inspect查看这个环境变量，也可以通过在docker run --env key=value时设置或修改环境变量。
假如你安装了JAVA程序，需要设置JAVA_HOME，那么可以在Dockerfile中这样写：
**ENV JAVA_HOME /path/to/java/dirent**
再例如：
ENV PG_MAJOR 9.3
ENV PG_VERSION 9.3.4
RUN curl http://example.com/postgres-$PG_VERSION.tar.xz | tar -xJC
/usr/src/postgress
ENV PATH /usr/local/postgres-$PG_MAJOR/bin:$PATH



### （9）ADD（将源文件复制到container的目标文件）

构建指令，所有拷贝到container中的文件和文件夹权限为0755，uid和gid为0；
**源文件要与Dockerfile位于相同目录中；**
1、如果源路径是个文件，且目标路径是以 / 结尾， 则docker会把目标路径当作一个目录，会把源文件拷贝到该目录下。**如果目标路径不存在，则会自动创建目标路径。**
2、如果源路径是个文件，且目标路径是不是以 / 结尾，则docker会把目标路径当作一个文件。如果目标路径不存在，会以目标路径为名创建一个文件，内容同源文件；如果目标文件是个存在的文件，会用源文件覆盖它，当然只是内容覆盖，文件名还是目标文件名。如果目标文件实际是个存在的目录，则会源文件拷贝到该目录下。 注意，这种情况下，最好显示的以 / 结尾，以避免混淆。
3、如果源路径是个目录，且**目标路径不存在**，则**docker会自动以目标路径创建一个目录**，把源路径目录下的文件拷贝进来。如果目标路径是个已经存在的目录，则docker会把源路径目录下的文件拷贝到该目录下。
4、**如果源文件是个归档文件（压缩文件），则docker会自动帮解压**。
格式:
ADD <src> <dest>
该命令将复制指定的<src>到容器中的<dest>。
其中<src>可以是Dockerfile所在目录的一个相对路径；也可以是一个 URL；还可以是一个tar 文件（自动解压为目录）
<dest>是container中的绝对路径
**例如：**
#test
FROM ubuntu
MAINTAINER hello
ADD test1.txt test1.txt
ADD test1.txt test1.txt.bak
ADD test1.txt /mydir/
ADD data1 data1
ADD data2 data2
ADD zip.tar /myzip



### （10）COPY

**格式为 COPY <src><dest>**
**复制本地主机的<src>（为Dockerfile所在目录的相对路径）到容器中的<dest>。**
源文件/目录要与Dockerfile**在相同的目录中**
COPY指令和ADD指令功能和使用方式类似。只是**COPY指令不会做自动解压工作**。



### （11）VOLUME（指定挂载点）

设置指令，使容器中的一个目录具有持久化存储数据的功能，该目录可以被容器本身使用，也可以共享给其他容器使用。我们知道容器使用的是AUFS，这种文件系统不能持久化数据，当容器关闭后，所有的更改都会丢失。当容器中的应用有持久化数据的需求时可以在Dockerfile中使用该指令。
格式:
VOLUME ["<mountpoint>"]
例如：FROM base
VOLUME ["/tmp/data"]
运行通过该Dockerfile生成image的容器，/tmp/data目录中的数据在容器关闭后，里面的数据还存在。例如另一个容器也有持久化数据的需求，且想使用上面容器共享的/tmp/data目录，那么可以运行下面的命令启动一个容器：
docker run -t -i -rm -volumes-from container1 image2 bash
container1为第一个容器的ID，image2为第二个容器运行image的名字。



### （12）WORKDIR(切换目录)

**设置指令，可以多次切换(相当于cd命令)，对RUN,CMD,ENTRYPOINT生效。为后续的RUN、CMD、ENTRYPOINT 指令配置工作目录。**
**格式:**
WORKDIR /path/to/workdir
例如： 在 /p1/p2 下执行 vim a.txt
WORKDIR /p1
WORKDIR p2
RUN vim a.txt
可以使用多个WORKDIR指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径。
例如
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
则最终路径为/a/b/c。



### （13）ONBUILD（在子镜像中执行）

**ONBUILD <Dockerfile关键字>**
**ONBUILD 指定的命令在构建镜像时并不执行，而是在它的子镜像中执行。**
**格式为：**
ONBUILD [INSTRUCTION] 。
配置当所创建的镜像作为其它新创建镜像的基础镜像时，所执行的操作指令。
例如，Dockerfile使用如下的内容创建了镜像image-A 。
[...]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[...]
如果基于 image-A 创建新的镜像时，新的Dockerfile中使用FROM image-A 指定基础镜像时，会自动执行ONBUILD 指令内容。
等价于在后面添加了两条指令。
FROM image-A
#Automatically run the following
ADD . /app/src
RUN /usr/local/bin/python-build --dir /app/src
使用ONBUILD指令的镜像，推荐在标签中注明，例如ruby:1.9-onbuild。
编写完成Dockerfile之后，可以通过docker build 命令来创建镜像。
基本的格式为docker build [选项] 路径，该命令将读取指定路径下的Dockerfile，并将该路径下所有内容发送给Docker 服务端，由服务端来创建镜像。因此一般建议放置
Dockerfile的目录为空目录。
**要指定镜像的标签信息，可以通过-t选项，例如**
$ sudo docker build –t myrepo /myapp/tmp/test1/
docker应用案例：使用dockerfile创建sshd镜像模板并提供http访问应用



# 编写Dockerfile文件

### **2、编写Dockerfile文件，内容如下：**

```shell
# my dockerfile
FROM busybox
MAINTAINER zh_2511@163.com
WORKDIR /testdir
RUN touch tmpfile1
COPY ["tmpfile2", "."]
ADD ["bunch.tar.gz", "."]
ENV WELCOME "You are in my container,welcome!"
```

注：Dockerfile 支持以 “#” 开头的注释

### 构建镜像，如下图所示

```shell
[root@docker ~]# ll
total 222140
-rw-r--r--. 1 root root       145 Dec 18 22:40 bunch.tar.gz    @ 1
-rw-r--r--. 1 root root       183 Dec 18 22:39 Dockerfile
  w-r--r--. 1 root root        15 Dec 18 22:41 tmpfile2 
[root@docker ~]# docker build -t my-image . 		@ 2
Sending build context to Docker daemon  227.5MB
Step 1/7 : FROM busybox
latest: Pulling from library/busybox
322973677ef5: Pull complete 
Digest: sha256:1828edd60c5efd34b2bf5dd3282ec0cc04d47b2ff9caa0b6d4f07a21d1c08084
Status: Downloaded newer image for busybox:latest
 ---> b534869c81f0
Step 2/7 : MAINTAINER zh@163.com
 ---> Running in a2495cfa5fb4
Removing intermediate container a2495cfa5fb4
 ---> 530c023698b3
Step 3/7 : WORKDIR /testdir
 ---> Running in 2f4b01aa7caa
Removing intermediate container 2f4b01aa7caa
 ---> 907b3cea706e
Step 4/7 : RUN touch tmpfile1
 ---> Running in 0a74adb53250
Removing intermediate container 0a74adb53250
 ---> 7f57370373fe
Step 5/7 : COPY ["tmpfile2", "."]
 ---> 121b9ba3106a
Step 6/7 : ADD ["bunch.tar.gz", "."]
 ---> 80dcc5d8c1a8
Step 7/7 : ENV WELCOME "You are in my container,welcome!"
 ---> Running in f3253ed2ec80
Removing intermediate container f3253ed2ec80
 ---> a69d3d84cce6
Successfully built a69d3d84cce6
Successfully tagged my-image:latest
```

①   构建前确保build context 中存在需要的文件。

②   依次执行 Dockerfile 指令， 完成构建。



### 运行容器，验证镜像内容，如下所示

```shell
[root@docker ~]# docker run  -it my-image
/testdir # ls      				@1
bunch     tmpfile1  tmpfile2     @2

/testdir # echo $WELCOME         @3
You are in my container,welcome!
```

①   进入容器，当前目录即为WORKDIR。

如果WORKDIR不存在，Docker会自动为我们创建。

②   WORKDIR 中保存了我们希望的文件和目录：

目录  bunch: 由ADD指令从 build context 复制的归档文件 bunch.tar.gz, 已经自动解压。

文件 tmpfile1: 由RUN指令创建。

文件tmpfile2: 由COPY指令从 build context 复制。

③   ENV指令定义的环境变量已经生效。











































