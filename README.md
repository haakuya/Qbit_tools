# FixQb_Back_Reload_exe
qb迁移跳检

### 工作环境：程序需要在Windows下执行，qbittorrent可以在本地可以在远程，开启webui即可。备份的种子文件需要和程序在一个设备下。

### 适用场景：
      1、简单备份迁移至新机器或新的客户端
      2、降级后需要重新检验（踩坑4.2.1降4.1.9）

### 工作原理：
场景1、qbittorrent开启webui，根据客户端信息获取种子hash值，name，保存路径，分类等信息，存入csv；获取默认备份路径下的种子文件，如 hash.torrent。
	替换qbittorrent部署环境或降级后，重新根据csv和torrent文件进行hash比对，根据webui提供的api添加至客户端。
注：备份目录为BT_backup，不同的设备位置不一样，本程序需要把所有种子文件download到本地。

场景2、已经降级的人，不需要更换客户端，而且已经有一部分检验完成的，只需要部分跳检的。将需要跳检的种子分类设置为“备份”，根据客户端获取符合该分类的种子信息记录hash和保存位置后，自行删除客户端该分类需要跳检的种子（此时备份目录的种子也会删除，所以需要提前拷贝到其他位置），根据拷贝的种子和提取的信息比对添加至客户端。

场景1、执行第一步根据客户端信息生成csv。4.2.1版本下22个种子

将备份种子下的文件拷贝至程序所在电脑任意处，降级qbittorrent：(迁移设备需要硬盘转移后与csv记录位置一致)
 
降级之后，开启webui并执行重加载（需要再次执行程序第2步）：（csv文件最好处于关闭状态）
 
现在假设不更换客户端的情况下有的种子需要跳检（已经踩坑的情况下）：将其中部分种子分类设置为备份：
 
执行程序选项3：
 
删除对应的种子：
 
更新后是处于暂停状态，确认无误后开始并更改分类为其他选项。

# usage of ut2gt.exe
原帖https://bbs.itzmx.com/forum.php?mod=viewthread&tid=86731
	由于utorrent是个垃圾软件，各种内存溢出崩溃，恶意弹窗广告推送，但是由于历史遗留问题，还有上千个种子任务存在utorrent中，这里快速把他进行迁移

	qBittorrent 是开源软件，简洁易用无广告，而且跨平台 Linux, Mac OS X, Windows都能用。
	选项-高级中可以关闭更新检测，不会像 UT 强制更新，而且网络性能强劲，可以自主设置缓冲区大小。

	qBittorrent 论坛有人写了个小工具，可以简单快速的将种子从 uTorrent 迁移到 qBittorrent 。
	首先当然是暂停 uTorrent 中的所有种子，确认全停了之后，然后退出uTorrent，qB 也要关闭。

	下载ut2gt.exe（地址在下面），然后打开CMD，先将程序拖到命令行窗口。
	会自动填写路径 例：  C:\Users\你的用户名\Desktop\ut2gt.exe
	然后一个空格，将 uTorrent 的 resume.dat 拖入窗口。
	再接一个空格，将 qBittorrent 的 BT_backup 文件夹拖入窗口。
	绿色版路径 C:\qBittorrent Stable (build 4.1.0.0)\qBittorrent\profile\qBittorrent\data\BT_backup
	
```C:\Users\你的用户名\Desktop\ut2gt.exe  D:\uTorrent\resume.dat  C:\qBittorrent Stable (build 4.1.0.0)\qBittorrent\profile\qBittorrent\data\BT_backup```

	然后按下回车等就OK了，会自动备份一次 resume.dat ，不信任的也可以手动备份。

# Qbittorrent 性能参数校准
本文的目的在于让 Qbittorrent 实现最大化的上传下载速度，无论有没有公网IPv4。

硬件配置
不说硬件配置就说软件优化的都是流氓.png

主机：NUC10i5FNH(Intel i5-10210U (8) @ 4.200GHz)
内存：32G DDR4 3200
主数据盘：HC550 16t
操作系统：Debian 11
Qbittorrent版本
https://johnrosen1.com/2022/04/09/qbt/qbt1.png 
此版本为我自己编译的版本，并没有修改源代码，只是指定了Libtorrent 1.2。

本文写作时Libtorrent 2.0有严重的内存泄露问题，因此使用1.2版本。

参数含义及校准建议
## 连接设置
	下载连接协议 TCP 

	用于传入连接的端口：11920 随机
	使用我的路由器的UPnP／NAT-PMP功能来转发端口

## 加密设置
上面三项建议全部启用，PT、BT会自动管理，不会互相干扰。
加密模式建议设置为强制加密以规避ISP QOS以及被动流量监听。
缺点在于与极其老旧的BT客户端连线会失败。

	启用DHT（去中心化网络）以找到更多用户启用用户交换（PeX）以找到更多用户

	启用本地用户发现以找到更多用户

	加密模式：强制加密 

## Trackers设置
最大并发 HTTP 发布建议值为30,这样Trackers更新不会停滞或消耗过多资源。
总是向同级的所有 Tracker 汇报需开启，否则Trackres会一直处于未联系状态。
Trackers List推荐https://trackerslist.com/best.txt
best.txt相比all.txt，连接超时的Trackers更少，可以节省网络资源。

	当IP或端口更改时，重新通知所有 trackers：
	总是向同级的所有Tracker汇报：
	总是向所有等级的Tracker汇报：
	最大并发HTTP发布（需要libtorrent＞＝1.2.7）：30 
	停止tracker超时：0 
	启用内置 Tracker：
	内置 tracker 端口：9000 

## 网络设置
网络接口建议设置为外网接口加所有地址，以免浪费网络资源。
	
	网络接口：eno1
	绑定到的可选 IP 地址：所有地址

与 peers 连接的服务类型（ToS）建议设置为0以减少流量特征。
	
	每秒传出连接数：200000 
	Socket backlog 大小：200000 
	传出端口（下限）［0：禁用］：0 
	传出端口 （上限） ［0：禁用］：0 
	UPnP 租期 ［0：永久］：0 
	与peers连接的服务类型（ToS）0
	μTP—TCP 混合模式策略：优先使用TCP 

## 安全设置
服务器端请求伪造（SSRF）攻击缓解与禁止连接到特权端口上的 Peer选项建议开启，防止连接到1024端口以下的peer，可有效防止DDOS与DHT投毒。
	
	支持国际化域名（IDN）（需要libtorrent＞＝1.2.12）：
	允许来自同—IP地址的多个连接：
	验证 HTTPS tracker 证书：
	服务器端请求伪造（SSRF）攻击缓解：
	禁止连接到特权端口上的Peer：

## 缓存设置
磁盘缓存是必需的，因为机械硬盘的IO有限。

异步 I/O 线程数填4倍CPU线程数，来源https://github.com/qbittorrent/qBittorrent/issues/11461
文件池大小建议设置为100000，这个设置等于Linux Nofile limit。
磁盘缓存建议设置为-1，让程序自己管理。
磁盘缓存过期时间间隔建议设置为15，即最快刷新。
	
	异步I／O线程数：32 
	散列线程（需要libtorrent＞＝2.0）：2 
	文件池大小：100000 
	校验时内存使用扩增量：256 MiB
	磁盘缓存（需要 libtorrent＜2.0） ： -1 MiB
	磁盘缓存过期时间间隔（要求libtorrent＜2.0）：15 秒

## 连接限制
Qbittorrent默认有连接数限制，主机性能足够可以考虑全部关闭。

测试是否生效：建议进行对比测试，就能知道是否生效了。本文不作相关论述。


原文https://johnrosen1.com/2022/04/09/qbt/
