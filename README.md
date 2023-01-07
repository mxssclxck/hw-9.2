# 9.2 «Zabbix. Часть 1» НиконоровДА - FOPS-6

## Задание 1

```shell
# Устанавливаем репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update

# Устанавливаем Zabbix сервер, веб-интерфейс и агент.
apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# Устанавливаем PostgreSQL
apt install postgresql

# Создаем пользователя и БД в PostgreSQL
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix

# Импортируем начальную схему и данные в PostgreSQL
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

# Редактируем файл по пути /etc/zabbix/zabbix_server.conf
DBPassword=zabbix

# Запускаем процессы Zabbix сервера и агента. И добавляем в автозагрузку.
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

```

![alt text](https://github.com/mxssclxck/hw-9.2/blob/main/img/1.png)

## Задание 2

* Cкриншот раздела Configuration > Hosts
![alt text](https://github.com/mxssclxck/hw-9.2/blob/main/img/2.png)

* Cкриншот лога zabbix agent
![alt text](https://github.com/mxssclxck/hw-9.2/blob/main/img/3.png)

* Cкриншот раздела Monitoring > Latest data для обоих хостов
![alt text](https://github.com/mxssclxck/hw-9.2/blob/main/img/4.png)

```Shell
# Устанавливаем  репозиторий Zabbix
rpm -Uvh https://repo.zabbix.com/zabbix/6.0/rhel/7/x86_64/zabbix-release-6.0-4.el7.noarch.rpm
yum clean all

# Устанавливаем Zabbix Agent2
yum install zabbix-agent2 zabbix-agent2-plugin-*

# Запускаем процесс Zabbix Agent2 и добавляем в авто загрузку
systemctl restart zabbix-agent2
systemctl enable zabbix-agent2

# На хостах по пути /etc/zabbix/zabbix_agent2.conf нужно прописать ip сервера zabbix
Server=192.168.0.105
```

