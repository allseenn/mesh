# Распределенные системы и сети

## Домашняя работа

### Урок 1. Типы сетей и основные технологии

1. Зарегистрировать свое доменное имя на reg.ru (опционально, при желании).
2. Зарегистрировать доменное имя для DDNS сервиса No-IP.
3. Создать CNAME записи не reg.ru по заданному формату.
4. Через reg.ru/nettools/dig убедиться, что все созданные записи для студента добавлены на DNS сервер.
5. Предоставить отчет о проделанной работе.

### Решение

Добавление а-записи на reg.ru

<img src=pics/01.png>

Регистрация имени на no-ip.com

<img src=pics/02.png>

Задание имени пользователя на no-ip.com

<img src=pics/03.png>

CNAME-запись для службы Grafana

<img src=pics/05.png>

CNAME-запись для службы Influxdb

<img src=pics/06.png>

CNAME-запись для службы Mosquitto

<img src=pics/07.png>

CNAME-запись для службы nodered

<img src=pics/08.png>

CNAME-запись для службы Wireguard

<img src=pics/09.png>

Получение ns-записей с помощью системной команды ``getent hosts``

<img src=pics/10.png>
