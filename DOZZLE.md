mkdir dozzle

docker run amir20/dozzle generate admin --password password --email test@email.net --name "John Doe" >> dozzle/users.yml

admin - admin
password - password
email - test@email.net
name - John Doe

удалить становившийся контейнер

docker run -d --restart unless-stopped --name dozzle -v /var/run/docker.sock:/var/run/docker.sock -v /root/dozzle:/data -p 8080:8080 amir20/dozzle:latest --auth-provider simple --auth-ttl 48h --enable-actions --no-analytics --base /health

/etc/nginx/sites-available/my_file:
```
 location /health {
        proxy_pass http://localhost:8080;
    }
``
