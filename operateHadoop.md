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

```
### 10.text
```

```
### 11.getmerge
```

```
### 12.setrep
```

```
### 13.stat
```

```
### 14.tail
```

```
### 15.snapshot
```

```

