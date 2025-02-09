1. mkdir dozzle

2. docker run --name dozzle amir20/dozzle:latest generate admin --password password --email test@email.net --name "John Doe" >> dozzle/users.yml

admin - admin
password - password
email - test@email.net
name - John Doe

3. docker rm dozzle (это контейнер должен был остановиться сам)

4. docker run -d --restart unless-stopped --name dozzle -v /var/run/docker.sock:/var/run/docker.sock -v /root/dozzle:/data -p 8080:8080 amir20/dozzle:latest --auth-provider simple --auth-ttl 48h --enable-actions --no-analytics --base /health

5. /etc/nginx/sites-available/my_file:

```
 location /health {
        proxy_pass http://localhost:8080;
    }
```

6. nginx -t

7. systemctl restart nginx
