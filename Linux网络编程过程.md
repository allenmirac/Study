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

```bash
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

## Solution for 'ssh: connect to host github.com port 22: Connection timed out' error

https://gist.github.com/Tamal/1cc77f88ef3e900aeae65f0e5e504794

https://docs.github.com/en/authentication/troubleshooting-ssh/using-ssh-over-the-https-port

```git
git remote -v
ssh -T git@github.com
// 改为使用443端口
```

## 安装和配置MySQL

https://developer.aliyun.com/article/979250

```bash
root@ubuntu:/home/mirac/Desktop# apt-get install mysql-server
.....
正在处理用于 man-db (2.9.1-1) 的触发器 ...
正在处理用于 libc-bin (2.31-0ubuntu9.9) 的触发器 ...
root@ubuntu:/home/mirac/Desktop# systemctl status mysql
# 这个时候还没有设置密码可以直接登录
root@ubuntu:/home/mirac/Desktop# mysql
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.31-0ubuntu0.20.04.2 (Ubuntu)
......
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
Query OK, 0 rows affected (0.00 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.01 sec)

mysql> quit;
Bye
root@ubuntu:/home/mirac/Desktop# service mysql restart
root@ubuntu:/home/mirac/Desktop# mysql
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
root@ubuntu:/home/mirac/Desktop# mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.31-0ubuntu0.20.04.2 (Ubuntu)
......
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

## 【Git】让git忽略所有二进制文件的方法

[如何忽略没有后缀的文件](https://blog.csdn.net/zhangyk11/article/details/124151075#:~:text=%E6%88%91%E4%BB%AC%E5%8F%AA%E9%9C%80%E5%9C%A8.gitignore,%E6%95%B4%E4%B8%AA%E6%96%87%E4%BB%B6%E5%A4%B9%E5%8D%B3%E5%8F%AF%E3%80%82&text=%E5%85%B6%E4%B8%AD%E4%B8%BA%E4%BD%A0%E6%83%B3,push%E7%AD%89%E6%93%8D%E4%BD%9C%E5%8D%B3%E5%8F%AF%E3%80%82)
