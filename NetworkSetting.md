# setting centos7 network
在虚拟机中配置Centos7网络，设置固定IP的方法如下：

## 1.修改网络配置文件
```
cd /etc/sysconfig/network-scripts
vim ifcfg-eno33
```
对其进行编辑，最终配置文件内容如下：

```
DEVICE=eth1
HWADDR=00:0C:29:90:5D:8C（这个可以在网络适配器查看）
TYPE=Ethernet
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=static
BROADCAST=192.168.20.255（前三位要和主机的ip地址一致，后一位为255）
DNS1=202.101.172.35
IPADDR=192.168.20.140（虚拟机的IP地址，前三位与主机的一致）
NETMASK=255.255.255.0
GATEWAY=192.168.20.1（主机的默认网关地址）
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=yes
```
## 2.重启网络服务
```
service network restart
```
## 3.测试
用shell直接ping前面设置的固定IP即可知道配置是否生效。
