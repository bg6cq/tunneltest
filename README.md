## 一、测试目的

测试隧道的可达性、隧道性能简单测试。

## 二、测试环境

科大侧提供了隧道端点S，在教育网、电信、联通、移动、科技网、IPv6等6个入口提供隧道连接。

| 网络接口   |   网络入口          |    IP地址  |
| :------- | :----------------- | :-------- |
| eth0  |  222.195.81.202 | 教育网   |
| eth1  |  218.22.21.27   | 电信  |
| eth2  |  218.104.71.165   | 联通  |
| eth3  |  202.141.176.27   | 移动  |
| eth4  |  210.72.22.2   | 科技网  |
| eth0  |  2001:da8:d800:381::202   | 教育网IPv6 |


=其他测试端，通过若干出口与科大侧隧道端点S建立隧道，测试连通性，并简单测试性能。

测试信息如下（密码单独发送）：

218.22.21.27
210.72.22.2 

## 三、测试步骤

每个测试点需要一台Linux机器，物理机器或虚拟机均可。

以下测试以CentOS 6.10为例演示，其他系统可能需略加修改。

3.1 系统安装

下载Centos6.10最小安装ISO，安装即可。

安装后请配置好网络，最简单的方式是修改文件`/etc/rc.d/rc.local`。增加类似内容：
```
ip link set eth0 up
ip addr add x.x.x.x/24 dev eth0
ip route add 0/0 via x.x.x.z
```
其中x.x.x.x是IP地址，x.x.x.z是网关。
并修改文件`/etc/resolv.conf`，增加
```
nameserver y.y.y.y
···
其中y.y.y.y是DNS服务器IP。

3.2 更新系统并安装相关软件

执行如下命令
```
yum install -y epel-release
yum update -y
yum install -y gcc git lz4-devel openssl-devel tcpdump ntpdate telnet bind-utils traceroute wget
cd /usr/src/
git clone https://github.com/bg6cq/ethudp.git
cd ethudp
make
···


