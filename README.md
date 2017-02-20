## milkbar

The below is a guide to setup a media server on a Raspberry Pi 3 Model B running Raspian Jessie with Pixel, Version: January 2017, Release date: 2017-01-11, Kernel version: 4.4

download the [latest image](https://www.raspberrypi.org/downloads/raspbian/)

insert the SD card into the workstation and identify the `disk number`:
```
$ diskutil list
```

unmount the disk:
```
$ diskutil unmountDisk /dev/disk<disk number>
```

copy the image to the SD card:
```
$ sudo dd bs=1m if=<path to recently downloaded image> of=/dev/rdisk<disk number>
```

check the progress with `SIGINFO` (ctrl+t):
```
load: 2.81  cmd: dd 15456 uninterruptible 0.00u 0.09s
239+0 records in
238+0 records out
249561088 bytes transferred in 18.255963 secs (13670114 bytes/sec)
```

once the image is copied over, eject the disk:
```
$ diskutil eject /dev/rdisk<disk number>
```

ssh is [disabled by default](https://www.raspberrypi.org/documentation/remote-access/ssh/). Insert the SD card into the Raspberry Pi, and boot it up. Use a mouse and a display to navigate through the Raspberry Pi Configuration and enable ssh.

get the ip address of the Rasberry Pi:
```
$ sudo arp-scan --interface=en1 --localnet
```

change the `hostname` of the Raspberry Pi, and add a user:
```
$ ssh pi@<ip-address>
$ sudo sed -ie 's/raspberrypi/milkbar/g' /etc/hostname
$ sudo sed -ie 's/raspberrypi/milkbar/g' /etc/hosts
$ sudo adduser <user name>
```

make the ip address static:
```
$ sudo sed -e "\$aip=<ip-address>" /boot/cmdline.txt
```

give the new user sudo priveledges, by copying the `root` line within the `User priveledge specification`, and replacing `root` with the new `<user name>`:
```
$ sudo visudo
```

reboot the Raspberry Pi
```
$ sudo reboot now
```

from the workstation, copy your keys to **milkbar**:
```
$ ssh-copy-id milkbar.local
```

from **milkbar**, expand the root partition to use the SD card's full capacity, and reboot:
```
$ sudo raspi-config
$ sudo reboot now
```

from **milkbar**, install the media server:
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install apt-transport-https
$ wget -O - https://dev2day.de/pms/dev2day-pms.gpg.key  | sudo apt-key add -
$ echo "deb https://dev2day.de/pms/ jessie main" | sudo tee /etc/apt/sources.list.d/pms.list
$ sudo apt-get update
$ sudo apt-get install -t jessie plexmediaserver
$ sudo sed -ie 's/PLEX_MEDIA_SERVER_USER=plex/PLEX_MEDIA_SERVER_USER=thomas/g' /etc/default/plexmediaserver
$ sudo service plexmediaserver restart
$ sudo reboot now
```

connect an external hardrive to **milkbar**, and update it's access permissions:
```
$ sudo chmod 775 /media/pi
```

identify the `device-name` of the mounted external hard drive:
```
$ sudo blkid
```

mount the external hard drive at boot
```
$ sudo sed -e "\$a<device-name> /media/pi/thomasshouler hfsplus defaults 0 0" /etc/fstab
```