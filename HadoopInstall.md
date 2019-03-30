# hadoop installation
安装配置单节点Hadoop
## 1.检测所需的软件是否安装
```
rpm -qa|grep java
rpm -qa|grep ssh
rpm -qa|grep rsync
```
目前要求jdk版本1.7以上即可，最好是1.8。

## 2.下载解压hadoop最新稳定版
```
cd /opt
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.1.2/hadoop-3.1.2.tar.gz
tar xzvf hadoop-3.1.2.tar.gz
```
## 3.设置jdk
由于centos自带的openjdk只有jre，因此还要安装openjdk-devel版本
检测软件名称，然后安装
```
yum list | grep openjdk-devel
yum install java-1.8.0-openjdk-devel.x86_64
```

设置java home，检查jdk所在目录，建立软链接
```
rpm -qa|grep openjdk
java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64

rpm -ql java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64

mkdir /usr/java
ln -s /usr/lib/jvm/java-1.8.0 /usr/java/latest
```
至此将/usr/java/latest指向最新的jdk目录。

修改/etc/profile配置文件，在末尾添加
```
export JAVA_HOME=/usr/java/latest
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
使其立刻生效
```
source /etc/profile
```
## 4.修改hostname
使用命令将hostname该为cent1
```
hostnamectl set-hostname cent1
```
在/etc/hosts文件末尾加上
```
cent1 youipaddr
```
在/etc/sysconfig/network加上
```
NETWORKING=yes  
HOSTNAME=cent1 
```

## 5.配置伪分布hadoop
### 修改hadoop-env.sh

修改hadoop配置文件：etc/hadoop/hadoop-env.sh
>
>  # The java implementation to use. By default, this environment
>  # variable is REQUIRED on ALL platforms except OS X!
>  export JAVA_HOME=/usr/java/latest

测试hadoop：
```
cd /opt/hadoop-3.1.2
./bin/hadoop
```
正常输出hadoop命令帮助

### 修改各配置文件
core-site.xml:加入以下配置

```
<configuration>
## 配置hdfs
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://cent1:9000</value>
</property>
## 配置hadoop目录
<property>
  <name>hadoop.tmp.dir</name>
  <value>/opt/hadoop-3.1.2/data/</value>
</property>
</configuration>

```

hdfs-site.xml：配置hdfs的副本数
```
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
```

配置 mapred-site.xml:指定MapReduce程序应该放在哪个资源调度集群上运行。若不指定为yarn，那么MapReduce程序就只会在本地运行而非在整个集群中运行
```
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
```

配置 yarn-site.xml:要配置的参数有2个：

1.指定yarn集群中的老大（就是本机）
```
<property>
<name>yarn.resourcemanager.hostname</name>
<value>cent1</value>
</property>
```
2.配置yarn集群中的重节点，指定map产生的中间结果传递给reduce采用的机制是shuffle
```
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
```

### 配置环境变量
vi /etc/profile
```
export HADOOP_HOME=/opt/hadoop-3.1.2
export PATH=$PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```
然后记得 source /etc/profile
现在就可以在任意地方使用hadoop指令了。

### 关闭防火墙
查看防火墙状态
firewall-cmd --state
关闭防火墙
systemctl stop firewalld.service
systemctl disable firewalld.service

### 配置ssh免密登录
1.创建ssh公钥/私钥，输入以下命令一路回车
ssh-keygen -t rsa

2.创建authorized_keys文件并修改权限为600
cd ~/.ssh
touch authorized_keys
chmod 600 authorized_keys

3.将公钥追加到authorized_keys文件中去
cat id_rsa.pub >> authorized_keys

4.测试是否能够ssh登录自己的主机cent1
ssh cent1

## 6.操作hadoop
### 6.1格式化
hadoop namenode -format
输出中有下列信息表示成功
```
 Storage directory /opt/hadoop-3.1.2/data/dfs/name has been successfully formatted.
```

### 6.2 修改启动脚本
直接执行start-all.sh 观察是否报错，如报错执行一下内容

$ vim sbin/start-dfs.sh
$ vim sbin/stop-dfs.sh

在空白位置加入
```
HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```

$ vim sbin/start-yarn.sh 
$ vim sbin/stop-yarn.sh 

在空白位置加入
```
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
```

### 6.3启动
start-all.sh
使用jps查看：输出如下内容表示启动成功
```
42384 Jps
41315 SecondaryNameNode
41891 NodeManager
41013 DataNode
40733 NameNode
41678 ResourceManager
```


