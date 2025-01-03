# Устанавливаем сервер базы данных
Работаем от root. База данных установлена прямо на host (без docker)

Необходимо отключить ufw (ufw disable, reboot, ufw status)! 

1. apt install mariadb-server

2. systemctl enable --now mariadb

3. systemctl start mariadb

4. systemctl status mariadb

5. mysql_secure_installation

```
Enter current password for root (enter for none):
Switch to unix_socket authentication [Y/n] n
Change the root password? [Y/n] n
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] n
Reload privilege tables now? [Y/n] y
```

6. mysql -u root -p

```
CREATE USER 'master01'@'%' IDENTIFIED BY 'pass01';
GRANT ALL PRIVILEGES ON *.* TO 'master01'@'%';
-- Создать базу данных n8n_base01
CREATE USER 'n8n_user01'@'%' IDENTIFIED BY 'n8n_pass01';
GRANT ALL PRIVILEGES ON n8n_base01.* TO 'n8n_user01'@'%';
FLUSH PRIVILEGES;
EXIT
```

7. nano /etc/mysql/mariadb.conf.d/50-server.cnf

```
bind-address = 0.0.0.0
```

8. service mysql restart
