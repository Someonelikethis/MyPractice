# Linux常用命令

[TOC]

## 压缩解压

### tar

```shell
压缩
tar -cvf xxx.tar aaa bbb ccc.log
解压到当前目录
tar -xvf xxx.tar
解压到指定目录
tar -xvf xxx.tar -C /path/to/
调用gzip
tar -zcvf file.tar.gz aaa bbb
tar -zxvf file.tar.gz

v 显示过程
f 指定文件名
c 将多个文件或目录打包
x 对tar包进行解压操作
z 调用gzip
```

### zip

```shell
打包多个文件
zip target_file.zip file1 file2

打包文件夹
zip -r target_file.zip forder1
 -r 表示将文件夹中的所有文件和子文件夹都压缩进压缩包。
 
 解压到当前目录
 unzip target_file.zip
 
 解压到指定目录
 unzip target_file.zip -d /path/to/dir
```

## 查看网络情况

```shell
hostname -I
ip address
ifconfig
ifconfig -a
ifconfig eth0
其中eth0网卡为本地网卡
```

## 查看操作系统版本

```shell
cat /etc/redhat-release
cat /etc/centos-release
hostnamectl
cat /proc/version
uname -a
```

## 创建文件夹-mkdir

```shell
创建多个目录
mkdir a b c
创建多级目录
mkdir -p a/b/c
```

## 查看文件夹大小-du

```shell
查询当前目录总大小
du -sh
查看当前目录下各文件、文件夹的大小
du -h --max-depth=1
du -sh *
```

## 统计文件夹

```shell
统计当前目录下文件的个数（不包括目录）
ls -l | grep "^-" | wc -l
统计当前目录下文件的个数（包括子目录）
ls -lR| grep "^-" | wc -l
查看某目录下文件夹(目录)的个数（包括子目录）
ls -lR | grep "^d" | wc -l
```

**命令解析：**

- `ls -l`

长列表输出该目录下文件信息(注意这里的文件是指目录、链接、设备文件等)，每一行对应一个文件或目录，`ls -lR`是列出所有文件，包括子目录。

- `grep "^-"`
  过滤`ls`的输出信息，只保留一般文件，只保留目录是`grep "^d"`。
- `wc -l`
  统计输出信息的行数，统计结果就是输出信息的行数，一行信息对应一个文件，所以就是文件的个数。

## 文件传输

sftp、scp、ftp

### sftp

```shell
sftp常用指令
默认端口是22
连接服务器
sftp -P 22 root@127.0.0.1
上传
mkdir aaa
put -r aaa
下载
get -r aaa
查看远程服务器内容
ls
查看本地服务器内容
lls
执行本地shell命令
![command]
```

### scp

```shell
从本地主机传输文件到远程主机
scp [本地文件路径] [用户名]@[远程主机IP地址]:[目标路径]

从远程主机传输文件到本地主机
scp [用户名]@[远程主机IP地址]:[远程文件路径] [本地目标路径]

 -P 22指定端口
 -r 指定目录
```

## 查看端口占用-netstat

```shell
查看全部端口占用
netstat -tunlp

查看指定端口占用
netstat -tunlp|grep 8080

根据进程id查看进程信息
ps -ef|grep [pid]

统计系统中每种 TCP 连接状态的数量，并以状态和数量的形式输出结果。
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
LISTEN: 服务器正在监听端口，等待连接。
ESTABLISHED: 已建立连接，正常的数据传输。
TIME_WAIT: 连接已关闭，但还在等待确保对端接收到 ACK。
CLOSE_WAIT: 对方已关闭连接，但本地还未释放资源。
SYN_SENT: 本地已发送 SYN 请求，等待对方回应。
FIN_WAIT1/FIN_WAIT2: 本地主动关闭连接，等待对方确认。
CLOSED: 无连接。
```

## 查看系统所有用户

```shell
more /etc/passwd
```

## curl

```shell
// 下载文件
curl url -O
// 下载文件指定名称
curl url -o text.txt
// 忽略证书
curl url --insecure
curl url -k
// 指定方法
curl url -X POST
// 添加请求头
curl url -X POST -H "Accept:application/json" -H "Content-Type:application/json"
// 添加请求体
curl url -X POST -H "Accept:application/json" -H "Content-Type:application/json" -d ' {
"password": "xxx",
"username": "xxx"
}'
// 设置代理
curl url -x http://proxy-server:port
注：对于复杂url需要添加""
// 指定账号密码
curl -u username:password -X GET "http://localhost:9200/"
```

## 执行多条shell命令

| 分隔符 | 命令示例       | 执行逻辑                                             |
| ------ | -------------- | ---------------------------------------------------- |
| ;      | cmd1 ; cmd2    | 不管前面的命令是否执行成功，都会执行后面的命令。     |
| &&     | cmd1 && cmd2   | 只有在前面的命令执行成功后，才会接着执行后面的命令。 |
| \|\|   | cmd1 \|\| cmd2 | 只有在前面的命令执行失败后，才会接着执行后面的命令。 |

## 磁盘分区与挂载

```shell
查看磁盘情况
lsblk
fdisk -l
df -h
特定分区创建文件系统
mkfs -t ext4 /dev/sdb
挂载分区
mount /dev/sdb /data
解除挂载
umount /dev/sdb
设置开机启动命令
vi /etc/rc.d/rc.local
chmod 755 /etc/rc.d/rc.local

/etc/fstab
```

## 查看cpu信息-lscpu

```shell
lscpu

Architecture:        # 架构
CPU op-mode(s):      # CPU 运行模式
Byte Order:          # 字节序
CPU(s):              # 逻辑CPU颗数
On-line CPU(s) list: # 在线CPU列表
Thread(s) per core:  # 每个核的线程数
Core(s) per socket:  # 每个CPU插槽核数/每颗物理CPU核数
CPU socket(s):       # CPU插槽数
NUMA node(s):        # NUMA节点
Vendor ID:           # CPU厂商ID
CPU family:          # CPU系列
Model:               # 型号
Model name:          # 型号名称
Stepping:            # 步进
CPU MHz:             # CPU主频
CPU max MHz:         # CPU最大主频
CPU min MHz:         # CPU最小主频
Virtualization:      # CPU支持的虚拟化技术
L1d cache:           # 一级缓存（CPU的L1数据缓存）
L1i cache:           # 一级缓存（CPU的L1指令缓存）
L2 cache:            # 二级缓存
```

## 查看cpu核心数

```
cat /proc/cpuinfo |grep processor

nproc
```

## 实时系统监控工具-top

```shell
top
```

## 用户和组

### 添加用户账号

```shell
useradd -m <username> -s /bin/bash
-s指定shell工具，默认是/bin/sh，不支持tab补全等
如果是已建用户修改到/bin/bash，可以编辑/etc/passwd修改
```

### 修改用户账号

```shell
passwd <username>
```

### 查询用户和组信息

```
id <username>
groups <username>
用户的默认组和用户名相同
```

### 添加用户到指定组

```shell
usermod -aG <goroupname> <username>
```

### 解锁用户密码锁定

```shell
sudo pam_tally2 --reset --user <username>
```

## chown

修改文件/目录的所有权

```
chown -R <username>:<groupname> /path
-R递归目录
```

## chmod

```
修改文件/文件夹权限
-R递归目录
700表示只有所有者可以读/写/执行，其他用户无权限
750表示所有者可以读/写/执行，组用户可以读取和执行
755表示所有者可以读/写/执行，组用户和其他用户可以读取和执行
777表示所有用户都可以读/写/执行
```



## 目录树生成工具-tree

```shell
tree -L 2
```

## 查看系统时间

```shell
date
date -R
hwclock
```

## 包管理工具

### apk

alpine

```shell
apk -I list 列出已安装
```

### yum

centos



### rpm

rhel、centos



### apt

ubuntu、debian

```shell
# 更新所有已安装的软件包
$ apt-get upgrade
# 更新
$ apt-get update
# 安装一个新软件包
$ apt-get install packagename
# 卸载一个已安装的软件包（保留配置文件）
$ apt-get remove packagename
# 卸载一个已安装的软件包（删除配置文件）
$ apt-get –purge remove packagename
# 列出已安装软件包
$ sudo apt list --installed
```



### dpkg

debian系

```shell
# 列出当前已安装的包
dpkg-query -l
# 安装包
$ dpkg -i package.deb
# 删除包
$ dpkg -r package
# 删除包（包括配置文件）
$ dpkg -P package
# 显示该包的版本
$ dpkg -l package
# 搜索所属的包内容
$ dpkg -S keyword
```

## 软连接

```shell
# 读取链接地址
readlink -f ds-example.conf
删除
unlink ds-example.conf
添加软连接
ln -s /usr/bin/python2.7 /usr/bin/python
```

## 防火墙

查看防火墙状态

```shell
systemctl status firewalld
```

启动防火墙

```shell
systemctl start firewalld
```

重启防火墙

```shell
service firewalld restart
systemctl restart firewalld
```

关闭防火墙

```shell
systemctl stop firewalld
```

开机禁用

```shell
systemctl disable firewalld
```

开机启动

```shell
systemctl enable firewalld
```

重新加载配置

```shell
firewall-cmd --reload
```

查看已开放端口

```shell
firewall-cmd --list-ports
```

查看所有策略

```shell
firewall-cmd --list-all
```

开放端口

```shell
开放指定端口
firewall-cmd --zone=public --permanent --add-port=9080/tcp --add-port=8090/tcp

批量开放80到90之间的所有端口
firewall-cmd --zone=public --add-port=80-90/tcp --permanent

允许单个ip访问80端口
firewall-cmd --permanent --remove-rich-rule="rule family='ipv4' source address='192.168.0.200' port protocol='tcp' port='80' accept"

批量允许多ip访问80端口
firewall-cmd --permanent --remove-rich-rule="rule family='ipv4' source address='192.168.3.0/24' port protocol='tcp' port='80' accept"
```

移除端口

```shell
firewall-cmd --zone=public --permanent --remove-port=9080/tcp
```

通过修改文件来控制

```shell
/etc/firewalld/zones/public.xml

<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="ssh"/>
  <service name="dhcpv6-client"/>
  <port protocol="tcp" port="10022"/>
  <rule family="ipv4">
    <source address="10.78.2.81"/>
    <port protocol="tcp" port="9000"/>
    <accept/>
  </rule>
  <rule family="ipv4">
    <source address="10.78.2.82"/>
    <port protocol="tcp" port="9000"/>
    <accept/>
  </rule>
</zone>
```

## iptables

```shell
查看指定chain
iptables -L INPUT
注意顺序由上往下，优先级从高到低

插入规则到指定行
iptables -I INPUT 1 -s 192.19.0.0/24 -p tcp --dport 3306 -j ACCEPT
-I表示插入，-A表示追加（到末尾），-D表示删除
INPUT是chain名称，表示插入或追加到哪个chain
插入时，1表示插入到第一行
-s指定source（源地址），可以指定ip或网段，192.19.0.0/24是网段
-p指定protocol，指定通信协议，tcp、udp等
--dport指定端口
ACCEPT表示接收数据包，DROP表示丢弃数据包
```

## 查看软件路径

```shell
which python
```

## 查看显卡占用

```shell
nvidia-smi
```

## ssh免密登录

1. **主控机生成 SSH 密钥对**：如果您还没有 SSH 密钥对，请使用以下命令生成一对新的密钥。在生成密钥对时，如果提示您输入密码，请留空，直接按回车键。

   ```shell
   ssh-keygen -t rsa -b 4096
   ```

2. **将公钥添加到目标主机的授权密钥列表**：将您生成的公钥 `~/.ssh/id_rsa.pub` 添加到目标主机的 `~/.ssh/authorized_keys` 文件中。您可以使用 `ssh-copy-id` 命令将公钥复制到目标主机上：

   ```shell
   ssh-copy-id username@hostname
   -v 查看详细信息
   ```

   或者手动将公钥内容添加到目标主机的 `~/.ssh/authorized_keys` 文件中。

3. **测试免密登录**：现在，您应该可以通过以下命令无需输入密码登录到目标主机：

   ```shell
   ssh username@hostname
   ```

请确保目标主机上的 `~/.ssh/authorized_keys` 文件具有正确的权限（应为 `600` 或 `644`）以及目录 `~/.ssh` 的权限为 `700`。

## 定时任务

```shell
# 查看任务
crontab -l
# 编辑当前用户crontab文件
crontab -e
# 查看执行任务
cd /var/log/
less cron
```

## 查看系统资源使用情况

### `ulimit -a`

```
core file size          (blocks, -c) 0
data seg size           (kbytes, -d) unlimited
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited
pending signals                 (-i) 123861
max locked memory       (kbytes, -l) unlimited
max memory size         (kbytes, -m) unlimited
open files                      (-n) 1048576
pipe size            (512 bytes, -p) 8
POSIX message queues     (bytes, -q) 819200
real-time priority              (-r) 0
stack size              (kbytes, -s) 8192000
cpu time               (seconds, -t) unlimited
max user processes              (-u) 1048576
virtual memory          (kbytes, -v) unlimited
file locks                      (-x) unlimited
```

**core file size (-c)**: 核心转储文件的大小限制，当前设置为 `0`，意味着不生成核心转储文件。

**data seg size (-d)**: 进程数据段的最大大小，当前设置为 `unlimited`，表示没有限制。

**scheduling priority (-e)**: 调度优先级的限制，当前设置为 `0`，这通常不受限制。

**file size (-f)**: 文件大小的限制，当前设置为 `unlimited`，没有限制。

**pending signals (-i)**: 等待的信号数的限制，当前设置为 `123861`。

**max locked memory (-l)**: 最大锁定内存的大小，当前设置为 `unlimited`，没有限制。

**max memory size (-m)**: 最大内存大小，当前设置为 `unlimited`，没有限制。

**open files (-n)**: 每个进程最大可打开的文件描述符数，当前设置为 `1048576`。

**pipe size (-p)**: 管道的大小，当前设置为 `8` 字节。

**POSIX message queues (-q)**: POSIX 消息队列的最大大小，当前设置为 `819200` 字节。

**real-time priority (-r)**: 实时优先级的限制，当前设置为 `0`。

**stack size (-s)**: 每个线程的栈大小，当前设置为 `8192000` 字节（约 8 MB）。

**cpu time (-t)**: 进程的 CPU 时间限制，当前设置为 `unlimited`，没有限制。

**max user processes (-u)**: 用户进程的最大数量，当前设置为 `1048576`。

**virtual memory (-v)**: 虚拟内存的限制，当前设置为 `unlimited`，没有限制。

**file locks (-x)**: 文件锁的数量限制，当前设置为 `unlimited`，没有限制。

#### 对于线程创建的相关参数

1. **max user processes (-u)**: 这个限制影响可以创建的最大线程数。因为线程是由进程创建的，所以你不能创建超过这个限制的线程。
2. **open files (-n)**: 这个限制影响了每个进程可以打开的最大文件数。线程通常共享文件描述符，所以这个限制也间接影响了线程的创建。

### `free`

```
$ free -m
              total        used        free      shared  buff/cache   available
Mem:          30985       23464         470          22        7051        7109
Swap:             0           0           0
```

#### `Mem` 行

- **total**: 30985 MB 表示系统中总共的物理内存。
- **used**: 23464 MB 表示目前已使用的物理内存，这包括了应用程序和操作系统使用的内存。
- **free**: 470 MB 表示未使用的物理内存。
- **shared**: 22 MB 表示用于 tmpfs 文件系统的共享内存。
- **buff/cache**: 7051 MB 表示操作系统用于文件系统缓冲区和缓存的内存。
- **available**: 7109 MB 表示实际可供新启动的应用程序使用的内存量。这比 free 更准确，因为它考虑了可以回收的缓冲区和缓存。

#### `Swap` 行

- **total**: 0 MB 表示系统中没有配置交换空间。
- **used**: 0 MB 表示没有使用任何交换空间。
- **free**: 0 MB 表示没有空闲的交换空间

## 查看ssl证书信息

```
openssl x509 -in cacert.pem -noout -text
```

## 查看文件md5

```
md5sum md5
```

## vim

### 清空文件

```
:%d
```

### 显示行号

```
:set-number
```

## 常用命令

删除系统中60天之前的所有exe文件

```shell
find /path/to/directory -type f -name "*.exe" -mtime +60 -exec rm -f {} \;
```

