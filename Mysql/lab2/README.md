# MySQL Enterprise Edition Lab2
# Introduction MySQL 
## MySQL Innovation: 5.7 --> 8.0 
![](img/01.PNG) 
## MySQL History 
![](img/02.PNG)

# MySQLComponents
## Create new MySQL Instance
```
cd /opt/download/mysql/mysql8.0/tar
tar -zxvf *.* 
mkdir /lab
cd /lab
ln -s /opt/download/mysql/mysql8.0/tar/mysql-commercial-8.0.18-el7-x86_64 mysql
chown mysql.mysql /lab
sudo -H -u mysql bash -c '/usr/sbin/mysqld --initialize-insecure --basedir=/lab/mysql/bin/ --datadir=/lab/mysql_home01'
``` 
## Create mysql configure files 
```
cat << EOF > /lab/mysql_home01/my.cfg
[mysqld]

sql_mode=''

# server configuration
datadir=/lab/mysql_home01/
basedir=/lab/mysql/bin/
plugin_dir=/usr/lib64/mysql/plugin/
lc-messages-dir=/usr/share/mysql-8.0/

port=3310
socket=/lab/mysql_home01/s1.sock
EOF
``` 


## MySQL Server 
```
sudo -u mysql /lab/mysql/bin/mysqld --defaults-file=/lab/mysql_home01/my.cfg 2>&1 &>/dev/null &
```
### Connect to mysqlserver 
```
mysql -u root -h 127.0.0.1 -P3310
```
mysql>
```
shutdown;
```
### Started with mysqld
```
sudo -u mysql /lab/mysql/bin/mysqld --defaults-file=/lab/mysql_home01/my.cfg 2>&1 &>/dev/null &
```

### Started with mysqld_safe
```
sudo -u mysql /lab/mysql/bin/mysqld_safe --defaults-file=/lab/mysql_home01/my.cfg 2>&1 &>/dev/null &
```
### Connect to mysqlserver 

```
mysql -u root -h 127.0.0.1 -P3310
```
mysql>
```
show variables like 'general_log';
set global general_log = true;
```
open new terminal 
```
cd /lab/mysql_home01/
tail -f *.log
```

Go to mysql terminal
```
select 1;
restart;
show variables like 'general_log%';
set persist general_log = true;
restart;
```

## Storage Engines
### InnoDB
```
show engines;
show status like '%Innodb%';
show engine innodb status\G
```
### Performance_schema
```
use performance_schema
show tables;
select * from events_statements_summary_by_digest order by COUNT_STAR desc limit 2\G

use sys
show tables;
select * from statement_analysis order by exec_count desc limit 2\G

\! mysqlslap --concurrency=5 --iterations=20 --number-int-cols=2 --number-char-cols=3 --auto-generate-sql

select * from statement_analysis order by exec_count desc limit 2\G
```