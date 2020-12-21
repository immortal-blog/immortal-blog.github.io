# Ubuntu LVM硬盘在线扩容教程

### 新建一个主分区

`sudo fdisk /dev/sda`
输入 m 查看帮助，输入 n 新建分区，选择主分区，一步步操作，按 w 写入保存，得到/dev/sda2

### 格式化该分区为 ext4 格式

`sudo mkfs -t ext4 /dev/sda2`

### 将该新分区加入 lvm 的原有 VG

`vgextend ubuntu--vg-root /dev/sda2`

### 更新磁盘大小

`resize2fs -p /dev/mapper/ubuntu--vg-root`

### 查看最新的硬盘大小

`df -h`
