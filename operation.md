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



### * Conda创建虚拟环境
创建个人环境
> conda create -n 环境名 python=3.6

激活
> source activate 环境名

安装包环境
> conda install -n 环境名 包名

删除包环境
> conda remove --name 环境名  包名

关闭
> source deactivate

复制
> conda create -n 新环境名 --clone 旧环境名 

删除
> conda remove -n 环境名 --all

分享环境
> source activate 要分享的环境名
>
> conda env export > environment.yml

获得environment,yml文件后，复制到要分享的电脑上
> conda env create -f environment.yaml
>
> or
>
> conda env create -f=environment.yml



### pytorch官方指令表(18.10.21)

```
# cuda 8.0
conda install pytorch torchvision cuda80
# cuda 9.0
conda install pytorch torchvision
# cuda 9.2
conda install pytorch torchvision cuda92
```

### pytorch1.0安装指令
```
# Linux, Conda, Python 3.6, CUDA 9
conda install pytorch torchvision -c pytorch

# Linux, Conda, Python 3.6, CUDA 10
conda install pytorch torchvision cuda100 -c pytorch

# Linux, pip, Python 3.6, CUDA 9
pip3 install torch torchvision

# Linux, pip, Python 3.6, CUDA 10
pip3 install http://download.pytorch.org/whl/cu100/torch-1.0.0-cp36-cp36m-linux_x86_64.whl
pip3 install torchvision

# Linux, Conda, Python 2.7, CUDA 9
conda install pytorch torchvision -c pytorch

# Linux, Conda, Python 2.7, CUDA 10
conda install pytorch torchvision cuda100 -c pytorch

# Linux, pip, Python 2.7, CUDA 9
pip install torch torchvision

# Linux, pip, Python 2.7, CUDA 10
pip install http://download.pytorch.org/whl/cu100/torch-1.0.0-cp27-cp27mu-linux_x86_64.whl
pip install torchvision
# if the above command does not work, then you have python 2.7 UCS2, use this command
pip install http://download.pytorch.org/whl/cu100/torch-1.0.0-cp27-cp27m-linux_x86_64.w
```

### 辅助环境包

> conda install scikit-learn scipy numpy


### 配置github SSH key

> ssh-keygen -t rsa -b 4096 -C "你的邮箱地址"

提示密钥存储在家目录.ssh下面的id_rsa
提示输入密钥密码（默认回车为空）
提示创建成功

在后台创建ssh进程
> eval "$(ssh-agent -s)"

添加ssh密钥
> ssh-add ~/.ssh/id_rsa

打开github设置页面选择SSH and GPG keys
选择New SSH key

将～/.ssh/id_rsa.public复制到Key中，随意设置一个Title，Add SSH Key

> cat ~/.ssh/id_rsa.pub



### 在conda虚拟环境中使用jupyter

在虚拟环境中安装jupyer-kernel

> conda install ipykernel

将环境写入notebook的kernel中

> python -m ipykernel install --user --name 环境名称 --display-name "Python (环境名称)"

如系统已安装jupyter，则在虚拟环境外启动jupyter

>  jupyter notebook

此时在New中可以看见虚拟环境的名字



### 将Conda中的python作为系统默认
vim ~/.bashrc
export PATH="/home/myname/anaconda2/bin:$PATH"
source ~/.bashrc



### 备份

>  tar cvpzf backup.tgz --exclude=/proc --exclude=/lost+found --exclude=/backup.tgz --exclude=/mnt --exclude=/sys --exclude=/media 
> $ cd /media/（U盘）

在tryUbuntu根目录下有media文件夹，里面是U盘文件夹和新安装的系统文件夹，在在里分别用（U盘）和（UBUNTU）表示
> $ sudo su
> mount -o remount rw ./
> sudo cp /media/(Ubuntu)/boot/grub/grub.cfg ./    

将新系统根目录下/boot/grub/grub.cfg文件备份到U盘中
> sudo cp /media/(UBUNTU)/etc/fstab ./

将新系统根目录下/etc/fstab文件备份到U盘中
fstab是与系统开机挂载有关的文件，grub.cfg是与开机引导有关的文件，所以这一步至关重要
> cd /media/(UBUNTU)

删除新装ubuntu全部的系统文件，有用的fstab及grub.cfg已经备份
> sudo rm -rf ./*

将U盘中backup.tgz复制到该目录下
> cp /media/(U盘)/backup.tgz ./

解压缩
> sudo tar xvpfz backup.tgz ./

创建打包系统时排除的文件

> sudo mkdir proc lost+found mnt sys media tmp

### Ubuntu18.04.1 安装docker

```
$ sudo apt install docker.io
$ sudo systemctl start docker
$ sudo systemctl enable docker
$ docker --version
```

### docker-CE

https://docs.docker.com/install/linux/docker-ce/ubuntu/

### caffe

https://github.com/BVLC/caffe/tree/master/docker

### Ubuntu清除内存
```
sudo sh -c 'echo 1 > /proc/sys/vm/drop_caches'
sudo sh -c 'echo 2 > /proc/sys/vm/drop_caches'
sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
```

### Ubuntu旋转显示器

> xrandr -o left

### 指定GPU设备

> CUDA_VISIBLE_DEVICES=1 python xxx.py