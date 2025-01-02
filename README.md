# n8n
Приведены инструкции по самостоятельному размещению n8n


## Настраиваем записи DNS

1. Убедитесь, что запись DNS A вашего домена указывает на IP-адрес вашего сервера

2. Если установлен фаервол, то разрешите порты 80, 443 и 5678 (зачем этот порт?)

## Устанавливаем Docker

1. apt update

2. apt install docker.io

3. systemctl start docker

4. systemctl enable docker


## Запускаем n8n в Docker (Будет работать только на localhost:5678. Без шифрования работать не будет!)

docker run -d --restart unless-stopped -it --name n8n -p 5678:5678 -e N8N_HOST="nero-n8n.duckdns.org/" -e WEBHOOK_TUNNEL_URL="https://nero-8n8.duckdns.org/" -e WEBHOOK_URL="https://nero-n8n.duckdns.org/" -v ~/.n8n:/root/.n8n n8nio/n8n


## Устанавливаем Nginx

1. apt install nginx


## Конфигурируем Nginx

1. nano /etc/nginx/sites-available/nero-n8n

server_names_hash_bucket_size 64;
server {
        listen 80;
        server_name nero-n8n.duckdns.org;
        location / {
            proxy_pass http://localhost:5678;
            proxy_http_version 1.1;
            chunked_transfer_encoding off;
            proxy_buffering off;
            proxy_cache off;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }
    }

3. ln -s /etc/nginx/sites-available/nero-n8n /etc/nginx/sites-enabled/

4. nginx -t

5. systemctl restart nginx


## Шифруем соединение (Будет отредактирован файл /etc/nginx/sites-available/nero-n8n)

1. apt install certbot python3-certbot-nginx

2. certbot --nginx -d nero-n8n.duckdns.org


## Отладка

Если у вас возникнут проблемы с Nginx, обратитесь к журналам, расположенным по адресу /var/log/nginx/error.log

Если есть проблемы, связанные с Docker, убедитесь, что служба Docker запущена: systemctl status docker


## How to update n8n:

1. Сделайте резервную копию ~/.n8n:/home/node/.n8n Чтобы создать резервную копию, вы можете скопировать ~/.n8n:/home/node/.n8n в локальную или другую директорию на той же виртуальной машине еще до удаления контейнера. А затем, после обновления и раскрутки нового контейнера, если вы видите, что данные теряются, вы можете заменить ~/.n8n:/home/node/.n8n на тот, который вы сохранили ранее

2. Убедитесь, что экземпляр n8n использует постоянный том или сопоставленный каталог для хранения данных. Это очень важно, поскольку рабочие процессы, учетные записи пользователей и конфигурации хранятся в файле базы данных (обычно database.sqlite), который должен находиться в каталоге, который остается нетронутым даже после удаления контейнера. В вашем docker-compose.yml должно быть что-то вроде этого:

volumes:
- ~/.n8n:/home/node/.n8n
Это сопоставление гарантирует, что каталог .n8n на хост-компьютере будет использоваться для хранения данных, сохраняя рабочие процессы и конфигурации при обновлении контейнера

3. Когда вы останавливаетесь и удаляете контейнер n8n, вы удаляете только сам экземпляр контейнера, а не данные, хранящиеся в постоянном томе. Если объем настроен правильно, ваши рабочие процессы и учетные записи не должны быть затронуты

4. Но чтобы избежать любой вероятности потери данных, вы должны сделать резервную копию ~/.n8n:/home/node/.n8n перед удалением контейнера
