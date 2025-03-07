# Устанавливаем сервер базы данных
Работаем от root. База данных установлена прямо на host (без docker)

Необходимо отключить ufw (ufw disable, reboot, ufw status)! Порт 3306 должен быть открыт!

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

При создании пользователя нужно всегда указывать доступ %, иначе N8N не сможет войти под его именем!
```
CREATE USER 'master01'@'%' IDENTIFIED BY 'pass01';
GRANT ALL PRIVILEGES ON *.* TO 'master01'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT
-- Создать пользователей, базы данных, таблицы через HeidiSQL
```

7. nano /etc/mysql/mariadb.conf.d/50-server.cnf

```
bind-address = 0.0.0.0
```

7.5 nano /etc/mysql/my.cnf

```
[mysqld]
default-time-zone='+01:00'
```

8. service mysql restart

9. Если используется n8n в Docker в Linux, то нужно использовать флаг --add-host для сопоставления при запуске контейнера

```
docker run -d --restart unless-stopped -it \
--name n8n \
--add-host host.docker.internal:host-gateway \
-p 5678:5678 \
-e N8N_HOST="nero-n8n.duckdns.org" \
-e WEBHOOK_TUNNEL_URL="https://nero-n8n.duckdns.org/" \
-e WEBHOOK_URL="https://nero-n8n.duckdns.org/" \
-v ~/.n8n:/root/.n8n \
n8nio/n8n
```

При настройке учетных данных MySQL в качестве адреса хоста вместо localhost необходимо указать host.docker.internal

10. apt install php-fpm php-mbstring php-zip php-gd php-json php-curl

11. apt install phpmyadmin

```
no server
no password
```

12. nano /etc/php/8.1/fpm/php.ini

```
cgi.fix_pathinfo=0
mbstring.func_overload=0
```

13. nano /etc/nginx/sites-available/nero2auto.conf

```
server {
    server_name nero2auto.duckdns.org;
    location /phpmyadmin/ {
        alias /usr/share/phpmyadmin/;
        index index.php;
        location ~ \.php$ {
            fastcgi_read_timeout 360;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }
```

14. ln -s /etc/nginx/sites-available/nero2auto.conf /etc/nginx/sites-enabled/

15. nginx -t

16. systemctl restart nginx
