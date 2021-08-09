# Monitor the temperature and health status of nvme ssd in Ubuntu

Install nvme-cli

```
sudo apt-get install nvme-cli
```

List disks

```
sudo nvme list
```

View the details of the hard drive

```
sudo nvme smart-log /dev/nvme0
```

Auto refresh status

```
sudo watch -n 1 nvme smart-log /dev/nvme0
```

Only view the temperature info

```
sudo nvme smart-log /dev/nvme0 | grep "^temperature"
```