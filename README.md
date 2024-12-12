# Установка и настройка Zabbix

## Выполненные шаги

```bash
# 1. Становимся root-пользователем
sudo -s

# 2. Устанавливаем репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_6.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_6.0+ubuntu24.04_all.deb
apt update

# 3. Устанавливаем Zabbix сервер, веб-интерфейс и агент
apt install zabbix-server-pgsql zabbix-frontend-php php8.3-pgsql zabbix-apache-conf zabbix-sql-scripts zabbix-agent

# 4. Создаем базу данных для Zabbix
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u postgres psql zabbix

# 5. Настраиваем базу данных в конфигурационном файле Zabbix
nano /etc/zabbix/zabbix_server.conf
# (Изменяем параметр `DBPassword` на пароль пользователя Zabbix)

# 6. Запускаем процессы Zabbix сервера, агента и Apache
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2

# 7. Проверяем статус сервисов
systemctl status apache2
systemctl status zabbix-server zabbix-agent

# 8. Открываем веб-интерфейс Zabbix в браузере
# Веб-интерфейс доступен по адресу: http://51.250.71.202/zabbix

# 9. Ссылка на Google-документ с фотографиями
# https://docs.google.com/document/d/1ecspcvtEY4TzW7Z4QhdCJbowKdorpmB79WRz8LfXUlg/edit?usp=sharing
