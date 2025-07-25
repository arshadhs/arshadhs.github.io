+++
title = 'Home Cloud'
date = 2016-03-05T22:51:00
draft = false
tags = ["BananaPi", "file-sharing", "storage"]
categories = ["tutorials", "banana-pi"]
+++

# Banana Pi - Home Cloud
---

### SSH
(Enable SSH)

```$ sudo apt-get install openssh-server

$ sudo nano /etc/rc.local
/etc/init.d ssh
exit 0```

### Resize
(Card on a ubuntu box)

```$ sudo fdisk -l
Disk /dev/mmcblk0: 7.4 GiB, 7948206080 bytes, 15523840 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x59d0bd13

Device         Boot     Start       End  Blocks  Id System
/dev/mmcblk0p1           2048    104447   51200  83 Linux
/dev/mmcblk0p2         104448  15523839 7709696  83 Linux

Disk /dev/sda: 7.5 GiB, 7998537728 bytes, 15622144 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x59d0bd13

Device    Boot     Start       End  Blocks  Id System
/dev/sda1           2048    104447   51200  83 Linux
/dev/sda2         104448   7167999 3531776  83 Linux

umount /dev/sda2

[bananapi@lemaker ~]$ sudo fdisk /dev/sda
Welcome to fdisk (util-linux 2.24.2).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
Command (m for help): d
Partition number (1,2, default 2): 2

Partition 2 has been deleted.

Command (m for help): n

Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): p
Partition number (2-4, default 2): 2
First sector (104448-15622143, default 104448): 335872
Last sector, +sectors or +size{K,M,G,T,P} (104448-15622143, default 15622143): 

Created a new partition 2 of type 'Linux' and of size 7.4 GiB.

Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.

pi@bananapi:~$ sudo resize2fs /dev/mmcblk0p2```

## SAMBA
(For accessing SSD from Windows PC inside home network on DLNA)

```pi@bananapi:~$ mkdir /home/pi/ssd

pi@bananapi:~$ sudo mount /dev/sda /home/pi/ssd

sudo apt-get install samba samba-common-bin -y

sudo cp -p smb.conf{,.bak}

sudo nano /etc/samba/smb.conf

[SSD] # My cloud
comment = SSD
path = /home/pi/ssd
create mask = 0775
directory mask = 0775
read only = no
browseable = yes
public = yes
force user = pi
#force user = root
only guest = no

sudo service smbd restart
sudo service nmbd restart```

## Mini DLNA
(For accessing SSD from devices in home network on DLNA)

```pi@bananapi:~$ sudo add-apt-repository ppa:stedy6/stedy-minidna
pi@bananapi:~$ sudo apt-get install minidlna
pi@bananapi:~$ sudo nano /etc/minidlna.conf

media_dir=P,/home/pi/ssd/photo
media_dir=A,/home/pi/ssd/music

friendly_name=my-cloud
```
