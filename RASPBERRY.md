## Raspberry Pi
on a Raspberry Pi 3 Model B running Raspian Stretch Lite, Version: March 2018, Release date: 2018-03-13, Kernel version: 4.9

### setup

download the [latest image](https://www.raspberrypi.org/downloads/raspbian/)

insert the SD card into the workstation and identify the `disk-number`:
```
$ diskutil list
```

unmount the disk:
```
$ diskutil unmountDisk /dev/disk<disk-number>
```

copy the image to the SD card:
```
$ sudo dd bs=1m if=<path to recently downloaded image> of=/dev/rdisk<disk-number> conv=sync
```

check the progress with `SIGINFO` (ctrl+t):
```
load: 2.81  cmd: dd 15456 uninterruptible 0.00u 0.09s
239+0 records in
238+0 records out
249561088 bytes transferred in 18.255963 secs (13670114 bytes/sec)
```

enable ssh:
```
touch /Volumes/boot/ssh
```

once the image is copied over, eject the disk:
```
$ diskutil eject /dev/rdisk<disk-number>
```

get the ip address of the Raspberry Pi:
```
$ sudo arp-scan --interface=en1 --localnet
```

change the `hostname` of the Raspberry Pi, and add a user:
```
$ ssh pi@<ip-address>
$ sudo sed -ie 's/raspberrypi/milkbar/g' /etc/hostname
$ sudo sed -ie 's/raspberrypi/milkbar/g' /etc/hosts
```

make the ip address static:
```
$ sudo sed -e "\$aip=<ip-address>" /boot/cmdline.txt
```

from **milkbar**, expand the root partition to use the SD card's full capacity, and reboot:
```
$ sudo raspi-config
$ sudo reboot now
```

from **milkbar**, install the media server:
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
$ sudo apt-get install apt-transport-https
$ wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key  | sudo apt-key add -
$ echo "deb https://dev2day.de/pms/ stretch main" | sudo tee /etc/apt/sources.list.d/pms.list
$ sudo apt-get update
$ sudo apt-get install plexmediaserver-installer
```

identify the `device-uuid` and `device-name` of the mounted external hard drive:
```
$ sudo blkid
```

install package so we can mount ntfs
```
$ apt-get install -y ntfs-3g
```

add record to `/etc/fstab` for mounting on boot:
```
UUID=<device-uuid> /mnt/plex ntfs defaults,auto,umask=000,users,rw 0 0
```