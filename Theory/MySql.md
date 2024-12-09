# MySQL

## Debian

```bash
sudo apt install default-mysql-server
# on debian service is started automatically
sudo ss -tlnp # check if listening to interface
```

## Enterprise linux

```bash
sudo dnf install -y mysql-server # maria db also available mariadb-server
sudo systemctl enable --now mysqld.service
sudo ss -tlnp # check if listening to interface
```

## Configuration

> Secure installation

```bash
sudo mysql_secure_installation # set root password, remove anonymous users, disallow root login remotely, remove test database, reload privilege tables
```

> A mysql user and group has been made on the system

---

## MySQL client

```bash
mysql -u root -p # login as root if unix_socket is not used

sudo mysql # login if unix_socket is used
```

> `~/.my.cnf`
> this allows you to login without password

```bash
[client]
user=root
password=your_password
```

```sql
-- show databases
SHOW DATABASES;

-- create database
CREATE DATABASE <database_name>;

-- use database
USE <database_name>;

-- create user
GRANT ALL ON <database_name>.* to '<username>'@'localhost' IDENTIFIED BY '<password>';

-- create user access from any host
CREATE USER 'kevin'@'%' IDENTIFIED BY 'hunter2';
GRANT ALL ON famouspeople.* TO 'kevin'@'%';

-- describe table
DESCRIBE <table_name>;

-- show users
SELECT user, host FROM user;
-- host = localhost means only local access
-- host = % means any host

-- delete database
DROP DATABASE IF EXISTS <database_name>;
```

```bash
# backup
# export database
mysqldump -u root -p <database_name> > <database_name>.sql

# import database
mysql -u root -p <database_name> < <database_name>.sql
```

---

> non-interactive mode

```bash
mysql -u root -p <password> -e "SHOW DATABASES;"


mysql -u<user> -p<password> <databasename> << _EOF_
SHOW TABLES;
DESCRIBE people;
_EOF_
```

---

> Access database over network
> make sure service is listening to the network interface `ss -tlnp`
>
> network interface:
>
> - 127.0.0.1: only local access
> - 0.0.0.0 or \*: any host
> - specific interface IP: only that interface

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf # debian

sudo nano /etc/my.cnf.d/mariadb-server.cnf # enterprise linux

# change bind-address
bind-address = <ip>

sudo systemctl restart <mysqld/mariadb>.service

# firewall
sudo firewall-cmd --add-service=mysql --permanent # enterprise linux
sudo ufw allow mysql # debian
```
