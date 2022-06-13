---
title: Apache on CentOS
cover: ./image.jpg
date: 2022-06-13
description: Web Server.
tags: ['Apache']
---

# **CentOS**

----
* Go to file **vhost.conf**.
```
vi /etc/httpd/conf.d/vhost.conf
```
* Default Configuration Virtual Hostings Multiple Domains.
```
#For Original Domain
    <VirtualHost *:80>
        DocumentRoot /var/www/html
        ServerName www.domain.name
    </VirtualHost>
    
#For New Domain
    <VirtualHost *:80>
        DocumentRoot /var/www/virtual.host
        ServerName www.domain.name
        ServerAdmin webmaster@domain.name
        ErrorLog logs/domain.name-error_log
        CustomLog logs/domain.name-access_log combined
    </VirtualHost>
```
* Create directory (Docroot) and Restart service Apache.
```
mkdir /var/www/virtual.host
systemctl restart httpd
```
