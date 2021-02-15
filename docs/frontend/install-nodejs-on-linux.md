# Install Node.js on Linux

## Using NodeSource

Refer to the [NodeSource documentation](https://github.com/nodesource/distributions/blob/master/README.md) for more
information on the available versions.

### Node.js v14.x:

```bash
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs

# Using Debian, as root
curl -fsSL https://deb.nodesource.com/setup_14.x | bash -
apt-get install -y nodejs
```

### Upgrade npm

```bash
npm install -g npm
```