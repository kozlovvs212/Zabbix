# Домашнее задание к занятию «Система мониторинга Zabbix»

## Выполнил: Виктор Козлов

### Задание 1: Установка Zabbix Server с веб-интерфейсом

### Задание 2: Установка Zabbix Agent на два хоста

#### Скриншоты
Все скриншоты, необходимые для выполнения задания, находятся по следующей ссылке:  
[Скриншоты выполнения задания](https://docs.google.com/document/d/1ecspcvtEY4TzW7Z4QhdCJbowKdorpmB79WRz8LfXUlg/edit?usp=sharing)

#### Использованные команды:
```bash
# Установка PostgreSQL
sudo apt update
sudo apt install postgresql
sudo systemctl start postgresql
sudo systemctl enable postgresql

# Добавление репозитория Zabbix
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-5%2Bubuntu22.04_all.deb
sudo dpkg -i zabbix-release_6.0-5+ubuntu22.04_all.deb
sudo apt update

# Установка Zabbix Server, веб-интерфейса и агента
sudo apt install zabbix-server-pgsql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# Настройка базы данных PostgreSQL
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/doc/zabbix-sql-scripts/postgresql/create.sql.gz | sudo -u zabbix psql zabbix

# Настройка Zabbix Server
sudo nano /etc/zabbix/zabbix_server.conf
# (Измените параметр DBPassword=<ваш_пароль>)

# Перезапуск сервисов
sudo systemctl restart zabbix-server zabbix-agent apache2
sudo systemctl enable zabbix-server zabbix-agent apache2

# Установка Zabbix Agent на обе машины
sudo apt update
sudo apt install zabbix-agent

# Настройка Zabbix Agent
sudo nano /etc/zabbix/zabbix_agentd.conf
# (Измените параметры Server и ServerActive, добавив IP Zabbix Server)

# Перезапуск Zabbix Agent
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent

# Проверка подключения
zabbix_get -s <IP Zabbix Agent> -k agent.ping
