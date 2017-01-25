# Installation guide for Plex Media Server on Turris Omnia

> WIP version

## Overview

The installation consists of these steps:

1. Create Debian LXC container
2. Simple Debian configuration
3. Install Plex Media Server
4. Connect to CIFS

## 1. Create Debian LXC container

**[Official Manual](https://www.turris.cz/doc/en/howto/lxc)**

```
lxc-create -t download -n Debian
```

- Distribution: **Debian**
- Release: **Jessie**
- Architecture: **armv7l**

## 2. Simple Debian configuration

Connect to container:

```
lxc-attach -n Debian
```

Change hostname (for eg. `Debian`):

```
nano /etc/hostname
```

Set new hostname to localhost:

```
nano /etc/hosts
```

And add this line (for `Debian` as hostname):

```
127.0.1.1   Debian
```

Install packages:

```
apt-get install git-core openssh-server rsync sudo fakeroot cifs-utils -y
```

Create your user (eg. `petr`):

```
adduser petr
```

Run `visudo` commnad and add:

```
petr ALL=(ALL:ALL) ALL
```

Log to your user using SSH or sudo:

```
sudo su petr
```

## 3. Install Plex Media Server

Add repository

```
echo "deb http://dl.bintray.com/tproenca/pmsarm7 jessie main" | sudo tee /etc/apt/sources.list.d/pms.list
```

Fix for automatic start:

```
sudo touch /usr/lib/plexmediaserver/start.sh
```

Install:

```
sudo apt-get update
sudo apt-get install plexmediaserver
```

## 4. Connect to CIFS

Write down `plex` user id (`uid`):

```
id -u plex
```

Create empty folder for mount:

```
mkdir -p /media/video
```

Add mount to `/etc/fstab` (set `uid` from `plex` user)

```
//192.168.1.10/video /media/video cifs uid=107,gid=1000,iocharset=utf8 0 0
```

[For more about CIFS mount](http://midactstech.blogspot.cz/2013/09/how-to-mount-windows-cifs-share-on_18.html)

## Credits

[Plex Media Server ARM package for Debian/Ubuntu Linux](https://tproenca.github.io/pmsarm7/)
