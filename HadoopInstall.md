# hadoop installation
安装配置单节点Hadoop
## 1.检测所需的软件是否安装
```
rpm -qa|grep jave
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
## 3.设置
设置java home，检查jdk所在目录，建立软链接
```
rpm -qa|grep jdk
java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64

rpm -ql java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64/jre/bin/policytool
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64/jre/lib/amd64/libawt_xawt.so
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64/jre/lib/amd64/libjawt.so
/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64/jre/lib/amd64/libjsoundalsa.so

mkdir /usr/java
ln -s /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.181-7.b13.el7.x86_64/ /usr/java/latest
```
至此将/usr/java/latest指向最新的jdk目录。

修改/etc/profile配置文件，在末尾添加
```
export JAVA_HOME=/usr/java/latest
export CLASSPATH=.:${JAVA_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
使其立刻生效
```
source /etc/profile
```

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

