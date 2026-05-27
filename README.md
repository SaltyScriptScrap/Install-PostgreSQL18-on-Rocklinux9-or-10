# Install-PostgreSQL18-on-Rocklinux9-or-10

check the video for visual guidance at https://youtu.be/5JPWacXr-Bg

disable default module in OS (Rockylinux 9 only?)
```
sudo dnf -qy module disable postgresql
```

Choose Repo for Rockylinux 9
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

Choose Repo for Rockylinux 10
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

then install
```
sudo dnf install -y postgresql18-server postgresql18
```

first initialization of database
```
/usr/pgsql-18/bin/postgresql-18-setup initdb
```

enable at boot then start the database service
```
sudo systemctl enable postgresql-18
```
```
sudo systemctl start postgresql-18
```
```
sudo systemctl status postgresql-18
```

# Setup connection for clients

switch OS user to postgres
```
sudo su - postgres
```

check db version
```
psql --version
```

inside psql command :

set password for superadmin (postgres):
```
\password postgres
```
*enter password*

*verify password*

quit psql command
```
\q
```

then logout back from OS user postgres to sudoer
```
exit
```



install editor (if none available)
```
sudo dnf install -y nano
```

edit database role
```
sudo nano /var/lib/pgsql/18/data/pg_hba.conf
```

edit in file
```
host    all             all             0.0.0.0/0            scram-sha-256
```

edit database general setting
```
sudo nano /var/lib/pgsql/18/data/postgresql.conf
```

change *listen_addresses* in file
```
listen_addresses = '*'
```

open the firewall, by service
```
sudo firewall-cmd --add-service=postgresql --permanent
```
or by port
```
sudo firewall-cmd --add-port=5432/tcp --permanent
```

reload the firewall
```
sudo firewall-cmd --reload
```



# Follow to download pgAdmin 4 client tool:
https://www.pgadmin.org/download/

for the postgreSQL tool, needed in restore, etc., download repo first:

Choose Repo for Rockylinux 9
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

Choose Repo for Rockylinux 10
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

then install the tool only 
```
sudo dnf install -y postgresql18
```








# install PGAGENT (at where your postgreSQL server OS is)
introduction: https://www.pgadmin.org/docs/pgadmin4/8.14/pgagent_install.html

don't need to install the repo again, here as reminder: 

Choose Repo for Rockylinux 9
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```

Choose Repo for Rockylinux 10
```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-10-x86_64/pgdg-redhat-repo-latest.noarch.rpm
```


then install pgagent
```
sudo dnf install -y pgagent_18
```

enable at boot and start immediately the service
```
sudo systemctl enable --now pgagent_18
```

edit / check your config
```
sudo nano  /etc/pgagent/pgagent_18.conf
```

check the file (this is Default)
``` 
DBNAME=postgres
DBUSER=postgres
DBHOST=127.0.0.1
DBPORT=5432
LOGFILE=/var/log/pgagent_18.log
```

check your pgAgent log
```
sudo cat  /var/log/pgagent_18.log
```


update your service user account
```
sudo nano /usr/lib/systemd/system/pgagent_18.service
```

in the file, change user and group
``` 
User=postgres
Group=postgres
```

reload the service configuration
```
sudo systemctl daemon-reload
```


save default postgres password for use with pgAgent:

change OS user to postgres first
```
sudo su - postgres
```

create or edit the password file for superadmin
```
nano ~/.pgpass
```

in the file (create and set for host : port : db : superadmin user : superadmin password)
``` 
127.0.0.1:5432:*:postgres:writePasswordHere
```

set the file permission
```
chmod 0600 ~/.pgpass
```

just to check the file exits
```
echo ~/.pgpass
```

logout from OS user postgres
```
exit
```

restart the pgAgent service
```
sudo systemctl restart pgagent_18
```


Done - continue the pgAgent setting in pgAdmin client tools.




