# Mysql

> Library reference
>
> [mysql homepage](https://www.mysql.com/)
>
> [mariadb homepage](https://mariadb.org/)

[TOC]

## Mysql in Win

> Library reference
>
> [mysql@5.7.24](https://dev.mysql.com/downloads/mysql/)

### Setup Mysql with zip package

```shell
# unzip package an add %MYSQL_HOME% variable to system enviroment path

# initialize without password
$ cd %MYSQL_HOME%

$ echo "
[mysqld]
port=3306
        
basedir=D:\\develop\\environment\\mysql-5.7.24-winx64
         
datadir=D:\\develop\\environment\\mysql-5.7.24-winx64\\data" 
> my.ini

$ cd bin

$ mysqld --initialize-insecure

$ mysqld --install

$ net start mysql

$ mysql -u root -p

$ set password = password('yourpass');

# import .sql file
mysql>source d:\develop\sample.sql
```

### Debug

```shell
# check the log
$ mysqld --console
# if can resolve, del the data folder and run
$ mysqld -remove
# then init again
```

## Mysql in Linux

### Install

```shell
$ yum install mariadb-server mariadb
$ systemctl enable mariadb
$ systemctl start mariadb
# 初次配置
$ mysql_secure_installation
```

### 主从配置

> Library reference
>
> [分库主从](https://blog.csdn.net/redstarofsleep/article/details/53539226)
>
> [全库主从](https://www.cnblogs.com/xiaocen/p/3702945.html)

master-node：192.168.33.129

slave-node：192.168.33.133

```shell
# 配置检查，确认版本号一致
mysql> select version();
```

> ![1555402417398](D:\develop\project\how-to-use-it\mysql\1555402417398.png)

```shell
# 配置 master-node 的配置文件 /etc/my.cnf
log-bin=/var/log/mariadb/master-bin
binlog_format=mixed
server-id=1

# 保存修改后重启服务
$ systemctl restart mariadb

# 给 slave-node 授权
mysql> GRANT REPLICATION SLAVE,REPLICATION CLIENT ON *.* TO 'root'@'192.168.33.133' IDENTIFIED BY 'root';
mysql > FLUSH PRIVILEGES;
```

```shell
# 配置 slave-node 的配置文件 /etc/my.cnf
relay-bin=/var/log/mariadb/relay-bin
binlog_format=mixed
server-id=2

# 保存修改后重启服务
$ systemctl restart mariadb

# 查看 master-node 状态
mysql> show master status;
```

> ![1555402990689](D:\develop\project\how-to-use-it\mysql\1555402990689.png)

```shell
# 指向 master-node
mysql > CHANGE MASTER TO master_host='192.168.33.129',master_user='root',master_password='root',master_log_file='master-bin.000001',master_log_pos=877302;

# 开始同步
mysql > start slave;
```

```mysql
# 相关命令
mysql > show master status;
mysql > show slave status\G;
mysql > stop slave;
mysql > reset slave;
```



### 异常处理案例

```shell
# 日志
/var/log/mariadb
# 报错处理，由于没用空间写入日志导致mysql无法启动，查看后发现是由于ibdata1文件过大
[Warning]: mysqld: Disk is full writing '/var/tmp/#sql_fasd.mad'
$ cd /var/lib/mysql
# 备份数据库
$ mysqldump -uroot -ppassword --all-databases --add-dorp-table > /root/all_mysql.sql
$ systemctl stop mariadb
$ vim /etc/my.cnf
# 在[mysqld]加增加
innodb_file_per_table=1
# 检验,成功为 on
> show variables like '%per_table%';
# 还原数据库
> source /root/all_mysql.sql
```

