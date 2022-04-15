# Linux

网络查看

```shell
apt-get install iftop
iftop
```

## 命令

| 说明   |命令   |
| ----------- | ----------- |
|查找看命令简介|type ls|
|查找看内置命令详情|help cd|
|查找看其他命令详情|man ls|

## 软件更新与安装

### 系统源

| 说明   |命令   |
| ----------- | ----------- |
| 更新软件列表 | apt-get update |
| 更新软件 | apt-get upgrade |
| 搜索软件 | search |
| 卸载软件 | autoremove

### 编译安装

* 查看编译信息

    ```shell
    ./configure --help | more
    ```

* 编译

    ```shell
    ./configure --prefix=/opt/xxx 
    ```

* 安装

    ```shell
    make && make install
    ```

### 软件常见命令

#### nginx

| 说明   |命令   |
| ----------- | ----------- |
|启动|nginx|
|安全结束|nginx -s quit|
|强制杀掉进程|nginx -s stop|
|查看端口|ps aux|grep nginx|
|重新加载配置文件| nginx -s reload |

#### php

master进程可以理解以下信号

INT, TERM 立刻终止
QUIT 平滑终止
USR1 重新打开日志文件
USR2 平滑重载所有worker进程并重新载入配置和二进制模块

| 说明   |命令   |
| ----------- | ----------- |
| 启动 | php-fpm |
| 查看进程 |  ```ps auxfww \| grep php \| grep -v grep``` |
| 重启php-fpm | kill -USR2 13225 |

```nginx
 fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
```

## 环境配置

更新环境

```shell
source /etc/profile
```

## 权限

修改目录下所有权限
sudo chmod 777 /目录/ -R
修改目录下所有用户组
 chown www:www www -R

## 用户管理

```shell
# 创建xxx用户
useradd xxx
# 修改xxx用户的密码
passwd xxx
```

## 文件

从当前目录下到家目录 cd 或者 cd ~
|     说明    |     命令    |
| ----------- | ----------- |
|创建目录 | mkdir |
|进入目录     |cd folder|
|返回上一级   |cd ..|
|返回用户目录   |cd|
|查找当前目录下文件     |find -name node|
|查找文件夹     |whereis node|
|当前目录文件列表 | ls -l|
|当前目录文件详细列表 | ls -sarln|
|文件详细 | file package.xml|
|复制 | cp|
|剪切 | mv|
|快捷键 | ln|

|     说明    |     命令    |
| ----------- | ----------- |
|查阅正在改变的日志文件 | tail -f a.log |
|查阅日志文件 | cat |
|查阅最后一行日志 | tac |

## 其他

|     说明    |     命令    |
| ----------- | ----------- |
|清理shel     |clear(或者<kbd>Ctrl</kbd>+<kbd>L</kbd>)|
|关机 | shutdown -h now |
|重启 | reboot [-n] [-w] [-d] [-f] [-i] |

## 用户

|     说明    |     命令    |
| ----------- | ----------- |
|当前在线用户 |who|
|我是谁 |whoami|

## 系统

|     说明    |     命令    |
| ----------- | ----------- |
|内核信息 |uname -a|
|输出 |echo $PATH|
|历史命令 |history|

## 查看硬盘

```shell
df
```

## 查看路径

```shell
pwd
```

## 防火墙

```shell
iptables -L
```

创建
[root@git-node1 ~]#useradd nulige
[root@git-node1 ~]#passwd nulige
让git用户可以执行shell命令
usermod -s /bin/bash git

# screen 多重视窗管理

|     说明    |     命令    |
| ----------- | ----------- |
|创建会话 |screen -S session_name|
|查看所有会话 |screen -ls|
|进入会话 |screen -r session_name|
|关闭会话 | exit |
|离开会话并不关闭 | <kbd>Ctrl</kbd> + <kbd>a</kbd> 然后再按 <kbd>d</kbd> |

# 磁盘扩容

## 扩大已有MBR分区

以“CentOS 7.4 64bit”操作系统为例，系统盘“/dev/vda”原有容量40GB，只有一个分区“/dev/vda1”。将系统盘容量扩大至100GB，本示例将新增的60GB划分至已有的MBR分区内“/dev/vda1”内。

1. 执行以下命令，安装growpart扩容工具。

```
yum install cloud-utils-growpart
```

> 说明：
可以用growpart命令检查当前系统是否已安装growpart扩容工具，若回显为工具使用介绍，则表示已安装，无需重复安装。

2. 执行以下命令，查看系统盘“/dev/vda”的总容量。

```
fdisk -l
```

回显类似如下信息：

```
[root@ecs-test-0001 ~]# fdisk -l


Disk /dev/vda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000bcb4e

Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048    83886079    41942016   83  Linux
```

3. 执行以下命令，查看系统盘分区“/dev/vda1”的容量。

```
df -TH
```

回显类似如下信息：

```
[root@ecs-test-0001 ~]# df -TH
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/vda1      ext4       43G  2.0G   39G   5% /
devtmpfs       devtmpfs  2.0G     0  2.0G   0% /dev
tmpfs          tmpfs     2.0G     0  2.0G   0% /dev/shm
tmpfs          tmpfs     2.0G  9.0M  2.0G   1% /run
tmpfs          tmpfs     2.0G     0  2.0G   0% /sys/fs/cgroup
tmpfs          tmpfs     398M     0  398M   0% /run/user/0
```

4. 执行以下命令，指定系统盘待扩容的分区，通过growpart进行扩容。
growpart 系统盘 分区编号

命令示例：

```
growpart /dev/vda 1
```

回显类似如下信息：

```
[root@ecs-test-0001 ~]# growpart /dev/vda 1
CHANGED: partition=1 start=2048 old: size=83884032 end=83886080 new: size=209713119,end=209715167
```

5. 执行以下命令，扩展磁盘分区文件系统的大小。
resize2fs 磁盘分区

命令示例：

```
resize2fs /dev/vda1
```

回显类似如下信息：

```
[root@ecs-test-0001 ~]# resize2fs /dev/vda1
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vda1 is mounted on /; on-line resizing required
old_desc_blocks = 5, new_desc_blocks = 13
The filesystem on /dev/vda1 is now 26214139 blocks long.
```

6. 执行以下命令，查看扩容后系统盘分区“/dev/vda1”的容量。

```
df -TH
```

回显类似如下信息：

```shell
[root@ecs-test-0001 ~]# df -TH
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/vda1      ext4      106G  2.0G   99G   2% /
devtmpfs       devtmpfs  2.0G     0  2.0G   0% /dev
tmpfs          tmpfs     2.0G     0  2.0G   0% /dev/shm
tmpfs          tmpfs     2.0G  9.0M  2.0G   1% /run
tmpfs          tmpfs     2.0G     0  2.0G   0% /sys/fs/cgroup
tmpfs          tmpfs     398M     0  398M   0% /run/user/0
```

## 新增MBR分区

系统盘“/dev/vda”原有容量40GB，只有一个分区“/dev/vda1”。将系统盘容量扩大至80GB，本示例为新增的40GB分配新的MBR分区“/dev/vda2”。

执行以下命令，查看磁盘的分区信息。

```
fdisk -l
```

回显类似如下信息：

```
[root@ecs-2220 ~]# fdisk -l

Disk /dev/vda: 85.9 GB, 85899345920 bytes, 167772160 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0008d18f

Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048    83886079    41942016   83  Linux
```

表示当前系统盘“dev/vda”容量为80 GB，当前正在使用的分区“dev/vda1”为40 GB，新扩容的40 GB还未分配分区。

执行如下命令之后，进入fdisk分区工具。

```
fdisk /dev/vda
```

回显类似如下信息：

```
[root@ecs-2220 ~]# fdisk /dev/vda
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help):
```

输入“n”，按“Enter”，开始新建分区。
回显类似如下信息：

```
Command (m for help): n
Partition type:
p   primary (1 primary, 0 extended, 3 free)
e   extended
```

表示磁盘有两种分区类型：
“p”表示主分区。
“e”表示扩展分区。
说明：
磁盘使用MBR分区形式，最多可以创建4个主分区，或者3个主分区加1个扩展分区，扩展分区不可以直接使用，需要划分成若干个逻辑分区才可以使用。

磁盘使用GPT分区形式时，没有主分区、扩展分区以及逻辑分区之分。

以创建一个主要分区为例，输入“p”，按“Enter”，开始创建一个主分区。
回显类似如下信息：

```
Select (default p): p
Partition number (2-4, default 2):
```

以分区编号选择“2”为例，输入分区编号“2”，按“Enter”。
回显类似如下信息：

```
Partition number (2-4, default 2): 2
First sector (83886080-167772159, default 83886080):
```

输入新分区的起始磁柱值，以使用默认起始磁柱值为例，按“Enter”。
系统会自动提示分区可用空间的起始磁柱值和截止磁柱值，可以在该区间内自定义，或者使用默认值。起始磁柱值必须小于分区的截止磁柱值。

回显类似如下信息：

```
First sector (83886080-167772159, default 83886080):
Using default value 83886080
Last sector, +sectors or +size{K,M,G} (83886080-167772159,default 167772159):
```

输入新分区的截止磁柱值，以使用默认截止磁柱值为例，按“Enter”。
系统会自动提示分区可用空间的起始磁柱值和截止磁柱值，可以在该区间内自定义，或者使用默认值。起始磁柱值必须小于分区的截止磁柱值。

回显类似如下信息：

```
Last sector, +sectors or +size{K,M,G} (83886080-167772159,
default 167772159):
Using default value 167772159
Partition 2 of type Linux and of size 40 GiB is set

Command (m for help):
```

输入“p”，按“Enter”，查看新建分区。
回显类似如下信息：

```
Command (m for help): p

Disk /dev/vda: 85.9 GB, 85899345920 bytes, 167772160 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x0008d18f

Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048    83886079    41942016   83  Linux
/dev/vda2        83886080   167772159    41943040   83  Linux
Command (m for help):
```

输入“w”，按“Enter”，将分区结果写入分区表中。
回显类似如下信息：

```
Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```

表示分区创建完成。

说明：
如果之前分区操作有误，请输入“q”，则会退出fdisk分区工具，之前的分区结果将不会被保留。

执行以下命令，将新的分区表变更同步至操作系统。
partprobe

执行以下命令，设置新建分区文件系统格式。
mkfs -t 文件系统 磁盘分区

ext*文件系统命令示例：
以“ext4” 文件格式为例：

```
mkfs -t ext4 /dev/vda2
```

回显类似如下信息：

```
[root@ecs-2220 ~]# mkfs -t ext4 /dev/vda2
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
2621440 inodes, 10485760 blocks
524288 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2157969408
320 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
4096000, 7962624

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

xfs文件系统命令示例：

```
mkfs -t xfs /dev/vda2
```

回显类似如下信息：

```
[root@ecs-2220 ~]# mkfs -t xfs /dev/vda2
meta-data=/dev/vda2              isize=512     agcount=4, agsize=2621440 blks
=                       sectsz=512    attr=2, projid32bit=1
=                       crc=1         finobt=0, sparse=0
data     =                       bsize=4096    blocks=10485760, imaxpct=25
=                       sunit=0       swidth=0 blks
naming   =version2               bsize=4096    ascii-ci=0 ftype=1
log      =internal log           bsize=4096    blocks=5120, version=2
=                       sectsz=512    sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096    blocks=0, rtextents=0
```

格式化需要等待一段时间，请观察系统运行状态，若回显中进程提示为done，则表示格式化完成。

（可选）执行以下命令，新建挂载目录。
若需要挂载至新建目录下，执行该操作。

mkdir 挂载目录

以新建挂载目录“/opt”为例：

```
mkdir /opt
```

执行以下命令，挂载新建分区。
mount 磁盘分区 挂载目录

以挂载新建分区“/dev/vda2”至“/opt”为例：

```
mount /dev/vda2 /opt
```

说明：
新增加的分区挂载到不为空的目录时，该目录下原本的子目录和文件会被隐藏，所以，新增的分区最好挂载到空目录或者新建目录。如确实要挂载到不为空的目录，可将该目录下的子目录和文件临时移动到其他目录下，新分区挂载成功后，再将子目录和文件移动回来。

执行以下命令，查看挂载结果。

```
df -TH
```

回显类似如下信息：

```
[root@ecs-2220 ~]# df -TH
Filesystem     Type      Size  Used Avail Use% Mounted on
/dev/vda1      ext4       43G  2.0G   39G   5% /
devtmpfs       devtmpfs  509M     0  509M   0% /dev
tmpfs          tmpfs     520M     0  520M   0% /dev/shm
tmpfs          tmpfs     520M  7.2M  513M   2% /run
tmpfs          tmpfs     520M     0  520M   0% /sys/fs/cgroup
tmpfs          tmpfs     104M     0  104M   0% /run/user/0
/dev/vda2      ext4       43G   51M   40G   1% /opt
```

说明：
云服务器重启后，挂载会失效。您可以修改“/etc/fstab”文件，将新建磁盘分区设置为开机自动挂载，请参见设置开机自动挂载磁盘分区。
