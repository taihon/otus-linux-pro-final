## OTUS Administrator Linux. Professional: Финальный проект

Проект отказоустойчивого веб-приложения с возможностью автоматического разворота, резервного копирования, мониторинга и централизованного журналирования.
Использованы Ansible, borgbackup, nginx, apache2, prometheus, alertmanager, mysql, php, wordpress и т.д.
Для разворота:

1. Склонируйте репозиторий;
2. Скачайте файл с rpm [отсюда](https://github.com/taihon/otus-linux-pro-final/releases/download/0.0.1-alpha/files.tar.gz);
3. Разверните архив с rpm в ansible/files
4. Установите коллекции ansible:

    - ansible-galaxy collection install ansible.posix
    - ansible-galaxy collection install community.mysql
    - ansible-galaxy collection install community.crypto
5. Разрешите в vagrant использовать сети:
    - 192.168.11.0/24
    - 172.16.1.0/24
    - 10.0.10.0/24
6. Измените значения переменных в ansible/vars/extra.json:
    - wpdb_site_host (требуется указать ip хост-машины с vagrant, порт обязателен)
    - ssl_common_name (требуется указать ip хост-машины с vagrant)
7. Создайте файл ansible/files/telebot.token, указав в нём токен от https://t.me/MiddlemanBot
8. Запустите vagrant up
9. Запустите разворот командой
    ```
    ansible-playbook playbook.yml -e="@vars/extra.json"
    ```
В случае, если всё сделано верно, то станут доступны следующие машины
- monitoring (172.16.1.2) - сервер мониторинга (prometheus, elk, alertmanager)
- fw (10.0.10.10) - сервер файрволла (firewalld)
- backup (172.16.1.3) - сервер РК (borgbackup)
- balancer (10.0.10.2) - сервер балансировки и reverse proxy (nginx)
- node1 (192.168.11.2) - сервер БД (mysql master)
- node2 (192.168.11.3) - сервер БД (mysql slave, borgbackup)
- apnode1 (192.168.11.4) - веб-сервер бэкенда (apache)
- apnode2 (192.168.11.5) - веб-сервер бэкенда (apache)
