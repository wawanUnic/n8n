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
CREATE USER helloIam11Admin2 WITH PASSWORD 'ololo2';
ALTER USER postgres WITH PASSWORD 'new_pass';
ALTER USER helloIam11Admin2 WITH SUPERUSER;
CREATE DATABASE FBD2 OWNER helloIam11Admin2;
\du
\q
```
