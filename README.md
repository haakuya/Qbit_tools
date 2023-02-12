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

	例：C:\Users\你的用户名\Desktop\ut2gt.exe  D:\uTorrent\resume.dat  C:\qBittorrent Stable (build 4.1.0.0)\qBittorrent\profile\qBittorrent\data\BT_backup
	然后按下回车等就OK了，会自动备份一次 resume.dat ，不信任的也可以手动备份。
	
原帖地址  (Migrate from uTorrent to qBittorrent easily)[https://qbforums.shiki.hu/index.php/topic,3224.0.html]

```C:\Users\你的用户名\Desktop\ut2gt.exe  D:\uTorrent\resume.dat  C:\qBittorrent Stable (build 4.1.0.0)\qBittorrent\profile\qBittorrent\data\BT_backup```
