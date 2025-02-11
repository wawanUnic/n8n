1. apt update

2. apt install postgresql postgresql-contrib (150 Mb)

3. systemctl start postgresql.service

4. systemctl enable postgresql.service

5. systemctl status postgresql.service

6. sudo -i -u postgres

```
psql
SELECT version(); (>=13)
```

7. apt install postgresql-server-dev-XX (700 Mb)
 
```
cd /tmp
git clone --branch v0.8.0 https://github.com/pgvector/pgvector.git
cd pgvector
make
make install # may need sudo
```

8. sudo -i -u postgres (with sudo!)

```
psql
CREATE EXTENSION vector; # create the vector extension
SELECT * FROM pg_available_extensions;
SELECT * FROM pg_extension;
/dx
```

```
CREATE TABLE items (aaaa bigserial PRIMARY KEY, bbbb vector(3));
INSERT INTO items (bbbb) VALUES ('[1,2,3]'), ('[4,5,6]');
SELECT * FROM items ORDER BY bbbb <-> '[3,1,2]' LIMIT 5;
```

```
ALTER USER postgres WITH PASSWORD 'new_pass';
CREATE USER helloIam11Admin2 WITH PASSWORD 'ololo2';
ALTER USER helloIam11Admin2 WITH SUPERUSER;
CREATE DATABASE FBD2 OWNER helloIam11Admin2;
\du
\q
```

9. apt install nginx (1.18.0)

10. systemctl enable nginx

11. systemctl start nginx

12. systemctl status nginx

13. apt install phppgadmin php php-fpm php-pgsql

14. php --version (8.1.2)

15. nano /etc/nginx/sites-available/nero2auto.conf

```
server {
    listen 80;
    server_name nero2auto.duckdns.org;

    location /phppgadmin/ {
        alias /usr/share/phppgadmin/;
        index index.php;
        location ~ \.php$ {
            fastcgi_read_timeout 360;
            fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
            include fastcgi_params;
            fastcgi_param SCRIPT_FILENAME $request_filename;
        }
    }
}
```

16. ln -s /etc/nginx/sites-available/nero2auto.conf /etc/nginx/sites-enabled/

17. chown -R www-data:www-data /usr/share/phppgadmin

18. chmod -R 755 /usr/share/phppgadmin

19. nginx -t

20. systemctl restart nginx

21. apt install certbot python3-certbot-nginx

22. certbot --nginx -d nero-n8n.duckdns.org

23. nano /etc/phppgadmin/config.inc.php

```
$conf['extra_login_security'] = false;
```
