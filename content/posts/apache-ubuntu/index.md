---
title: Apache on Ubuntu
cover: ./apache.jpeg
date: 2022-06-14
description: Web Server
tags: ['Apache']
---

----
### Installation.
* Install Apache2.
```
apt -y install apache2
```
* Configure Apache2.
```
vi /etc/apache2/apache2.conf
```
```
#In line 70: add your server name
ServerName www.domain.name
```
```
vi /etc/apache2/mods-enabled/dir.conf
```
```
#In line 2: add file name that it can access only with directory's name
DirectoryIndex index.html index.htm index.php
```
* Restart service.
```
systemctl restart apache2
```
* Test and Access _http://(your server name or IP Address)_ with web browser. If no problem, page is show default page Apache2 Ubuntu.
----
### Virtual Hostings.
* Create and Go to file **domain.name.conf**.
```
vi /etc/apache2/sites-available/domain.name.conf
```
* Default Configuration.
```
#create new
<VirtualHost *:80>
    ServerName www.domain.name
    ServerAdmin webmaster@domain.name
    DocumentRoot /home/ubuntu/public_html
    ErrorLog /var/log/apache2/domain.name.error.log
    CustomLog /var/log/apache2/domain.name.access.log combined
    LogLevel warn
</VirtualHost>
```
* Activating VirtualHost File and Restart service Apache.
```
a2ensite domain.name
systemctl restart apache2
```
