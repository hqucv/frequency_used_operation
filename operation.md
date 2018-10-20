### * 硬盘格式化以及挂载

> df -h
>
> sudo fdisk -l

两个命令，前者查看当前挂载的硬盘，后者查看可以查询到的所有硬盘信息。

查看当前未挂载的硬盘，接下来我们要进行硬盘格式化以及建立文件系统。

假设目前未挂载的硬盘上为/dev/sdb

> sudo fdisk /dev/sdb

根据命令：

选择p查看当前磁盘情况

选择d删除分区

选择n新建分区，选择e建立逻辑分区，输入分区数量（默认一个分区），选择起始位置和终止位置（默认回车两次）

选择t创建分区类型，L显示所有分区类型的代码，选择83（Linux）

w进行保存，此时完成格式化。下面建立ext4文件系统：

> sudo mkfs -t ext4 /dev/sdb

对于一般存储，我们使用ext4文件系统。

接着进行挂载处理，首先在/media下面创建一个disk1文件夹

> sudo mount /dev/sdb /media/disk1
>
> sudo chown -R 用户名:用户组 /media/disk1

此时就可以使用新的硬盘了，如果我们想要开机挂载，要修改/etc/fstab文件

首先查看对应硬盘的UUID

> sudo blkid

找到/dev/sdb对应的UUID，修改/etc/fstab文件

> sudo vim /etc/fstab

增加下面一条

> UUID=刚刚查看的UUID号  /media/disk1  ext4  defaults  0  3

重启查看是否会自动挂载