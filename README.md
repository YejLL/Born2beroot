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

then, uncomment this line 'cgi.fix_pathinfo=1' inside the file php.ini
```
# vim /etc/php/7.3/fpm/php.ini
```
<img width="476" alt="스크린샷 2021-11-02 19 32 33" src="https://user-images.githubusercontent.com/80348069/139927048-f1a68d00-3ab1-4784-8356-b147eb88cf53.png">


now, we should edit the config file of lighttpd's Fastcgi.
```
# vim /etc/lighttpd/conf-available/15-fastcgi-php.conf
```

I commented out the 'bin path' and 'socket' and added this line -> "socket" => "/var/run/php/php7.3-fpm.sock"

<img width="668" alt="스크린샷 2021-11-02 19 39 35" src="https://user-images.githubusercontent.com/80348069/140535356-f19b4d04-bcf6-493f-ab6b-1ab07a9f205d.png">

then, you should reload lighttpd for enable mode when you start up your virtual machine with this command
```
$ sudo lighttpd-enable-mod fastcgi
$ sudo lighttpd-enable-mod fastcgi-php
service lighttpd force-reload
```


I also allow a port 80 on ufw, because lighttpd uses this port for server. you can easily verify inside the file lighttpd.conf
```
$ ufw allow 80
$ sudo vim /etc/lighttpd/lighttpd.conf
```

<img width="713" alt="스크린샷 2021-11-02 19 28 59" src="https://user-images.githubusercontent.com/80348069/140537052-a75492dd-52d7-4757-b9c6-39d42448b890.png">



**If you want to know whether php was successfully linked, make a file info.php on /var/www/html**
```
$ sudo vim /var/www/html/info.php
```
and write like this photo on your file info.php

<img width="238" alt="스크린샷 2021-11-05 16 46 42" src="https://user-images.githubusercontent.com/80348069/140539678-d7d4cb52-5bf7-424e-84d5-5dfa0ee9ddb2.png">


I personally used a port forwarding to connect a server, if you write (hostip):(a port number that you choose for host port) on address bar
```
http://192.100.00.00:8080/info.php
```

now, you can see this information page if php was successfully installed.

<img width="807" alt="스크린샷 2021-11-05 16 48 00" src="https://user-images.githubusercontent.com/80348069/140541922-b109f83a-79ad-4506-b671-a8007cb884cb.png">





## Born2beroot bonus part3. Installation Maria DB

Install maria db server via this command
```
$ sudo apt install mariadb-server
```

if you want to know whether maria db was successfully installed, you can verify with this command
```
$ dpkg -l | grep mariadb-server
```

for enable mode **Maria DB** on startup
```
# systemctl start mariadb
# systemctl enable mariadb
# systemctl status mariadb
```

for secure MariaDb server via this command
```
# mysql_secure_installation
```

for setting, follow this step
```
# Switch to unix_socket authentication [Y/n]: Y
# Enter current password for root(enter for none): Enter
# Set root password? [Y/n]: Y
# New password: (write your own password)
# Re-enter new password: (same password)
# Remove anonymous users? [Y/n]: Y
# Disallow root login remotely? [Y/n]: Y
# Remove test database and access to it? [Y/n]: Y
# Reload privilege table now? [Y/n]: Y
```

then, you should restart Maria db service via this command
```
# systemctl restart mariadb
```

you can enter on maria db with this command by logging-in with your password that you choose on setting
```
# mysql -u root -p
```
then, create a database for your wordpress site
```
MariaDB [(none)]> CREATE DATABASE wordpress;
MariaDB [(none)]> CREATE USER 'your user name'@'localhost' IDENTIFIED BY 'your password';
MariaDB [(none)]> GRANT ALL ON wordpress.* TO 'your user name'@'localhost' IDENTIFIED BY 'your password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
**GRANT ALL ON ~... WITH GRANT OPTION; -> give a full access to DB that you created**


## Born2beroot bonus Problems encountered

I worked on my labtop and i should take a trip with it. but when i verified a wordpress site, i recognized that my ip address was changed. In this case, you can not access to your wordpress site with your previous ip address. so, i changed my ip address on virtual machine.

first, you should connect in maria DB
```
# mysql -u root -p
```

then, follow this command
```
use wordpress;
show tables;
```

now, it will show you a tables like this photo

<img width="496" alt="스크린샷 2021-11-05 17 48 19" src="https://user-images.githubusercontent.com/80348069/140549492-c164fe35-7b41-41c7-9680-a3abefdfa5fd.png">

on wp_options's table has a information that we need to change, you can access to it via this command
```
select * from wp_options where option_id<3;
```

you can verify your previous ip address that you should change

<img width="750" alt="스크린샷 2021-11-05 17 49 17" src="https://user-images.githubusercontent.com/80348069/140550276-9cbcd9dd-c50e-4921-9877-eecdf7af0d6a.png">

write a command to change your current ip address
```
update wp_options set option_value='http://(current ip address)' where option_id<3;
```

now, you can see the ip address was successfully changed
```
select * from wp_options where option_id<3;
```
