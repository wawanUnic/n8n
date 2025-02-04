# Устанавливаем менеджер сервера базы данных
Работаем от root.

0a. apt update

0b. apt install php-fpm

0c. apt install php-mysql

0d. apt-get install php-mbstring (apt install php-mbstring)

0e. php -v (8.3.6)

1. apt install nginx (1.10.3)

2. systemctl enable nginx

3. systemctl start nginx

4. systemctl status nginx

5. apt install phpmyadmin (никакой из двух серверов не ставим)

6. /etc/nginx/sites-available/my_file:

```
server_names_hash_bucket_size 64;
location /phpmyadmin/ {
        alias /var/www/html/phpmyadmin/;
        index index.php;
        location ~ \.php$ {
            fastcgi_read_timeout 360;
            fastcgi_pass unix:/var/run/php/php-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }
```

7. /etc/nginx/nginx.conf

```
http {client_max_body_size 30M; ...
```

8. nginx -t

9. systemctl restart nginx

10. /etc/php/8.x/fpm/php.ini

```
upload_max_filesize 30M
post_max_size 30M
max_execution_time 300
max_input_time 300
```

11. systemctl restart php8.3-fpm
