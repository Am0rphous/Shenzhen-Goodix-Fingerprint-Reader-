# Shenzhen-Goodix-Fingerprint-Reader

**October 2024 update:** There might be a possible fix I haven't tried to get this to work. Check out [this](https://www.reddit.com/r/archlinux/comments/16jdx2x/goodix_27c65395_passing_from_goodixfpdump_to/) reddit post. It uses the [goodix-fp-dump](https://github.com/goodix-fp-linux-dev/goodix-fp-dump) repo, which also mentions a [Discord server](https://discord.com/invite/6xZ6k34Vqg) which may give more recent intel about progress. (Btw I sold the computer so let me know if anyone get this to work.) Cheers!
Edit: The script works.

<br>

This is just some notes i have related to enabling finger print reader on Linux on a Dell laptop. In the end i didn't get Kali (Debian) to work using the knowledge below. Shenzhen's source code for the fingerprint reader is closed btw >:( . If you get it to work, please give me a pull request or open an issue, and I will gladly update and clean up the resources below, thanks.<br><br>

| OS version | State| Date| Computer model | Comment |
| - | - | - | -| - |
| Ubuntu 24.04| Working | October 2024 | Dell XPS 15 7590| [Reddit post](https://www.reddit.com/r/archlinux/comments/16jdx2x/goodix_27c65395_passing_from_goodixfpdump_to/)|
| Ubuntu 23.10|Not working| March 2024||

<br>

According to the device list at https://gitlab.freedesktop.org/libfprint/wiki/-/wikis/Unsupported%20Devices, the Shenzhen Fingerprint Reader for device ID 27c6:5395 is not supported.

lsusb shows
````
Bus 001 Device 006: ID 27c6:5395 Shenzhen Goodix Technology Co.,Ltd. Fingerprint Reader
````

I tried the following without success:
1. Download and install `libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb` from [ubuntu](https://packages.ubuntu.com/focal/amd64/libfprint-2-tod1/download)
````
wget http://ubuntu.mirrors.tds.net/ubuntu/pool/main/libf/libfprint/libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb
sudo dpkg -i libfprint-2-tod1_1.90.1+tod1-0ubuntu4_amd64.deb 
````
2. Then install `libfprint-2-tod1-goodix_0.0.4`
````
wget http://dell.archive.canonical.com/updates/pool/public/libf/libfprint-2-tod1-goodix/libfprint-2-tod1-goodix_0.0.4-0ubuntu1somerville1_amd64.deb
$ sudo dpkg -i libfprint-2-tod1-goodix_0.0.4-0ubuntu1somerville1_amd64.deb
````

3. Tried running some commands do fix firmware and service
````
sudo pam-auth-update
fprintd-verify
fwupdmgr update                            #Updates firmware
sudo systemctl restart fprintd.service     #Restarts the fprintd service
sudo systemctl status fprintd.service      #Lets check the status
fprintd-enroll                             #Starts the enrolling process when setting up the figngerprint
journalctl -f -u fprintd.service           #any error should show here
````


# Might be nice to know about
- Try https://www.reddit.com/r/Dell/comments/hq3kf3/xps_13_fingerprint_drivers_are_here/
  - [laybe this](https://gist.github.com/d-k-bo/15e53eab53e2845e97746f5f8661b224)
- [Libfprint - goodix-fp-linux-dev](https://github.com/goodix-fp-linux-dev/libfprint) - Library for fingerprint readers
- [libfprint](https://gitlab.freedesktop.org/libfprint/libfprint) - For unsupported devices, please see/edit https://gitlab.freedesktop.org/libfprint/wiki/-/wikis/Unsupported%20Devices
- [Libfprint-fprintd](https://github.com/freedesktop/libfprint-fprintd) - D-Bus service to access fingerprint readers
  - [Libfprint](https://github.com/freedesktop/libfprint) - Library for fingerprint readers

- [Enable FingerPrint Login in Ubuntu](https://www.tec4tric.com/linux/enable-fingerprint-login-in-ubuntu)
- Download Page for libpam-fprintd_1.94.2-2_amd64.deb on AMD64 machines - [link](https://packages.debian.org/bookworm/amd64/libpam-fprintd/download)
- sudo apt install fprintd-doc fprintd libpam-fprintd
- [
How to set up built-in fingerprint reader authentication with PAM on any Linux
](https://codepre.com/how-to-set-up-built-in-fingerprint-reader-authentication-with-pam-on-any-linux.html)


## Ideel
````
lsusb
sudo apt install fprintd libpam-fprintd
fprintd
auth
````



- [source](https://www.dell.com/community/XPS/XPS-13-9300-Does-fingerprint-reader-work-on-linux/m-p/7628310/highlight/true#M63982)
````
sudo sh -c 'cat > /etc/apt/sources.list.d/focal-dell.list << EOF
deb http://dell.archive.canonical.com/updates/ focal-dell public
# deb-src http://dell.archive.canonical.com/updates/ focal-dell public

deb http://dell.archive.canonical.com/updates/ focal-oem public
# deb-src http://dell.archive.canonical.com/updates/ focal-oem public

deb http://dell.archive.canonical.com/updates/ focal-somerville public
# deb-src http://dell.archive.canonical.com/updates/ focal-somerville public

deb http://dell.archive.canonical.com/updates/ focal-somerville-melisa public
# deb-src http://dell.archive.canonical.com/updates focal-somerville-melisa public
EOF'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F9FDA6BED73CDC22
sudo apt update -qq

sudo apt install oem-somerville-melisa-meta libfprint-2-tod1-goodix oem-somerville-meta tlp-config -y

sudo add-apt-repository ppa:boltgolt/howdy -y
sudo apt update -qq
sudo apt install howdy -y
````


## Try this
- https://www.reddit.com/r/Dell/comments/ixwgm0/xps_15_9500_ubuntu_popos_fingerprint_howto/
  - https://askubuntu.com/questions/1015416/use-fingerprint-authentication-not-only-for-login
- Didnt help either http://dell.archive.canonical.com/updates/pool/public/libf/libfprint-2-tod1-broadcom/
- Hm? https://github.com/adnanjee/Goodix-GF3208
- Nope: https://www.dell.com/community/Linux-General/No-driver-for-fingerprint-scanner-Goodix-GF3208-on-Linux/m-p/6242579
