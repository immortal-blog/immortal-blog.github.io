# Online expansion of Ubuntu LVM disk

### Create a new primary partition

```bash
sudo fdisk /dev/sda
```

Enter `m` to view the help, enter `n` to create a new partition, select the primary partition, step by step, press `w`
to write and save, and get /dev/sda2

### Format the partition as ext4 format

```bash
sudo mkfs -t ext4 /dev/sda2
```

### Add the new partition to the original VG of lvm

```bash
vgextend ubuntu--vg-root /dev/sda2
```

### Update file system size

```bash
resize2fs -p /dev/mapper/ubuntu--vg-root
```

### View the latest hard drive size

```bash
df -h
```