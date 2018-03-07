# 计划任务索引共享文件夹，提升 Everything 启动速度

Everything 软件可以快速的索引本地文件。快速的原因是因为索引是用的 NTFS 文件系统中的 USN 日志，不需要逐个文件夹遍历。

但是如果索引的文件夹是 samba 协议的共享文件夹，索引方式就变成逐个文件夹遍历。因此一旦要索引的文件夹、文件夹的内容很多，索引的时间就会很漫长，要等好久好久~~~~

这样的索引过程发生在 Everything 启动、更新配置生效时，于是，就有这样的尴尬：
- 启动 Everything ，等待好久才能搜索
- 更改配置，点确定， Everything 卡死

## 分析
一般情况下，大家对共享文件夹的实时性要求并不高，是可以将索引过程和搜索过程异步的。

Everything 有个设置功能，设置定时更新。但是然并卵，软件启动时还是要索引，要等很久。
![尝试监控文件夹变化](http://osivibnve.bkt.clouddn.com/office/180307/GEBdF6DgKi.png?imageslim)

还有一个[启动时禁止 Everything 扫描文件夹](https://www.voidtools.com/zh-cn/support/everything/folder_indexing/#如何添加网络映射分区到_everything_索引？)的设置项，可以设置软件启动时不索引文件，但是我尝试了，好像不起作用。
![mark](http://osivibnve.bkt.clouddn.com/office/180307/8e670e9DFD.png?imageslim)

## 如何解决？
阅读手册并尝试后发现， Everything 加载 efu 格式的文件列表速度非常快。
因此，可以通过文件列表作为缓存，计划任务定期更新文件列表，不再索引文件夹。


### 步骤

>- 编写单次索引指定文件夹的 bat 脚本
>- 将脚本加入计划任务
>- 配置 Everything ，载入指定 efu



- **编写单次索引指定文件夹的 bat 脚本**

```powershell
@echo off
chcp 65001
D:
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\1.efu" "\\192.168.1.1\1"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\2.efu" "\\192.168.1.1\2"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\3.efu" "\\192.168.1.1\3"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\4.efu" "\\192.168.1.1\4"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\5.efu" "\\192.168.1.1\5"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\6.efu" "\\192.168.1.1\6"
"C:\Program Files\Everything\Everything.exe" -create-filelist "D:\everything-efu\7.efu" "\\192.168.1.1\7"
@echo on
```


- **将脚本加入计划任务**

![将脚本加入计划任务](http://osivibnve.bkt.clouddn.com/office/180307/I03Kg5hddj.png?imageslim)



- **配置 Everything ，载入指定 efu**

![mark](http://osivibnve.bkt.clouddn.com/office/180307/kBi1ijmBKF.png?imageslim)