# Create a private Ubuntu mirror

To create a private Ubuntu image, the commonly used tools are debmirror and apt-mirror.  
As of today (2021-05-17), these two tools have their own advantages and disadvantages.

## debmirror

[debmirror document](https://help.ubuntu.com/community/Debmirror)  
debmirror has many downloading methods, and it will compare the local file with the file on the server before
downloading so that only the missing files are downloaded. However, it does not seem to support multi-threaded
downloading. Debmirror is downloaded one by one, so the download speed is relatively slow.

## apt-mirror

[apt-mirror document](https://apt-mirror.github.io/)  
apt-mirror supports 20 threads to download at the same time by default. If you choose a suitable mirror station, the
download speed can reach the maximum of my network. However, it does not seem to make a comparison. Although it can
speed up the download start speed, sometimes some files may be lost, resulting in an incomplete mirror, so the client
cannot be updated.