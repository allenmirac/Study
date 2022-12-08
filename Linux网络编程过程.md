## Socket编程时recv函数错误：Transport endpoint is not connected的解决

int read_size = read(listen_fd, buf, sizeof(buf));

client_fd这个是客户端的socke_fd，填成了listen_fd就会出现这个问题

解决：

*   改为int read_size = read(client_fd, buf, sizeof(buf));



## Please input a string sended to server:dasf   	send error: Connection reset by peer	epoll_wait failed: Bad address

测试Epoll.h，Epoll.cpp出现

mirac@ubuntu:~/Desktop/Github/30dayMakeCppServer/code/day04$ ./client 

mirac@ubuntu:~/Desktop/Github/30dayMakeCppServer/code/day04$ ./server 5005

https://blog.csdn.net/xc_zhou/article/details/80950753

两个原因：

*   Socket一端连接已经关闭，但是其中一方还在发送数据（发送的第一个数据包引发该异常(Connect reset by peer)。
*   一端退出，但退出时并未关闭该连接，另一端如果在从连接中读数据则抛出该异常（Connection reset by peer）。

解决：

*   bzero(&events, sizeof(*events) * MAX_EVENTS);因为events是一个指针，但是传入了&events
*   改为bzero(events, sizeof(*events) * MAX_EVENTS);
*   但是又出现了新的错误：accept failed: Resource temporarily unavailable，继下

## accept failed: Resource temporarily unavailable

判断events[i].data.fd = serv_sock->getListen_fd()是不是服务器listen_fd时，使用了等号，

要使用双等号==，

解决：

*   改为events[i].data.fd == serv_sock->getListen_fd()

非阻塞模式下read、recv的使用问题：https://blog.csdn.net/qq276592716/article/details/7295131

## 使用Linux epoll模型，水平触发模式；当socket可写时，会不停的触发 socket 可写的事件，如何处理？

第一种最普遍的方式：

*   需要向 socket 写数据的时候才把 socket 加入 epoll ，等待可写事件。
*   接受到可写事件后，调用 write 或者 send 发送数据。。。
*   当所有数据都写完后，把 socket 移出 epoll。

这种方式的缺点是，即使发送很少的数据，也要把 socket 加入 epoll，写完后在移出 epoll，有一定操作代价。

一种改进的方式：

*   开始不把 socket 加入 epoll，需要向 socket 写数据的时候，直接调用 write 或者 send 发送数据。
*   如果返回 EAGAIN，把 socket 加入 epoll，在 epoll 的驱动下写数据，全部数据发送完毕后，再移出 epoll。


## 解决办法：git错误 error: failed to push some refs to 'https://github.com/

https://blog.csdn.net/dietime1943/article/details/85682688

出现原因：本地库与远程库不一致，

**解决办法**：使用命令行：

```git
git pull --rebase origin master
```

## Command 'curl' not found, but can be installed with:

**sudo 全称是switch user and do something**

## make: 'test' is up to date.

https://stackoverflow.com/questions/3931741/why-does-make-think-the-target-is-up-to-date

https://www.cnblogs.com/kex1n/p/8303619.html

PHONY 目标并非实际的文件名：只是在显式请求时执行命令的名字。有两种理由需要使用PHONY 目标：避免和同名文件冲突，改善性能。

所谓的PHONY这个单词就是伪造的意思，makefile中将.PHONY放在一个目标前就是指明这个目标是伪文件目标，如下：
.PHONY:clean

## Redefinition of default argument

https://www.codeproject.com/Questions/1226183/Redefinition-of-default-parameter-parameter

The preferred approach is specifying it only in function (or method) *declarations*.

## make: 'server' is up to date.

https://stackoverflow.com/questions/3931741/why-does-make-think-the-target-is-up-to-date

```makefile
.PHONY: all test clean
```

*A phony target is one that is not really the name of a file; rather it is just a name for a recipe to be executed when you make an explicit request.*

## 配置MySQL

```bash
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Current-Root-Password';
ERROR 1290 (HY000): The MySQL server is running with the --skip-grant-tables option so it cannot execute this statement
mysql> FLUSH PRIVILEGES;
Query OK, 0 rows affected (0.03 sec)

mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Current-Root-Password';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.02 sec)

mysql> set global validate_password_policy=0;
ERROR 1193 (HY000): Unknown system variable 'validate_password_policy'
mysql> set global validate_password.policy=0;
Query OK, 0 rows affected (0.00 sec)

mysql> flush privilige;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'privilige' at line 1
mysql> flush privilege;
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'privilege' at line 1
mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password.check_user_name    | ON    |
| validate_password.dictionary_file    |       |
| validate_password.length             | 8     |
| validate_password.mixed_case_count   | 1     |
| validate_password.number_count       | 1     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 1     |
+--------------------------------------+-------+
7 rows in set (0.01 sec)

mysql> set global validate_password.mixed_case_count=0;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password.number_count=3;
Query OK, 0 rows affected (0.00 sec)

mysql> set global validate_password.special_char_count=0;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW VARIABLES LIKE 'validate_password%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password.check_user_name    | ON    |
| validate_password.dictionary_file    |       |
| validate_password.length             | 8     |
| validate_password.mixed_case_count   | 0     |
| validate_password.number_count       | 3     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 0     |
+--------------------------------------+-------+
7 rows in set (0.00 sec)

mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'PASSWORD('123')' at line 1
mysql> set global validate_password.length=3;
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> UPDATE mysql.user set authentication_string=PASSWORD('123456') where user='root';
ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '('123456') where user='root'' at line 1
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY'123456';
Query OK, 0 rows affected (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;

mysql> quit
Bye
root@ubuntu:/home/mirac/Desktop/Github/WebServer# systemctl stop mysql
root@ubuntu:/home/mirac/Desktop/Github/WebServer# vim /etc/mysql/mysql.conf.d/mysqld.cnf
root@ubuntu:/home/mirac/Desktop/Github/WebServer# mysql
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)
root@ubuntu:/home/mirac/Desktop/Github/WebServer# systemctl start mysql
root@ubuntu:/home/mirac/Desktop/Github/WebServer# systemctl start mysql
root@ubuntu:/home/mirac/Desktop/Github/WebServer# systemctl start mysql
root@ubuntu:/home/mirac/Desktop/Github/WebServer# mysql
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
root@ubuntu:/home/mirac/Desktop/Github/WebServer# mysql -u -root -p
Enter password: 
ERROR 1045 (28000): Access denied for user '-root'@'localhost' (using password: YES)
root@ubuntu:/home/mirac/Desktop/Github/WebServer# vim /etc/mysql/mysql.conf.d/mysqld.cnf
root@ubuntu:/home/mirac/Desktop/Github/WebServer# systemctl restart mysql
root@ubuntu:/home/mirac/Desktop/Github/WebServer# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 7
Server version: 8.0.31-0ubuntu0.20.04.2 (Ubuntu)

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show variables like 'validate_password%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)

mysql> quit;
Bye
root@ubuntu:/home/mirac/Desktop/Github/WebServer# 

```

以上都没有配置成功

## Solution for 'ssh: connect to host github.com port 22: Connection timed out' error

https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794

https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port

```git
git remote -v
ssh -T git@github.com
// 改为使用443端口
```

