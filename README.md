# Born2beroot

This project aims to introduce you to the wonderful world of virtualization.
You will create your first machine in VirtualBox (or UTM if you can’t use VirtualBox)
under specific instructions. Then, at the end of this project, you will be able to set up
your own operating system while implementing strict rules.
Personally, i choose a debian to 


## Born2beroot Bonus part1. Installation Lighttpd


you can install lighttpd with this command
```
$ sudo apt-get install lighttpd
```

check with this command if lighttpd is properly installed
```
$ dpkg -l | grep lighttpd
$ lighttpd -v   #version check
```

To start,stop this lighttpd service and enable mode at start up, follow this command
```
$ sudo systemctl start lighttpd.service
$ sudo systemctl stop lighttpd.service
$ sudo systemctl enable lighttpd.service
```
