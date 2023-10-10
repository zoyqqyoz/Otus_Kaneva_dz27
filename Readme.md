ДЗ 27. MySQL репликация

Поднимаем 2 ВМ в соответствии с предложенным стендом: https://gitlab.com/otus_linux/stands-mysql

```
neva@Uneva:~/Otus_Kaneva_dz27$ vagrant up
Bringing machine 'master' up with 'virtualbox' provider...
Bringing machine 'slave' up with 'virtualbox' provider...
==> master: Importing base box 'centos/7'...
==> master: Matching MAC address for NAT networking...
==> master: Checking if box 'centos/7' version '2004.01' is up to date...
==> master: Setting the name of the VM: Otus_Kaneva_dz27_master_1696923464564_62277
==> master: Fixed port collision for 22 => 2222. Now on port 2200.
==> master: Clearing any previously set network interfaces...
==> master: Preparing network interfaces based on configuration...
    master: Adapter 1: nat
    master: Adapter 2: hostonly
==> master: Forwarding ports...
    master: 22 (guest) => 2200 (host) (adapter 1)
==> master: Running 'pre-boot' VM customizations...
==> master: Booting VM...
==> master: Waiting for machine to boot. This may take a few minutes...
    master: SSH address: 127.0.0.1:2200
    master: SSH username: vagrant
    master: SSH auth method: private key
    master:
    master: Vagrant insecure key detected. Vagrant will automatically replace
    master: this with a newly generated keypair for better security.
    master:
    master: Inserting generated public key within guest...
    master: Removing insecure key from the guest if it's present...
    master: Key inserted! Disconnecting and reconnecting using new SSH key...
==> master: Machine booted and ready!
==> master: Checking for guest additions in VM...
    master: No guest additions were detected on the base box for this VM! Guest
    master: additions are required for forwarded ports, shared folders, host only
    master: networking, and more. If SSH fails on this machine, please install
    master: the guest additions and repackage the box to continue.
    master:
    master: This is not an error message; everything may continue to work properly,
    master: in which case you may ignore this message.
==> master: Setting hostname...
==> master: Configuring and enabling network interfaces...
==> master: Rsyncing folder: /home/neva/Otus_Kaneva_dz27/ => /vagrant
==> master: Running provisioner: shell...
    master: Running: inline script
==> slave: Importing base box 'centos/7'...
==> slave: Matching MAC address for NAT networking...
==> slave: Checking if box 'centos/7' version '2004.01' is up to date...
==> slave: Setting the name of the VM: Otus_Kaneva_dz27_slave_1696923514155_19384
==> slave: Fixed port collision for 22 => 2222. Now on port 2201.
==> slave: Clearing any previously set network interfaces...
==> slave: Preparing network interfaces based on configuration...
    slave: Adapter 1: nat
    slave: Adapter 2: hostonly
==> slave: Forwarding ports...
    slave: 22 (guest) => 2201 (host) (adapter 1)
==> slave: Running 'pre-boot' VM customizations...
==> slave: Booting VM...
==> slave: Waiting for machine to boot. This may take a few minutes...
    slave: SSH address: 127.0.0.1:2201
    slave: SSH username: vagrant
    slave: SSH auth method: private key
    slave:
    slave: Vagrant insecure key detected. Vagrant will automatically replace
    slave: this with a newly generated keypair for better security.
    slave:
    slave: Inserting generated public key within guest...
    slave: Removing insecure key from the guest if it's present...
    slave: Key inserted! Disconnecting and reconnecting using new SSH key...
==> slave: Machine booted and ready!
==> slave: Checking for guest additions in VM...
    slave: No guest additions were detected on the base box for this VM! Guest
    slave: additions are required for forwarded ports, shared folders, host only
    slave: networking, and more. If SSH fails on this machine, please install
    slave: the guest additions and repackage the box to continue.
    slave:
    slave: This is not an error message; everything may continue to work properly,
    slave: in which case you may ignore this message.
==> slave: Setting hostname...
==> slave: Configuring and enabling network interfaces...
==> slave: Rsyncing folder: /home/neva/Otus_Kaneva_dz27/ => /vagrant
==> slave: Running provisioner: shell...
    slave: Running: inline script

neva@Uneva:~/Otus_Kaneva_dz27$ vagrant status
Current machine states:

master                    running (virtualbox)
slave                     running (virtualbox)
```

Устанавливаем percona-server:

```
neva@Uneva:~/Otus_Kaneva_dz27$ vagrant ssh master
[vagrant@master ~]$ sudo -i
[root@master ~]# yum install https://repo.percona.com/yum/percona-release-latest.noarch.rpm -y
Failed to set locale, defaulting to C
Loaded plugins: fastestmirror
percona-release-latest.noarch.rpm                                                                                                                                                                                     |  20 kB  00:00:00
Examining /var/tmp/yum-root-dytJSv/percona-release-latest.noarch.rpm: percona-release-1.0-27.noarch
Marking /var/tmp/yum-root-dytJSv/percona-release-latest.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package percona-release.noarch 0:1.0-27 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================================================================================================================================================
 Package                                                   Arch                                             Version                                           Repository                                                                Size
=============================================================================================================================================================================================================================================
Installing:
 percona-release                                           noarch                                           1.0-27                                            /percona-release-latest.noarch                                            32 k

Transaction Summary
=============================================================================================================================================================================================================================================
Install  1 Package

Total size: 32 k
Installed size: 32 k
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : percona-release-1.0-27.noarch                                                                                                                                                                                             1/1
* Enabling the Percona Original repository
<*> All done!
* Enabling the Percona Release repository
<*> All done!
The percona-release package now contains a percona-release script that can enable additional repositories for our newer products.

For example, to enable the Percona Server 8.0 repository use:

  percona-release setup ps80

Note: To avoid conflicts with older product versions, the percona-release setup command may disable our original repository for some products.

For more information, please visit:
  https://www.percona.com/doc/percona-repo-config/percona-release.html

  Verifying  : percona-release-1.0-27.noarch                                                                                                                                                                                             1/1

Installed:
  percona-release.noarch 0:1.0-27

Complete!

[root@master ~]# percona-release setup ps57
* Disabling all Percona Repositories
* Enabling the Percona Server 5.7 repository
* Enabling the Percona XtraBackup 2.4 repository
[root@master ~] yum install Percona-Server-server-57

...

Installed:
  Percona-Server-server-57.x86_64 0:5.7.43-47.1.el7                                                                 Percona-Server-shared-compat-57.x86_64 0:5.7.43-47.1.el7

Dependency Installed:
  Percona-Server-client-57.x86_64 0:5.7.43-47.1.el7   Percona-Server-shared-57.x86_64 0:5.7.43-47.1.el7   libaio.x86_64 0:0.3.109-13.el7          net-tools.x86_64 0:2.0-0.25.20131004git.el7    perl.x86_64 4:5.16.3-299.el7_9
  perl-Carp.noarch 0:1.26-244.el7                     perl-Encode.x86_64 0:2.51-7.el7                     perl-Exporter.noarch 0:5.68-3.el7       perl-File-Path.noarch 0:2.09-2.el7             perl-File-Temp.noarch 0:0.23.01-3.el7
  perl-Filter.x86_64 0:1.49-3.el7                     perl-Getopt-Long.noarch 0:2.40-3.el7                perl-HTTP-Tiny.noarch 0:0.033-3.el7     perl-PathTools.x86_64 0:3.40-5.el7             perl-Pod-Escapes.noarch 1:1.04-299.el7_9
  perl-Pod-Perldoc.noarch 0:3.20-4.el7                perl-Pod-Simple.noarch 1:3.28-4.el7                 perl-Pod-Usage.noarch 0:1.63-3.el7      perl-Scalar-List-Utils.x86_64 0:1.27-248.el7   perl-Socket.x86_64 0:2.010-5.el7
  perl-Storable.x86_64 0:2.45-3.el7                   perl-Text-ParseWords.noarch 0:3.29-4.el7            perl-Time-HiRes.x86_64 4:1.9725-3.el7   perl-Time-Local.noarch 0:1.2300-2.el7          perl-constant.noarch 0:1.27-2.el7
  perl-libs.x86_64 4:5.16.3-299.el7_9                 perl-macros.x86_64 4:5.16.3-299.el7_9               perl-parent.noarch 1:0.225-244.el7      perl-podlators.noarch 0:2.5.1-3.el7            perl-threads.x86_64 0:1.87-4.el7
  perl-threads-shared.x86_64 0:1.43-6.el7

Replaced:
  mariadb-libs.x86_64 1:5.5.65-1.el7

Complete!
```

Выполняем эти же действия на ВМ slave.
По умолчанию Percona хранит файлы конфигурации в /etc/my.cnf и  /etc/my.cnf.d/, данные хранятся в /var/lib/mysql.
Копируем конфиги из /vagrant/conf.d в /etc/my.cnf.d/

```
[root@master ~]# cp /vagrant/conf/conf.d/* /etc/my.cnf.d/
[root@slave ~]#  cp /vagrant/conf/conf.d/* /etc/my.cnf.d/
[root@slave ~]# ls  /etc/my.cnf.d/
01-base.cnf  02-max-connections.cnf  03-performance.cnf  04-slow-query.cnf  05-binlog.cnf  mysql-clients.cnf
```

После этого можно запустить службу на обеих ВМ:

```
[root@master etc]# systemctl start mysql
[root@master etc]# systemctl status mysql
● mysqld.service - MySQL Server
   Loaded: loaded (/usr/lib/systemd/system/mysqld.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2023-10-10 08:09:09 UTC; 6s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
  Process: 4139 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid $MYSQLD_OPTS (code=exited, status=0/SUCCESS)
  Process: 4082 ExecStartPre=/usr/bin/mysqld_pre_systemd (code=exited, status=0/SUCCESS)
 Main PID: 4142 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─4142 /usr/sbin/mysqld --daemonize --pid-file=/var/run/mysqld/mysqld.pid
```

При установке Percona автоматически генерирует пароль для пользователя root и кладет его в файл /var/log/mysqld.log:

```
[root@master vagrant]# cat /var/log/mysqld.log | grep 'root@localhost:' | awk '{print $11}'
!?_8KykOt.)Q

[root@slave ~]# cat /var/log/mysqld.log | grep 'root@localhost:' | awk '{print $11}'
_BpC9yOaqhn?
```

Подключаемся к mysql и меняем пароль для доступа к полному функционалу:

```
[root@slave ~]# mysql -uroot -p'_BpC9yOaqhn?'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.43-47-log

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> ALTER USER root@localhost IDENTIFIED BY 'Qwerty#####123';
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye
[root@slave ~]# mysql -uroot -p'Qwerty#####123'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.43-47-log Percona Server (GPL), Release 47, Revision ff1a4d42212

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Репликацию будем настраивать с использованием GTID.
Следует обратить внимание, что атрибут server-id на мастер-сервере должен обязательно отличаться от server-id слейв-сервера. Проверить, какая переменная установлена в текущий момент можно следующим образом:

```
mysql>  SELECT @@server_id;
+-------------+
| @@server_id |
+-------------+
|           1 |
+-------------+
1 row in set (0.00 sec)
```

Убеждаемся, что GTID включен:

```
mysql> SHOW VARIABLES LIKE 'gtid_mode';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| gtid_mode     | ON    |
+---------------+-------+
1 row in set (0.02 sec)
```

Создадим тестовую базу bet и загрузим в нее дамп и проверим:

```
mysql> CREATE DATABASE bet;
Query OK, 1 row affected (0.00 sec)

[root@master vagrant]# mysql -uroot -p -D bet < /vagrant/bet.dmp
Enter password:
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
[root@master vagrant]# mysql -uroot -p -D bet < /vagrant/bet.dmp
Enter password:
[root@master vagrant]# mysql -uroot -p'Qwerty#####123'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5
Server version: 5.7.43-47-log Percona Server (GPL), Release 47, Revision ff1a4d42212

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> USE bet;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> SHOW TABLES;
+------------------+
| Tables_in_bet    |
+------------------+
| bookmaker        |
| competition      |
| events_on_demand |
| market           |
| odds             |
| outcome          |
| v_same_event     |
+------------------+
7 rows in set (0.00 sec)
```

Создадим пользователя для репликации и даем ему права на эту самую репликацию:

```
mysql> CREATE USER 'repl'@'%' IDENTIFIED BY '!OtusLinux2018';
Query OK, 0 rows affected (0.01 sec)

mysql> SELECT user,host FROM mysql.user where user='repl';
+------+------+
| user | host |
+------+------+
| repl | %    |
+------+------+
1 row in set (0.00 sec)

mysql> GRANT REPLICATION SLAVE ON *.* TO 'repl'@'%' IDENTIFIED BY '!OtusLinux2018';
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

Дампим базу для последующего залива на слэйв и игнорируем таблицы по заданию:

```
[root@master vagrant]# mysqldump --all-databases --triggers --routines --master-data --ignore-table=bet.events_on_demand --ignore-table=bet.v_same_event -uroot -p > master.sql
Enter password:
Warning: A partial dump from a server that has GTIDs will by default include the GTIDs of all transactions, even those that changed suppressed parts of the database. If you don't want to restore GTIDs, pass --set-gtid-purged=OFF. To make a complete dump, pass --all-databases --triggers --routines --events.
```

На этом настройка Master-а завершена. Файл дампа нужно залить на слейв.

```
[root@master vagrant]# scp master.sql vagrant@192.168.11.151:/home/vagrant
vagrant@192.168.11.151's password:
master.sql  
```

Проверяем, что конфиги мы скопировали:

```
[root@slave vagrant]# ls /etc/my.cnf.d/
01-base.cnf  02-max-connections.cnf  03-performance.cnf  04-slow-query.cnf  05-binlog.cnf
```

Правим в /etc/my.cnf.d/01-base.cnf директиву server-id = 2

```
[root@slave my.cnf.d]# vi /etc/my.cnf.d/01-base.cnf

[mysqld]
pid-file=/var/run/mysqld/mysqld.pid
log-error=/var/log/mysqld.log
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
symbolic-links=0

server-id = 2
innodb_file_per_table = 1
skip-name-resolve
```

Перезапускаем mysqld и проверяем параметр server-id

```
[root@slave my.cnf.d]# systemctl restart mysqld
[root@slave my.cnf.d]# mysql -uroot -p'Qwerty#####123'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.43-47-log Percona Server (GPL), Release 47, Revision ff1a4d42212

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> SELECT @@server_id;
+-------------+
| @@server_id |
+-------------+
|           2 |
+-------------+
1 row in set (0.00 sec)
```

Раскомментируем в /etc/my.cnf.d/05-binlog.cnf строки:
#replicate-ignore-table=bet.events_on_demand
#replicate-ignore-table=bet.v_same_event
Таким образом указываем таблицы, которые будут игнорироваться при репликации.

```
[root@slave my.cnf.d]# vi /etc/my.cnf.d/05-binlog.cnf
[mysqld]
log-bin = mysql-bin
expire-logs-days = 7
max-binlog-size = 16M
binlog-format = "MIXED"

# GTID replication config
log-slave-updates = On
gtid-mode = On
enforce-gtid-consistency = On

# Эта часть только для слэйва - исключаем репликацию таблиц
replicate-ignore-table=bet.events_on_demand
replicate-ignore-table=bet.v_same_event
```

Заливаем дамп мастера и убеждаемся, что база есть и она без лишних таблиц:

```
[root@slave vagrant]# cp master.sql /mnt/master.sql
[root@slave vagrant]# mysql -uroot -p'Qwerty#####123'
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.43-47-log Percona Server (GPL), Release 47, Revision ff1a4d42212

Copyright (c) 2009-2023 Percona LLC and/or its affiliates
Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
mysql> SOURCE /mnt/master.sql
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected, 1 warning (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1 row affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)
Statement prepared

Query OK, 1 row affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
Statement prepared

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 1 row affected (0.00 sec)

Database changed
Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.03 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 50 rows affected (0.00 sec)
Records: 50  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 909 rows affected (0.04 sec)
Records: 909  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1806 rows affected (0.02 sec)
Records: 1806  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 660 rows affected (0.21 sec)
Records: 660  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 73 rows affected (0.00 sec)
Records: 73  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 8 rows affected (0.00 sec)
Records: 8  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1 row affected (0.04 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 6 rows affected (0.00 sec)
Records: 6  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 2 rows affected (0.00 sec)
Records: 2  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 1 row affected (0.00 sec)

Database changed
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 335 rows affected (0.03 sec)
Records: 335  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 7 rows affected (0.01 sec)
Records: 7  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 345 rows affected (0.01 sec)
Records: 345  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.01 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.02 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 15 rows affected (0.01 sec)
Records: 15  Duplicates: 0  Warnings: 0

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

ERROR 1840 (HY000): @@GLOBAL.GTID_PURGED can only be set when @@GLOBAL.GTID_EXECUTED is empty.
Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)
Statement prepared

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected, 1 warning (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

Query OK, 0 rows affected (0.00 sec)

mysql> SHOW DATABASES LIKE 'bet';
+----------------+
| Database (bet) |
+----------------+
| bet            |
+----------------+
1 row in set (0.00 sec)

mysql> USE bet;
Database changed
mysql> SHOW TABLES;
+---------------+
| Tables_in_bet |
+---------------+
| bookmaker     |
| competition   |
| market        |
| odds          |
| outcome       |
+---------------+
5 rows in set (0.00 sec)
```

таблиц v_same_event и events_on_demand нет
подключаем и запускаем слейв, предварительно удалив пользователя repl, т.к. его присутствие в БД не дает запустить репликацию. Решение найдено здесь: https://it-lux.ru/mysql-%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0-1396-error-operation-create-user/

```
mysql> CHANGE MASTER TO MASTER_HOST = "192.168.11.150", MASTER_PORT = 3306, MASTER_USER = "repl", MASTER_PASSWORD = "!OtusLinux2018", MASTER_AUTO_POSITION = 1;
Query OK, 0 rows affected, 2 warnings (0.01 sec)

mysql> START SLAVE;
Query OK, 0 rows affected (0.00 sec)

mysql> delete from mysql.user where user='repl';
Query OK, 1 row affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> start slave;
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 192.168.11.150
                  Master_User: repl
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000002
          Read_Master_Log_Pos: 119552
               Relay_Log_File: slave-relay-bin.000003
                Relay_Log_Pos: 454
        Relay_Master_Log_File: mysql-bin.000002
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
           Replicate_Do_DB:
          Replicate_Ignore_DB:
           Replicate_Do_Table:
       Replicate_Ignore_Table: bet.events_on_demand,bet.v_same_event
      Replicate_Wild_Do_Table:
  Replicate_Wild_Ignore_Table:
                   Last_Errno: 0
                   Last_Error:
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 119552
              Relay_Log_Space: 701
              Until_Condition: None
               Until_Log_File:
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File:
           Master_SSL_CA_Path:
              Master_SSL_Cert:
            Master_SSL_Cipher:
               Master_SSL_Key:
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error:
               Last_SQL_Errno: 0
               Last_SQL_Error:
  Replicate_Ignore_Server_Ids:
             Master_Server_Id: 1
                  Master_UUID: 47b567de-6744-11ee-892d-5254004d77d3
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind:
      Last_IO_Error_Timestamp:
     Last_SQL_Error_Timestamp:
               Master_SSL_Crl:
           Master_SSL_Crlpath:
           Retrieved_Gtid_Set: 47b567de-6744-11ee-892d-5254004d77d3:1-39
            Executed_Gtid_Set: 47b567de-6744-11ee-892d-5254004d77d3:1:3-39,
882f5deb-6745-11ee-be80-5254004d77d3:1
                Auto_Position: 1
         Replicate_Rewrite_DB:
                 Channel_Name:
           Master_TLS_Version:
1 row in set (0.01 sec)

```

Проверим репликацию в действии. На мастере:

```
mysql> USE bet;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> INSERT INTO bookmaker (id,bookmaker_name) VALUES(1,'1xbet');
Query OK, 1 row affected (0.00 sec)

mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 sec)

```

На слейве:

```
mysql> SELECT * FROM bookmaker;
+----+----------------+
| id | bookmaker_name |
+----+----------------+
|  1 | 1xbet          |
|  4 | betway         |
|  5 | bwin           |
|  6 | ladbrokes      |
|  3 | unibet         |
+----+----------------+
5 rows in set (0.00 sec)
```

В binlog-ах на cлейве также видно последнее изменение, туда же он пишет информацию о GTID:

```
[root@slave mysql]# mysqlbinlog slave-relay-bin.000005
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=1*/;
/*!50003 SET @OLD_COMPLETION_TYPE=@@COMPLETION_TYPE,COMPLETION_TYPE=0*/;
DELIMITER /*!*/;
# at 4
#231010 12:54:17 server id 2  end_log_pos 123 CRC32 0xb21523f9  Start: binlog v 4, server v 5.7.43-47-log created 231010 12:54:17
# This Format_description_event appears in a relay log and was generated by the slave thread.
# at 123
#231010 12:54:17 server id 2  end_log_pos 194 CRC32 0x08b13ff0  Previous-GTIDs
# 47b567de-6744-11ee-892d-5254004d77d3:1-39
# at 194
#700101  0:00:00 server id 1  end_log_pos 0 CRC32 0xf1a55fa5    Rotate to mysql-bin.000002  pos: 4
# at 241
#231010  8:09:09 server id 1  end_log_pos 123 CRC32 0x0f73318c  Start: binlog v 4, server v 5.7.43-47-log created 231010  8:09:09
BINLOG '
pQYlZQ8BAAAAdwAAAHsAAAAAAAQANS43LjQzLTQ3LWxvZwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
AAAAAAAAAAAAAAAAAAAAAAAAEzgNAAgAEgAEBAQEEgAAXwAEGggAAAAICAgCAAAACgoKKioAEjQA
AYwxcw8=
'/*!*/;
# at 360
#231010 12:54:17 server id 0  end_log_pos 407 CRC32 0x41d51f0a  Rotate to mysql-bin.000002  pos: 154
# at 407
#231010 12:54:17 server id 0  end_log_pos 454 CRC32 0x429dbc4e  Rotate to mysql-bin.000002  pos: 119552
# at 454
#231010 12:59:06 server id 1  end_log_pos 119617 CRC32 0x08760f3c       GTID    last_committed=39       sequence_number=40      rbr_only=no
SET @@SESSION.GTID_NEXT= '47b567de-6744-11ee-892d-5254004d77d3:40'/*!*/;
# at 519
#231010 12:59:06 server id 1  end_log_pos 119690 CRC32 0x1b2e8eb0       Query   thread_id=11    exec_time=0     error_code=0
SET TIMESTAMP=1696942746/*!*/;
SET @@session.pseudo_thread_id=11/*!*/;
SET @@session.foreign_key_checks=1, @@session.sql_auto_is_null=0, @@session.unique_checks=1, @@session.autocommit=1/*!*/;
SET @@session.sql_mode=1436549152/*!*/;
SET @@session.auto_increment_increment=1, @@session.auto_increment_offset=1/*!*/;
/*!\C utf8 *//*!*/;
SET @@session.character_set_client=33,@@session.collation_connection=33,@@session.collation_server=8/*!*/;
SET @@session.lc_time_names=0/*!*/;
SET @@session.collation_database=DEFAULT/*!*/;
BEGIN
/*!*/;
# at 592
#231010 12:59:06 server id 1  end_log_pos 119817 CRC32 0x40602eea       Query   thread_id=11    exec_time=0     error_code=0
use `bet`/*!*/;
SET TIMESTAMP=1696942746/*!*/;
INSERT INTO bookmaker (id,bookmaker_name) VALUES(1,'1xbet')
/*!*/;
# at 719
#231010 12:59:06 server id 1  end_log_pos 119848 CRC32 0x74b2d276       Xid = 666
COMMIT/*!*/;
SET @@SESSION.GTID_NEXT= 'AUTOMATIC' /* added by mysqlbinlog */ /*!*/;
DELIMITER ;
# End of log file
/*!50003 SET COMPLETION_TYPE=@OLD_COMPLETION_TYPE*/;
/*!50530 SET @@SESSION.PSEUDO_SLAVE_MODE=0*/;
```

