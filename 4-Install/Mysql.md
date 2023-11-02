- [1. Install MYSQL](#1-install-mysql)
  - [1.1. WSL 安装](#11-wsl-安装)
  - [1.2. 修改MYSQL密码](#12-修改mysql密码)
  - [1.3. 运行远程访问](#13-运行远程访问)
  - [1.4. MYSQL 命令](#14-mysql-命令)

# 1. Install MYSQL

![image.png](https://cdn.nlark.com/yuque/0/2022/png/21870146/1666143344877-517915fc-67c6-4b98-a67b-b56894ebdbf7.png#averageHue=%23faf9f9&clientId=uf8d9cc3a-a607-4&from=paste&height=720&id=uee7066d5&originHeight=900&originWidth=1600&originalType=binary&ratio=1&rotation=0&showTitle=false&size=69958&status=done&style=none&taskId=uedd50b96-9d68-462a-a2e3-a2400f9d138&title=&width=1280)

## 1.1. WSL 安装
```bash
sudo apt-get install mysql-server
```

安装完成后在shell输入mysql，会碰到这个问题：ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)，此时使用以下命令可解决：

```bash
sudo mkdir -p /var/run/mysqld
sudo chown mysql /var/run/mysqld/
sudo service mysql restart
```

mysql 默认没有密码，可以用以下命令进入

```bash
sudo mysql
```

同时mysql也提供了一个默认账号：

```bash
sudo cat /etc/mysql/debian.cnf
```

## 1.2. 修改MYSQL密码

```bash
alter user 'root'@'localhost' identified with mysql_native_password by '密码'; # 修改密码
flush privileges; # 刷新缓存
```

## 1.3. 运行远程访问

一般我们使用 navicat 访问，是需要开启允许远程访问的

```bash
use mysql;
select host,user from user; # 查看用户是否运行远程访问
```

发现root用户的访问权限是localhost,需要修改host为%，输入命令：

```bash
update user set host='%' where user='root';
flush privileges;
```

## 1.4. MYSQL 命令

```bash
service mysql status
service mysql start
service mysql stop
service mysql restart
```
