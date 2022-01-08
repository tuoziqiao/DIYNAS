# DIYNAS  不完全装机作业


[TOC]



# 💎 配置单

| 类型     | 型号              | 备注                                                         | 价格 | 渠道 |
| -------- | ----------------- | ------------------------------------------------------------ | ---- | ---- |
| 处理器   | i5-10500ES QSRK   | 十代测试不显版，6核12线程，主频3.0G，全3.7G。备选QSRL        | 720  | 某鱼 |
| 主板     | 华擎B460Mpro4     | 6个SATA盘位 24.4x22.9cm（备选：华硕的TUF GAMING B460M-PLUS重炮手） | 569  | 某东 |
| 机箱     | N8B               | 8盘位NAS机箱，厂家都差不多                                   | 780  | 某宝 |
| 内存条   | 8G                | 2400、2666频率就够了                                         | 179  | 某东 |
| CPU散热  | IS-50X            | 矮风扇                                                       | 109  | 某宝 |
| 电源     | 益衡 7025B        | 小机箱1U电源，双12V输出，最少也能支持8个硬盘同时启动。15x8x4cm | 245  | 某宝 |
| 千兆网卡 | I350-T4           | 【可选】选择寨卡。做软路由                                   |      |      |
| SATA卡   | 乐扩的SATA3       | 【可选】只需要X1的PCIE就可以增加4个盘位                      |      |      |
| 万兆网卡 | MCX311            | 【可选】                                                     |      |      |
| U盘      | 闪迪酷豆 CZ33 16G | USB2.0。安装黑群晖，UNRAID                                   | 30   | 某宝 |

❄ 总价  **2450** 左右 ![手动狗头](https://gitee.com/yokogood/YokoImages/raw/master/img/%E6%89%8B%E5%8A%A8%E7%8B%97%E5%A4%B4.png)



# 💎 安装黑群晖



## 🔍 制作黑群晖引导U盘

```
一、先用分区大师，给U盘格式化

二、再用芯片无忧工具，查询U盘 PID 和 VID 序列号

三、再用镜像写入工具，将引导安装到U盘上

四、最后在分区大师中，修改U盘引导文件中的PID VID，就安装完成了
```

#### 1、分区大师

选中U盘，直接点击上面的快速分区，修改分区表为MBR，一个分区，然后点击确定，完成后， <font color="red">关掉分区大师工具</font>  

![image-20220107141130733](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107141130733.png)

#### 2、芯片无忧

打开芯片无忧工具，选择到闪迪U盘，可以看到设备ID的地方 ， <font color='red'> 记住U盘的   VID 、 PID</font>

```
设备ID    :  VID = 0781     PID = 5571
```

![image-20220107141948947](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107141948947.png)

#### 3、镜像写入

<font color="red">关闭芯片无忧、分区大师工具</font>，然后再打开 镜像写入工具
选择映象文件，找到 **DS918+_6.23-25423-1.04b补丁齐全半洗白.img**  引导镜像，然后设备选择U盘的盘符，点击写入。等待完成，关闭写入工具

如果系统弹出要格式化U盘，不要格式化

![image-20220107142815092](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107142815092.png)

#### 4、分区大师

U盘写入完成后，再次打开分区大师工具。
可以看到U盘内已经多了一些文件了，点击ESP，再点击grub文件夹
在右边，点击 gurb.cfg文件，然后点击右键，复制文件到桌面

![image-20220107144253233](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107144253233.png)

在桌面右键用记事本打开这个文件，然后将其中的 vid 和 pid 修改成我们用芯片无忧工具获取的数值

<font color='red'> 不要删掉 0x </font>，保存文件

![image-20220107144550781](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107144550781.png)

然后将这个改了vid 和 pid 的文件，拖入到分区大师，相同的目录下，点击替换。

![image-20220107144640465](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107144640465.png)



搞定！![哇哦](https://gitee.com/yokogood/YokoImages/raw/master/img/%E5%93%87%E5%93%A6.png)   

当然  某宝上买一个引导盘也行![微笑](https://gitee.com/yokogood/YokoImages/raw/master/img/%E5%BE%AE%E7%AC%91.png)



##  🔍 安装黑群晖系统



#### 1、设置U盘引导的优先级

将制作好的U盘插入到NAS上。接上 电源线、显示器、键盘鼠标，以及网线

先插入一块硬盘，这个硬盘最好是格式化过的干净硬盘

刚开始安装，建议先安装一个硬盘，后续系统一切正常后，再安装剩下的硬盘

开机，按Delete键，进入到主板的BIOS，（不同主板进入方法不一样，按各自的主板快捷键进入）

确认我们的U盘引导的优先级是最高级，然后按F10保存退出（按各自主板保存快捷键）

机器进行重启

当显示器，显示下面这个信息的时候，就代表我们的黑群晖已经安装好了！

![image-20220105145531619](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220105145531619.png)

这个时候，就可以拔掉显示器和键盘鼠标了

以后这台DIY的NAS，只需要一个U盘，一根网线接在上面即可



#### 2、搜索NAS

我们在自己局域网的PC台式机上，下载群晖的 Synology Assistant 工具，打开后，可以看到局域网中的NAS信息

Synology Assistant 是群晖的官方查找工具，百度一下，群晖的官网即可下载

也可以通过路由器的后台IP地址，查看最新的IP地址，找到这台NAS

![image-20220107225110561](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107225110561.png)

#### 3、手动安装

我们在浏览器输入刚刚查找到的黑群晖IP地址：192.168.50.2

就可以进入了群晖的安装界面了，我们点击设置按钮

<font color='red'> 不要点击立即安装，一定要点击手动安装</font>

![image-20220107231514389](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107231514389.png)

点击浏览，选择  **DSM_DS918+_25423.pat** 这个系统文件，立即安装

安装大概在10分钟以内即可完成

#### 4、特殊情况

目前这个引导文件，特别适合于 7 、 8 、 9 代 处理器使用，比如i3-8100、 i5-9500、 G5400等处理器。

<font color='red'>一直处于 95%无进展 </font>

解决方案：格式化U盘，重新制作引导

<font color='red'>掉IP···· </font>

![image-20220105150604972](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220105150604972.png)

出现这种情况，大多是U盘引导文件中的网卡驱动不正确导致。

这个情况，也只有10代处理器，或者部分**i219v的主板网卡**会出现这个情况。大部分主板都没这个问题

解决方案：格式化U盘，重新制作引导，引导选择 DS918+_6.23-25423-1.04b（引导文件）加扩展驱动.img 这个加扩展驱动的引导，然后安装后，就成功进入了系统

#### 5、安装完成

安装好后，设置一下服务器的名称、用户、密码

然后，设置一下更新通知，点下一步。<font color='red'>一定不要选择到自动安装DSM的更新</font>，选择手动安装

然后在QC外网访问页面的时候，<font color='red'>点击最下面的跳过</font>

![image-20220107232253587](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107232253587.png)

当出现这个页面的时候，就是DSM系统了

![image-20220107234053148](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107234053148.png)

我们的黑群晖系统，正式安装完成！！！

可以愉快的玩耍了![微笑](https://gitee.com/yokogood/YokoImages/raw/master/img/%E5%BE%AE%E7%AC%91.png)



## 🔍 设置黑群晖系统



#### 1、关闭 自动安装新更新

首先，在控制面板，**更新和还原** 页面，找到更新设置，改成通知更新，不要点击自动更新、

以后只要我们不升级系统，这台NAS就可以完美使用

![image-20220107234210410](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107234210410.png)

#### 2、关机 填装其他硬盘

在关机状态，把其它硬盘安装进去，再启动。

![image-20220107234354844](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107234354844.png)

如果SATA背板支持热插拔可以不用关机，不过建议关机安装

#### 3、设置 RAID 阵列

打开存储空间管理员

![image-20220105152922403](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220105152922403.png)

点击存储空间，新增，建立RAID 阵列存储池

![image-20220105152756407](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220105152756407.png)



考虑到以后硬盘会增多，而且容量可能会比较杂，所以直接图方便，建立了SHR阵列

SHR 是群晖独创的阵列，和RAID5类似。这个阵列，是有一个容错硬盘，当硬盘损坏一块的时候，数据也不会丢失，相对来说还是比较安全的

选择所有的硬盘后，一直点击下一步即可

SHR 阵列，在扩容方面也比较方便，如果硬盘只有2-3块，组SHR阵列后，可以随时添加新硬盘来增加容量

选择SHR，还有一个好处，是可以增加存储空间的大文件读写速度，和RAID 5 一样，属于又安全，又有速度提升的一种阵列模式

组了阵列以后，是会进行校验的，这个校验过程很久，而且会占用CPU性能

建议等校验完成后，我们再进行其它操作

<font color="red">非常不建议用RAID 0 阵列</font> ，而其它的阵列都可以尝试一下！

其中，RAID1 最安全，但是需要2块容量一致的硬盘。RAID 5 和SHR阵列其实是类似的，但是RAID 5 需要硬盘容量一致，SHR则不需要

![image-20220107235217893](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107235217893.png)

等待系统验证硬盘，通过奇偶校验。

![image-20220107235406955](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107235406955.png)

#### 4、下载应用

当设置完存储空间以后，则可以点击桌面的套件中心，下载一下常用的应用

- Drive，用来同步文件
- Moments 和 Photo，用来自动备份手机里面的照片
- Docker 容器
- Cloud Sync，可以同步NAS文件到百度网盘

等等



#### 5、共享文件夹

点击控制面板的共享文件夹，新增加一个共享文件夹，比如  NAS。用来存放各种常用的数据

![image-20220107151858684](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107151858684.png)







创建了共享文件夹后，我们打开我的电脑的地址栏，输入斜杠 + NAS的 ip地址。

输入用户名密码后，可访问NAS的存储空间![微笑](https://gitee.com/yokogood/YokoImages/raw/master/img/%E5%BE%AE%E7%AC%91.png)

![image-20220107152044714](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107152044714.png)



右键这个文件夹，点击映射网络驱动器

![image-20220107152103758](https://gitee.com/yokogood/YokoImages/raw/master/img/image-20220107152103758.png)





映射后，点击完成即可







