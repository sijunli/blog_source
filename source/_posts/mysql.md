---
title: Mysql create new account in Ubuntu14.04
date: 2016-06-06 01:25:56
tags: 
- linux
- mysql
- wiki
---

```shell
$ sudo /etc/init.d/mysql stop
$ sudo mysqld_safe --skip-grant-tables &
$ mysql -uroot -p

mysql> use mysql
mysql> insert into user(host,user,password)  values('%','username',password('12345678'));

$ mysqladmin shutdown
$ sudo /etc/init.d/mysql start
$ mysql -uroot -p

mysql> grant all privileges on userdb.* to username@localhost;
mysql> flush privileges;

$ sudo /etc/init.d/mysql restart
```

