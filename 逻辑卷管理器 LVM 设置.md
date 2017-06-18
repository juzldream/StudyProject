1. 逻辑卷管理器 [logical volume manager]：
LVM 可以整合多个物理分区在一起，让这些分区看起来就像是一个磁盘一样，而且，还可以在将来其它的物理分区或将其从这个LVM管理的磁盘当中删除。LVM 重点在于可以单独调整文件系统的容量。

![LVM](https://mmbiz.qlogo.cn/mmbiz/4iaE7bB4HCjeIYfjteiaHdFLL5RxQicTUTxlickEicGhd4FvsErwluT6kOxEZQw34r4gTK6MsJuPrvBt7xgydNZFibIQ/0?wx_fmt=gif)

2. who ？ PV 、VG 、PE 、LV ：

- Physical Volume，PV，物理卷
指磁盘分区或从逻辑上与磁盘分区具有同样功能的设备（如RAID），是LVM的基本存储逻辑块，但和基本的物理存储介质（如分区、磁盘等）比较，却包含有与LVM相关的管理参数。
- Volume Group，VG，卷用户组
类似于非LVM系统中的物理磁盘，其由一个或多个物理卷PV组成。可以在卷组上创建一个或多个LV（逻辑卷）。
- Physical Extend，PE,物理扩展卷
 每一个物理卷PV被划分为称为PE（Physical Extents）的基本单元，具有唯一编号的PE是可以被LVM寻址的最小单元。PE的大小是可配置的，默认为4MB。所以物理卷（PV）由大小等同的基本单元PE组成。这个 PE 有点像文件系统里面的 block 大小。
- Logical Volume，LV,逻辑卷
逻辑卷LV也被划分为可被寻址的基本单位，称为LE。在同一个卷组中，LE的大小和PE是相同的，并且一一对应。LV 最后可以被格式化使用的文件系统了。
- Physical Storage Media，PSM,物理存储介质
指系统的物理存储设备：磁盘，如：/dev/hda、/dev/sda等，是存储系统最底层的存储单元。

3. 创建和管理 LVM
* 创建物理卷 PV
    - 与 PV 相关命令
        *  pvcreate：初始化一个磁盘分区为 LVM
        * pvscan：显示目前系统具有PV的磁盘
        * pvdisplay：显示 PV 属性
        * pvremove：删除 PV
    - 检查有无 PV 在系统上，然后新建 /dev/sdb1~/dev/sdb4 位 PV 格式
```
    Last login: Sun Aug  7 01:58:59 2016 from             192.168.88.1
    Welcome To JuzlDream ^_^
    [root@testplan ~]# pvscan
    No matching physical volumes found

```
```
    [root@testplan ~]# pvcreate /dev/sdb1
    Physical volume "/dev/sdb1" successfully created
    [root@testplan ~]# pvcreate /dev/sdb2
    Physical volume "/dev/sdb2" successfully created
    [root@testplan ~]# pvcreate /dev/sdb3
    Physical volume "/dev/sdb3" successfully created
    [root@testplan ~]# pvcreate /dev/sdb5
    Physical volume "/dev/sdb5" successfully created

```

```
    [root@testplan ~]# pvscan
  PV /dev/sdb1                      lvm2 [15.66 MiB]
  PV /dev/sdb2                      lvm2 [7.84 MiB]
  PV /dev/sdb3                      lvm2 [7.84 MiB]
  PV /dev/sdb5                      lvm2 [7.81 MiB]
  Total: 4 [39.16 MiB] / in use: 0 [0   ] / in no VG: 4 [39.16 MiB]
```

* 创建卷组 VG
    - 与vg相关的命令
        * vgextend：
        * vgeduce：
        * vgchange：
        * vgromve:
    - 建立 vg ，并制定 pe 为 16 MB.
```
[root@testplan ~]# vgcreate -s 16M testplanVG /dev/sdb1
  Volume group "testplanVG" successfully created

```

* 创建逻辑卷 LV
    - 与lv相关的命令
        - lvextend：
        - lvreduce：
        - lvremove：
        - lvresize：
    - 将整个 vg 全部分配给 lv ，注意 pe 个数
```
[root@testplan ~]# vgdisplay 
  --- Volume group ---
  VG Name               testplanVG
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               1.75 TiB
  PE Size               16.00 MiB
  Total PE              114977
  Alloc PE / Size       114977 / 1.75 TiB
  Free  PE / Size       0 / 0   
  VG UUID               ko05Yo-YK1H-hSaU-u0VT-B7li-e1tN-STtHez


```
```
[root@testplan ~]# lvcreate -l 114977 -n testplanLV testplanVG
  Logical volume "testplanLV" created

```
```
[root@testplan ~]# ls -l /dev/testplanVG/testplanLV 
lrwxrwxrwx. 1 root root 7 Aug  7 03:14 /dev/testplanVG/testplanLV -> ../dm-0
```


---

![clv](https://mmbiz.qlogo.cn/mmbiz_png/4iaE7bB4HCjd6ZV957qIuIECmkPyKo42F8gCw7dDZ6phB8DJfWAESLiaPCRld0opIqCdiauZmmBNVO6BZGdpias3ag/0?wx_fmt=png)


* 文件系统
    * 格式化、挂载、查看
```
[root@testplan ~]# mkfs -t ext3 /dev/testplanVG/testplanLV 
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=1024 (log=0)
Fragment size=1024 (log=0)
Stride=0 blocks, Stripe width=0 blocks
3072 inodes, 12288 blocks
614 blocks (5.00%) reserved for the super user
First data block=1
Maximum filesystem blocks=12582912
2 block groups
8192 blocks per group, 8192 fragments per group
1536 inodes per group
Superblock backups stored on blocks: 
	8193

Writing inode tables: done                            
Creating journal (1024 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 20 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.

```
```
[root@testplan testLVM]# mkdir /mnt/testLVM
```


```
[root@testplan testLVM]# mount /dev/testplanVG/testplanLV /mnt/testLVM/
```
