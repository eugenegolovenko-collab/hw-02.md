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

`Screenshots`

---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6. 

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота 2](ссылка на скриншот 2)`


---

