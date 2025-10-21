# Домашнее задание к занятию "`Система мониторинга Zabbix`" - `Евгений Головенко`

---

### Задание 1

`Установите Zabbix Server с веб-интерфейсом.`

Процесс выполнения

1. `Установите PostgreSQL.` 

`Установка PostgreSQL:`

```
sudo apt update && sudo apt upgrade -y
sudo apt install postgresql -y

```

Проверка запуска службы:

```
sudo systemctl enable postgresql
sudo systemctl start postgresql
sudo systemctl status postgresql
```

Создание базы и пользователя:

```
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
```

2. Пользуясь конфигуратором команд с официального сайта, составьте набор команд для установки последней версии Zabbix с поддержкой PostgreSQL и Apache.
Выполните все необходимые команды для установки Zabbix Server и Zabbix Web Server.

Становлюсь root user

```
sudo -s
```

Устанавливаю репозиторий Zabbix:

```
wget https://repo.zabbix.com/zabbix/7.4/release/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.4+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.4+ubuntu24.04_all.deb
apt update
```

Устанаваю Zabbix server, frontend:

```
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts
```

Импортирую начальную схему и данные:

```
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix
```

Настраиваю БД для cthdthf Zabbix:

```
nano /etc/zabbix/zabbix_server.conf
...................................
^W -> DBP
DBPassword=******
^O -> ^X
```

Запуск процессов Zabbix-сервера:

```
systemctl restart zabbix-server apache2
systemctl enable zabbix-server apache2
```

Открываю веб-страницу Zabbix UI:

```
http://192.168.1.227/zabbix
```

Проверка зависимостей - все зелено.

Указываю параметры PostgreSQL - DBname, User, Password.

Авторизация: Admin / zabbix

<img width="1142" height="835" alt="Zabbix_login" src="https://github.com/user-attachments/assets/e8791864-2f9b-425b-bdfe-054c740ff43a" />

<img width="1479" height="861" alt="Zabbix_dashboard" src="https://github.com/user-attachments/assets/cb357032-403e-47cf-9e60-3b652eb8be85" />

---

### Задание 2

`Установите Zabbix Agent на два хоста.`

Процесс выполнения:

1. Установите Zabbix Agent на 2 вирт.машины, одной из них может быть ваш Zabbix Server.
2. Добавьте Zabbix Server в список разрешенных серверов ваших Zabbix Agentов.
3. Добавьте Zabbix Agentов в раздел Configuration > Hosts вашего Zabbix Servera.
Проверьте, что в разделе Latest Data начали появляться данные с добавленных агентов.

`Требования к результатам:`

Приложите в файл README.md скриншот раздела Configuration > Hosts, где видно, что агенты подключены к серверу

Приложите в файл README.md скриншот лога zabbix agent, где видно, что он работает с сервером

Приложите в файл README.md скриншот раздела Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные.

Приложите в файл README.md текст использованных команд в GitHub (?!!!)

1. Установлен Zabbix Agent на 2 вирт.машины, одна из них мой Zabbix Server на Ubuntu, вторая - VM на Debian.

<img width="1464" height="1049" alt="Zabbix_2 хоста" src="https://github.com/user-attachments/assets/7743ef7f-9fe4-4c57-b481-d050fb3905a1" />

2. Лог Zabbix-agent с VM Debian:

<img width="847" height="636" alt="Zabbix_client" src="https://github.com/user-attachments/assets/405051a4-d6c3-4d4d-82a1-36407c82e96d" />

4. Скриншот Monitoring > Latest data для обоих хостов, где видны поступающие от агентов данные:

<img width="1465" height="1052" alt="Zabbix_monitoring" src="https://github.com/user-attachments/assets/7c4fea50-19d6-4614-9b3f-d18968de23c1" />

5. Текст использованных команд /в GitHub?! - может быть настройки конфигураций клиентов?/

`Клиент на Zabbix-server (Ubuntu)`

```
sudo apt update
sudo apt install zabbix-agent -y
sudo nano /etc/zabbix/zabbix_agentd.conf
.......................
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=Zabbix-server
.......................

sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent
sudo systemctl status zabbix-agent
```

`Клиент на VM Debian 13.0.0`

```
sudo wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
sudo dpkg -i zabbix-release_latest_7.4+debian13_all.deb
sudo apt update
sudo apt install zabbix-agent2
sudo apt install zabbix-agent2-plugin-mongodb zabbix-agent2-plugin-mssql zabbix-agent2-plugin-postgresql
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
sudo nano /etc/zabbix/zabbix_agent2.conf
...........................
Server=192.168.1.227
ServerActive=192.168.1.227
Hostname=debian-client
...........................
sudo systemctl restart zabbix-agent2
sudo systemctl enable zabbix-agent2
sudo systemctl status zabbix-agent2

sudo tail -n 20 /var/log/zabbix/zabbix_agent2.log
```
---

