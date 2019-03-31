# 操作hadoop
## hadoop命令
### 1.ls
```
hadoop fs -ls /
列出目录下所有内容
hadoop fs -ls -R /
```
### 2.mkdir
```
一次创建一级目录
hadoop fs -mkdir /tmp
一次创建多级目录
hadoop fs -mkdir -p /tmp2/tmp3/tmp4
```
### 3.put或者copyFromLocal
```
上传目录或者文件
hadoop fs -put somefileOrsomeDir /tmp
hadoop fs -copyFromLocal somefileOrsomeDir /tmp
```
copyFromLocal与put效果类似，但是moveFromLocal会在上传后删除本地文件或者目录
```
hadoop fs -moveFromLocal somefileOrsomeDir /tmp
```
### 4.get或者copyToLocal
```
下载目录或者文件
hadoop fs -get /tmp/somefileOrsomeDir
hadoop fs -copyToLocal /tmp/somefileOrsomeDir
```
### 5.rm
```
hadoop fs -rm /tmp
一次删除多个文件或者目录
hadoop fs -rm -r /tmp
```
### 6.cp
在hdfs中copy文件或者目录
```
hadoop fs -cp /user/test.txt /user/test1.txt
```
### 7.mv
在hdfs中移动文件或者目录
```
hadoop fs -mv /user/test.txt /user/test1.txt
```
### 8.count
统计hdfs对应路径下的目录个数，文件个数，文件总计大小
显示为目录个数，文件个数，文件总计大小，输入路径
```
hadoop fs -count < hdfs path >
```
### 9.du和df
```
hadoop fs -du < hdsf path> 
显示hdfs对应路径下每个文件夹和文件的大小

hadoop fs -du -s < hdsf path> 
显示hdfs对应路径下所有文件和的大小

hadoop fs -du - h < hdsf path> 
显示hdfs对应路径下每个文件夹和文件的大小,文件的大小用方便阅读的形式表示，例如用64M代替67108864
```
df用来显示hdfs文件系统的总容量，已用容量和可用容量
```
hadoop fs -df -h
```
### 10.text和cat
输出hdfs中文件的文本
```
hadoop fs -text /user/test.txt
hadoop fs -cat /user/test.txt
```
### 11.getmerge
将hdfs中某个目录下的文件排序、合并，写入指定的本地文件
```
hadoop fs -getmerge < hdfs dir >  < local file >
hadoop fs -getmerge -nl  < hdfs dir >  < local file >
```
-nl参数表示每个文件之间空一行
### 12.setrep
改变一个文件在hdfs中的副本个数，上述命令中数字3为所设置的副本个数，-R选项可以对一个人目录下的所有目录+文件递归执行改变副本个数的操作
```
hadoop fs -setrep -R 3 < hdfs path >
```
### 13.stat
统计信息
```
hadoop fs -stat [format] <path> ...]
```
其中format可选参数有：%b（文件大小），%o（Block大小），%n（文件名），%r（副本个数），%y（最后一次修改日期和时间）
例如
```
hadoop fs -stat %b%n%r%y /user/test.txt
```
### 14.tail
输出某个文件末尾的1KB数据
```
hadoop fs -tail < hdfs file >
```
### 15.snapshot
允许某目录创建快照
```
hdfs dfsadmin -allowSnapshot <path>
```
创建快照
```
hadoop fs -createSnapshot <snapshotDir> [<snapshotName>]
```
删除快照
```
hadoop fs -deleteSnapshot <snapshotDir> <snapshotName>]
```
