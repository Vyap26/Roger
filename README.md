# Roger
Этапы

1. Диск
df -h
lsblk

2. Firewall
sudo ufw status verbose

3. DOS (Denial Of Service Attack) 
sudo vim /etc/fail2ban/jail.conf
[sshd]
enabled = true
port    = 42
logpath = %(sshd_log)s
backend = %(sshd_backend)s
maxretry = 3
bantime = 600

#Add after HTTP servers:
[http-get-dos]
enabled = true
port = http,https
filter = http-get-dos
logpath = /var/log/apache2/access.log (le fichier d'access sur server web)
maxretry = 300
findtime = 300
bantime = 600
action = iptables[name=HTTP, port=http, protocol=tcp]
Add http-get-dos filter

sudo cat /etc/fail2ban/filter.d/http-get-dos.conf
Output:

[Definition]
failregex = ^<HOST> -.*"(GET|POST).*
ignoreregex =

ПОТОМ перезапустить
sudo ufw reload
sudo service fail2ban restart

4. Защиты от скана портов
Config portsentry
First, we have to edit the /etc/default/portsentry file

TCP_MODE="atcp"
UDP_MODE="audp"
After, edit the file /etc/portsentry/portsentry.conf

BLOCK_UDP="1"
BLOCK_TCP="1"
Comment the current KILL_ROUTE and uncomment the following one:

KILL_ROUTE="/sbin/iptables -I INPUT -s $TARGET$ -j DROP"
Comment the following line:

KILL_HOSTS_DENY="ALL: $TARGET$ : DENY
We can now restart the service to make changes effectives
sudo service portsentry restart

5. Процессы останавливаем
sudo systemctl disable console-setup.service
sudo systemctl disable keyboard-setup.service
sudo systemctl disable apt-daily.timer
sudo systemctl disable apt-daily-upgrade.timer
sudo systemctl disable syslog.service

Проверка 
sudo service --status-all

6. Скрипт /root/scripts/update_script.sh
7. Скрипт root/monitor.sh

Проверяем crontab -e
minute   hour   day   month   dayofweek   command

8. Сайт на https://192.168.56.2
9. Deployment - Ansible

https://www.digitalocean.com/community/tutorials/how-to-configure-apache-using-ansible-on-ubuntu-14-04

10. Deployment 2
scp scp file.gz root@server.my:/home/dir


Скопировать локальный файл на сервер:

scp file.gz root@server.my:/home/dir
Скопировать всё содержимое папки на сервере (рекурсивно) в локальную папку (с подробным выводом):

scp -r root@server.my:/home/dir/ /home/local/my/
Между серверами:

scp -r root@server1.my:/home/dir/ root@server2.my:/home/dir/
С указанием порта:

scp -P 9999 file.zip user@server.my:~/

/var/www/roger там лежит index.html  и папка Fedora_files












