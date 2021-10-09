# Create socks server on Ubuntu 20.04

In daily life, we may need to create a socks server to provide network for other hosts.   
I summarized the method of creating socks service through dante-server under Ubuntu 20.04.

### install dante-server

```shell
sudo apt install dante-server
```

### add user for dante-server

Use this method to create a user to prevent it from logging in.

```shell
sudo useradd -s /sbin/nologin dante-socks
```

### Edit the configuration file

```shell
sudo nano /etc/danted.conf
```

minimal configuration for dante-server

```
logoutput: syslog stdout
internal: ztuku335yr port = 1080
external: ens3
user.notprivileged: dante-socks
clientmethod: none
socksmethod: none
#allow connections from local network
client pass {
        from: 10.0.0.229/24 to: 0.0.0.0/0
        log: error # connect disconnect
}
socks pass {  
        from: 0.0.0.0/0 to: 0.0.0.0/0
        command: bind connect udpassociate
        log: error # connect disconnect iooperation
}
socks pass {
        from: 0.0.0.0/0 to: 0.0.0.0/0
        command: bindreply udpreply
        log: error # connect disconnect iooperation
}
```