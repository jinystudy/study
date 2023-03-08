

### 라즈베리파이 설치
https://www.raspberrypi.com/software/  
Manually install an operating system image - See all download options  
```2023-02-21-raspios-bullseye-armhf.img.xz``` 파일 다운로드  
=> "2023-02-21-raspios-bullseye-armhf.img"  

https://www.raspberrypi.com/  
Software > ```imager_1.7.4.exe``` 파일 다운로드  
=> SD카드에 다운로드 받은 "2023-02-21-raspios-bullseye-armhf.img" 파일 쓰기  

https://www.raspberrypi.com/software/operating-systems/  
Raspberry Pi OS > Raspberry Pi OS with desktop  
Release date: ```February 21st 2023```  
System: ```32-bit```  
Kernel version: 5.15  
Debian version: 11 (bullseye)  
Size: 924MB  


### 시스템 정보관련 명령어
$ ```df -h```  
/dev/root        59G  
$ ```free -h```  
Mem:           743Mi  
$ ```getconf LONG_BIT```  
32  
$ ```cat /proc/cpuinfo```  
Model : Raspberry Pi 3 Model B Rev 1.2  

### 시스템 업데이트 및 설정 관련 명령어
$ sudo apt-get update    
$ sudo apt-get upgrade    
$ ```sudo raspi-config```  
5 Localisation... => Timezone => Asia/Seoul  
6 Advanced Options => Expand Filesystem  

### 삼바설치(공유폴더)
$ sudo apt-get install samba samba-common-bin  
$ sudo smbpasswd -a pi  
$ sudo nano /etc/samba/smb.conf  
```
...
[homepi]
comment=Raspberry Pi Share
path=/
browseable=Yes
writeable=Yes
only guest=no
create mask=0777
directory mask=0777
pulic=no
```

### MariaDB 설치
```
pi@raspberrypi:~ $ sudo apt-get install mariadb-server mariadb-client

pi@raspberrypi:~ $ sudo mysql -u root
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 30
Server version: 10.5.15-MariaDB-0+deb11u1 Raspbian 11

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]> SELECT USER, HOST, PASSWORD FROM user;
+-------------+-----------+----------+
| User        | Host      | Password |
+-------------+-----------+----------+
| mariadb.sys | localhost |          |
| root        | localhost | invalid  |
| mysql       | localhost | invalid  |
+-------------+-----------+----------+
3 rows in set (0.006 sec)

MariaDB [mysql]> grant all privileges on *.* to 'root'@'%' identified by 'pwxxxx';
Query OK, 0 rows affected (0.006 sec)

MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.002 sec)
```

### MariaDB 외부접속허용
$ sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf  
주석처리   
```# bind-address = 127.0.0.1```  
$ sudo service mysql restart  


