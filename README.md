PHP DDNS Server
==============

This is a DDNS (or DynDNS)-Server written in pure PHP.  
The records lie in a MySQL database and can easily be updated via the update.php script.  
The installation information below is suited for a newly installed debian jessie.  
  
Only one thread answers requests per interface. So maybe don't use for heavy load 😉  
Requires PHP >= 5.3 and has very little overhead

Supported record types
----------------------

* A
* NS
* CNAME
* SOA
* MX
* TXT
* AAAA

Supported DDNS protocols:
----------------------
 * cron/wget
 * dyndns2

Installation:
-------------
```
apt install php5-cli php5-mysqlnd supervisor mysql-server libapache2-mod-php5

mkdir /var/ddns
git clone https://github.com/nemiah/PHP-DDNS-Server.git

cd PHP-DDNS-Server

cp serverAll.conf /etc/supervisor/conf.d/
ln -s /var/ddns/PHP-DDNS-Server/update.php /var/www/html/update.php
ln -s /var/ddns/PHP-DDNS-Server/checkip.php /var/www/html/checkip.php

mysql -uroot -p < setup.sql

service supervisor restart

dig @localhost nemiah.de
```

IP update:
----------
Via cron/wget:
```
wget -qO- https://nemiah.de/update.php?domain=home.nemiah.de&username=nena&password=Hallo123&ip=123.123.123.123 &> /dev/null
dig @localhost home.nemiah.de
```

Via ddclient:
```
# /etc/ddclient.conf

protocol=dyndns2
use=web, web=nemiah.de/checkip.php
server=nemiah.de/update.php
login=nena
password='Hallo123'
home.nemiah.de
```