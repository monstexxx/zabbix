## How to install Zabbix in Ubuntu 18.04

### Check Version
```
root@nugraha:/home/nugraha# lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.5 LTS
Release:	18.04
Codename:	bionic
root@nugraha:/home/nugraha#
```

### Install Zabbix Latest Version
```
root@nugraha:/home/nugraha# wget https://repo.zabbix.com/zabbix/5.4/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.4-1+ubuntu18.04_all.deb
root@nugraha:/home/nugraha# dpkg -i zabbix-release_5.4-1+ubuntu18.04_all.deb
root@nugraha:/home/nugraha# apt update
```

### Install Zabbix server, frontend, agent
```
root@nugraha:/home/nugraha# apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

### Create Initial Database
```
root@nugraha:/home/nugraha# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 29331
Server version: 5.7.34-0ubuntu0.18.04.1 (Ubuntu)

Copyright (c) 2000, 2021, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
mysql> quit;

--------------------------------------------------------
# nama database: zabbix
# user database: zabbix
# pass database: zabbix
--------------------------------------------------------

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| zabbix             |
+--------------------+
5 rows in set (0.02 sec)

mysql> quit;
```

### Import Zabbix Database
```
root@nugraha:/home/nugraha# zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix
Password= zabbix
```

### Edit Zabbix Configuration
```
root@nugraha:/home/nugraha# nano /etc/zabbix/zabbix_server.conf
--------------------------------------------------------
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
--------------------------------------------------------
```

### Restart Service
```
root@nugraha:/home/nugraha# service zabbix-server start
root@nugraha:/home/nugraha# update-rc.d zabbix-server enable
```

### Edit Apache2 Configuration
```
root@nugraha:/home/nugraha# nano /etc/apache2/conf-enabled/zabbix.conf

--------------------------------------------------------
php_value max_execution_time 300
php_value memory_limit 128M
php_value post_max_size 16M
php_value upload_max_filesize 2M
php_value max_input_time 300
php_value max_input_vars 10000
php_value always_populate_raw_post_data -1
php_value date.timezone Asia/Jakarta
--------------------------------------------------------

root@nugraha:/home/nugraha# service apache2 restart
```

## Access Zabbix
```
http://10.211.55.24/zabbix/
```




