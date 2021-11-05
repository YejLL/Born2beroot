# Born2beroot

This project aims to introduce you to the wonderful world of virtualization.
You will create your first machine in VirtualBox (or UTM if you can’t use VirtualBox)
under specific instructions. Then, at the end of this project, you will be able to set up
your own operating system while implementing strict rules.



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

## Born2beroot Bonus part2. Installation PHP

install php with this command
```
$ sudo apt-get install php7.3-fpm
```

then, uncomment this line 'cgi.fix_pathinfo=1' with this command
```
# vim /etc/php/7.3/fpm/php.ini
```
<img width="476" alt="스크린샷 2021-11-02 19 32 33" src="https://user-images.githubusercontent.com/80348069/139927048-f1a68d00-3ab1-4784-8356-b147eb88cf53.png">

in this configuration file of lighttpd, 
