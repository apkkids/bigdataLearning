# yum setting
## 配置阿里云yum源
1、打开centos的yum文件夹
```
cd  /etc/yum.repos.d/
```
2、用wget下载repo文件

输入命令
```
wget  http://mirrors.aliyun.com/repo/Centos-7.repo
```
如果wget命令不生效，说明还没有安装wget工具，输入yum -y install wget 回车进行安装。

当前目录是/etc/yum.repos.d/，刚刚下载的Centos-7.repo也在这个目录上

3、备份系统原来的repo文件
```
mv  CentOs-Base.repo CentOs-Base.repo.bak
```
即是重命名 CentOs-Base.repo -> CentOs-Base.repo.bak

4、替换系统原理的repo文件
```
mv Centos-7.repo CentOs-Base.repo
```
即是重命名 Centos-7.repo -> CentOs-Base.repo

5、执行yum源更新命令
```
yum clean all
yum makecache
yum update
```
依次执行上述三条命令即配置完毕。

## yum相关命令
### 1.yum常用命令
1.使用YUM查找软件包 
```
命令：yum search 
```
2.列出所有可安装的软件包 
```
命令：yum list 
```
3.列出所有可更新的软件包 
```
命令：yum list updates 
```
4.列出所有已安装的软件包 
```
命令：yum list installed 
```
5.列出所有已安装但不在 Yum Repository 内的软件包 
```
命令：yum list extras 
```
6.列出所指定的软件包 
```
命令：yum list 
```
7.使用YUM获取软件包信息 
```
命令：yum info 
```
8.列出所有软件包的信息 
```
命令：yum info 
```
9.列出所有可更新的软件包信息 
```
命令：yum info updates 
```
10.列出所有已安装的软件包信息 
```
命令：yum info installed 
```
11.列出所有已安装但不在 Yum Repository 内的软件包信息 
```
命令：yum info extras 
```
12.列出软件包提供哪些文件 
```
命令：yum provides
```
### 2.yum检测某软件是否安装
1、首先安装一个redis
```
yum install redis
```
2、查找redis的安装包
```
rpm -qa|grep redis
redis-3.2.10-2.el7.x86_64
```
3、查找安装包的安装路径
```
rpm -ql redis-3.2.10-2.el7.x86_64
/etc/logrotate.d/redis
/etc/redis-sentinel.conf
/etc/redis.conf
/etc/systemd/system/redis-sentinel.service.d
/etc/systemd/system/redis-sentinel.service.d/limit.conf
/etc/systemd/system/redis.service.d
/etc/systemd/system/redis.service.d/limit.conf
/usr/bin/redis-benchmark
/usr/bin/redis-check-aof
/usr/bin/redis-check-rdb
/usr/bin/redis-cli
```
