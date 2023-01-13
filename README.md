# Shenzhen-Goodix-Fingerprint-Reader-
According to the device list at https://gitlab.freedesktop.org/libfprint/wiki/-/wikis/Unsupported%20Devices, the Shenzhen Fingerprint Reader for device ID 27c6:5395 is not supported. Yet here is a fix and how to get it working.

lsusb shows
````
Bus 001 Device 006: ID 27c6:5395 Shenzhen Goodix Technology Co.,Ltd. Fingerprint Reader
````

To get it working do the following
1. Download and install `libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb`
````
wget http://ubuntu.mirrors.tds.net/ubuntu/pool/main/libf/libfprint/libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb
sudo dpkg -i libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb 
````
2. Then install `libfprint-2-tod1-goodix_0.0.4`
````
wget http://dell.archive.canonical.com/updates/pool/public/libf/libfprint-2-tod1-goodix/libfprint-2-tod1-goodix_0.0.4-0ubuntu1somerville1_amd64.deb
$ sudo dpkg -i libfprint-2-tod1-goodix_0.0.4-0ubuntu1somerville1_amd64.deb
````
