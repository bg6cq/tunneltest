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


其他测试端，通过若干出口与科大侧隧道端点S建立隧道，测试连通性，并简单测试性能。

测试信息如下（密码单独发送）：

### 2.1 测试端1 信息

| 网络入口 |  服务器端IP地址  |  隧道UDP端口  |   隧道IP地址段  | 
| :-----   | :--------------- | :------- |  :------   |
| 教育网   |  222.195.81.202  | 6011  |  100.64.11.* |
| 电信     |  218.22.21.27    | 6012  |  100.64.12.* |
| 联通     |  218.104.71.165  | 6013  |  100.64.13.* |
| 移动     |  202.141.176.27  | 6014  |  100.64.14.* |
| 科技网   |  210.72.22.2     | 6015  |  100.64.15.* |
| IPv6     |  2001:da8:d800:381::202  | 6016 |   100.64.16.* |


### 2.2 测试端2 信息

| 网络入口 |  服务器端IP地址  |  隧道UDP端口  |   隧道IP地址段  |  
| :-----   | :--------------- | :------- |  :------   |
| 教育网   |  222.195.81.202  | 6021  |  100.64.21.* |
| 电信     |  218.22.21.27    | 6022  |  100.64.22.* |
| 联通     |  218.104.71.165  | 6023  |  100.64.23.* |
| 移动     |  202.141.176.27  | 6024  |  100.64.24.* |
| 科技网   |  210.72.22.2     | 6025  |  100.64.25.* |
| IPv6     |  2001:da8:d800:381::202  | 6026 |   100.64.26.* |

### 2.3 测试端3 信息

| 网络入口 |  服务器端IP地址  |  隧道UDP端口  |   隧道IP地址段  |
| :-----   | :--------------- | :------- |  :------   |
| 教育网   |  222.195.81.202  | 6031  |  100.64.31.* |
| 电信     |  218.22.21.27    | 6032  |  100.64.32.* |
| 联通     |  218.104.71.165  | 6033  |  100.64.33.* |
| 移动     |  202.141.176.27  | 6034  |  100.64.34.* |
| 科技网   |  210.72.22.2     | 6035  |  100.64.35.* |
| IPv6     |  2001:da8:d800:381::202  | 6036 |   100.64.36.* |

### 2.4 测试端4 信息

| 网络入口 |  服务器端IP地址  |  隧道UDP端口  |   隧道IP地址段  |
| :-----   | :--------------- | :------- |  :------   |
| 教育网   |  222.195.81.202  | 6041  |  100.64.41.* |
| 电信     |  218.22.21.27    | 6042  |  100.64.42.* |
| 联通     |  218.104.71.165  | 6043  |  100.64.43.* |
| 移动     |  202.141.176.27  | 6044  |  100.64.44.* |
| 科技网   |  210.72.22.2     | 6045  |  100.64.45.* |
| IPv6     |  2001:da8:d800:381::202  | 6046 |   100.64.46.* |


### 2.5 测试端5 信息

| 网络入口 |  服务器端IP地址  |  隧道UDP端口  |   隧道IP地址段  |
| :-----   | :--------------- | :------- |  :------   |
| 教育网   |  222.195.81.202  | 6051  |  100.64.51.* |
| 电信     |  218.22.21.27    | 6052  |  100.64.52.* |
| 联通     |  218.104.71.165  | 6053  |  100.64.53.* |
| 移动     |  202.141.176.27  | 6054  |  100.64.54.* |
| 科技网   |  210.72.22.2     | 6055  |  100.64.55.* |
| IPv6     |  2001:da8:d800:381::202  | 6056 |   100.64.56.* |

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
```
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
```

3.3 隧道测试

请下载测试结果文件，用于记录测试过程: [tunneltest.docx](tunneltest.docx)

这里有一份测试结果供参考：[tunneltest-sample.docx](tunneltest-sample.docx)

根据"二、测试环境"中信息，对服务器的6个入口分别测试（如果没有IPv6环境，可以跳过IPv6的测试）：

以测试点1、服务器端教育网入口测试为例，测试过程如下：

以下命令中，`P=PASSWORD` `INDEX=1`请依据测试站点的不同修改。
```

P=PASSWORD
INDEX=1

killall -9 EthUDP
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0.0.0.0 60${INDEX}1 222.195.81.202 60${INDEX}1 100.64.${INDEX}1.2 24
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0.0.0.0 60${INDEX}2 218.22.21.27 60${INDEX}2 100.64.${INDEX}2.2 24
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0.0.0.0 60${INDEX}3 218.104.71.165 60${INDEX}3 100.64.${INDEX}3.2 24
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0.0.0.0 60${INDEX}4 202.141.176.27 60${INDEX}4 100.64.${INDEX}4.2 24
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0.0.0.0 60${INDEX}5 210.72.22.2 60${INDEX}5 100.64.${INDEX}5.2 24
/usr/src/ethudp/EthUDP -i -p $P -enc aes-128 -k $P 0::0 60${INDEX}6 2001:da8:d800:381::202 60${INDEX}6 100.64.${INDEX}6.2 24

#测试各个线路的ping延迟，请记录平均值，单位是ms，有效数字两位即可

echo CERNET ping
ping -c 5 222.195.81.202
echo CERNET tunnel ping
ping -c 5 100.64.${INDEX}1.1
read -p "press enter to continue"

echo CT ping
ping -c 5 218.22.21.27
echo CT tunnel ping
ping -c 5 100.64.${INDEX}2.1
read -p "press enter to continue"

echo CU ping
ping -c 5 218.104.71.165
echo CU tunnel ping
ping -c 5 100.64.${INDEX}3.1
read -p "press enter to continue"

echo CM ping
ping -c 5 202.141.176.27
echo CM tunnel ping
ping -c 5 100.64.${INDEX}4.1
read -p "press enter to continue"

echo CSTNET ping
ping -c 5 210.72.22.2
echo CSTNET tunnel ping
ping -c 5 100.64.${INDEX}5.1
read -p "press enter to continue"

echo IPv6 ping
ping6 -c 5 2001:da8:d800:381::202
echo IPv6 tunnel ping
ping -c 5 100.64.${INDEX}6.1
read -p "press enter to continue"


#测试各个线路的wget速度，请记录最后结果，单位是MB/s，有效数字两位即可
echo CERNET wget speed
wget -O /dev/null http://222.195.81.202/test.iso
read -p "press enter to continue"
echo CERNET tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}1.1/test.iso
read -p "press enter to continue"

echo CT wget speed
wget -O /dev/null http://218.22.21.27/test.iso
read -p "press enter to continue"
echo CT tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}2.1/test.iso
read -p "press enter to continue"

echo CU wget speed
wget -O /dev/null http://218.104.71.165/test.iso
read -p "press enter to continue"
echo CU tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}3.1/test.iso
read -p "press enter to continue"

echo CM wget speed
wget -O /dev/null http://202.141.176.27/test.iso
read -p "press enter to continue"
echo CM tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}4.1/test.iso
read -p "press enter to continue"

echo CSTNET wget speed
wget -O /dev/null http://210.72.22.2/test.iso
read -p "press enter to continue"
echo CSTNET tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}5.1/test.iso
read -p "press enter to continue"

echo IPv6 wget speed
wget -O /dev/null http://[2001:da8:d800:381::202]/test.iso
read -p "press enter to continue"
echo IPv6 tunnel wget speed
wget -O /dev/null http://100.64.${INDEX}6.1/test.iso
```
