# Install-PostgreSQL18-on-Rocklinux9-or-10
=============================

sudo dnf -qy module disable postgresql

# for Rockylinux 9
`sudo dnf -qy module disable postgresql`
`sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm`

# for Rockylinux 10
`sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm`

`sudo dnf install -y postgresql18-server postgresql18`

`/usr/pgsql-18/bin/postgresql-18-setup initdb`

`sudo systemctl enable postgresql-18`
`sudo systemctl start postgresql-18`
`sudo systemctl status postgresql-18`

`sudo su - postgres`
`psql --version`


psql:
\password postgres
*enter password*
*verify password*
\q



sudo nano /var/lib/pgsql/18/data/pg_hba.conf  
```
host    all             all             0.0.0.0/0            scram-sha-256
```

sudo nano /var/lib/pgsql/18/data/postgresql.conf
```
listen_addresses = '*'
```
sudo firewall-cmd --add-service=postgresql --permanent
sudo firewall-cmd --add-port=5432/tcp --permanent
sudo firewall-cmd --reload



===================================================================================
in pgAdmin 4 :
https://www.pgadmin.org/docs/pgadmin4/8.14/pgagent_install.html















PGAGENT (INSTALL)
==================
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm

sudo dnf install -y pgagent_18
sudo systemctl enable --now pgagent_18
sudo nano  /etc/pgagent/pgagent_18.conf
``` (Default)
DBNAME=postgres
DBUSER=postgres
DBHOST=127.0.0.1
DBPORT=5432
LOGFILE=/var/log/pgagent_18.log
```
sudo cat  /var/log/pgagent_18.log

sudo nano /usr/lib/systemd/system/pgagent_18.service
``` (edit)
User=postgres
Group=postgres
```

sudo su - postgres
nano ~/.pgpass
``` (edit  host:port:db:user:pass)
127.0.0.1:5432:*:postgres:not123456
```
chmod 0600 ~/.pgpass
echo ~/.pgpass
exit 

