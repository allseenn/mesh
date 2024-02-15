# Распределенные системы и сети

## Урок 3. Примеры распределённых технологий

### Домашнее задание

Развертывание системы проксирования через NGINX, согласно занятию.

Предоставление реквизитов доступа в виде доменных имен к сервисам Grafana, InfluxDB, Node-Red каждым студентом.

(Например: grafana-gvi.gb-iot-1234.ru, influxdb-gvi.gb-iot-1234.ru, nodered-gvi.gb-iot-1234.ru).

## Решение

### Учетные данные

Ко всем службам используются одни учетные данные:

- логин: admin
- пароль: students

### Список сервисов и их адресов

1. **Mosquitto:** wss://mosquitto-rrg-5471.gb-iot.ru
2. **IndluxDB2:** https://influxdb-rrg-5471.gb-iot.ru
3. **Grafana:** https://grafana-rrg-5471.gb-iot.ru
4. **NodeRed:** https://nodered-rrg-5471.gb-iot.ru

#### 1. Mosquitto

В качестве клиента рекомендую [MQTTX](https://mqttx.app/downloads), т.к. стандартный консольный клиент не умеет работать с вебсокетом, который еще и обернут в TLS от nginx.

В схеме обратного прокси с поддоменами, достаточно одного открытого порта на шлюзе 443 для nginx, в отличие от схемы с подпапками, где пришлось отркывать дополнительный порт под вебсокет.

В MQTTX через кнопку (+) плюс создаем новое соединение с шифрованным вебсокетом wss:

<img src=pics/01.png>

Нажав кнопку Connect соединяемся через nginx с Mosquitto. Таким образом мы можем эмулировать работу оконечного оборудования.

Предположим мы имеем, несколько метеостанций в различных районах и информация поступает по защищенному протоколу на наш сервер по сети интернет. Внизу программы с помощью окошка отправки сообщения, в его заголовке напечатаем MeteoStationKremlin - это topic, и выбрав формат сообщения PlainText в теле сообщения отправим температуру -15.5

<img src=pics/02.png>

#### 2. IndluxDB2

С помощью браузера откроем адрес https://influxdb-rrg-5471.gb-iot.ru

<img src=pics/03.png>

После авторизации попадаем в основное окно, нажав слева в главной вертикальной панели инструментов, кнопку Data Explorer, попадем в проводник данных:

<img src=pics/04.png>

Раздел Data Explorer состоит из двух основных областей: главной области графиков и снизу таблица запросов, состоящая из последовательно расположенных команд запроса в виде столбцов с выпадающими списками.

В первом столбце запросов FROM видим наш бакет IOT, поставив галку во втором столбце в самом низу списка на mqtt_consumer, откроется третий столбец формирования запроса, выберем в нем из выпадающего списка topic и увидим все наши сообщения разбитые по темам относящимся к нашему оконечному оборудованию, тут мы наблюдаем только основную метео станцию MeteoStationKremlin, поставим на ней галку. 

В появившемся четвертом столбце в списке _field поставим галку на value. Осталось нажать на кнопку SUBMIT и наш сформированный запрос методом нокод сформирует график временных рядов с показаниями температуры с нашей "метеостанции", которую сэмулировали с помощью MQTTX.

<img src=pics/05.png>

Нажав рядом с кнопкой SUBMIT другую SCRIPT EDITOR увидим реальный код запроса, а активировав переключатель View Raw Data наш график отобразит детальную информацию отправленную с метеостанции:
 
<img src=pics/06.png>

Код запроса можно использовать для построения графиков и спидометров в дашбордах Графаны.

#### 3. Grafana

В браузере откроем адрес https://grafana-rrg-5471.gb-iot.ru

<img src=pics/07.png>

После авторизации через главное меню слева экрана войдем в раздел Дашбордов 

<img src=pics/08.png>

Прежде всего дашборд нужно создать нажав на кнопку +Create Dashboard, в следующем окне нажимаем кнопку +Add visualization.

В окне Select data source виберем нашу базу InfluxDB, кликнув по ней один раз.

<img src=pics/09.png>

После этого откроется главное окно нашего дашборда, внизу которого расположено окно запросов, туда мы вставим наш код из второго пункта InfluxDB2

<img src=pics/10.png>

Тут имеется много настроек, главный смысл которых, организация удобного представления данных для аналитики, можно настроить автообновления и тогда будем наблюдать картинку происходящего в режиме реального времени. После завершения настроек нажав Apply свехру справа. Останутся только графики, либо спидометры с гистограммами, которые можно уменьшать перемещать и делать заметки.

#### 4. NodeRed

Откроем в браузере https://nodered-rrg-5471.gb-iot.ru

<img src=pics/11.png>

После авторизации, попадаем в мощнейший инструмент автоматизации, эмуляции, фильтрации, отладки, подключения и парсинга данных для IOT.

Сэмулируем еще одну метеостанцию MeteoStationBarviha:

1. Для этого из общих элементов перетащим на основное поле потока 1 метку времени (inject), двоиным щелчком по метка времени откроем ее параметры и зададим их:

<img src=pics/12.png>

2. Выберем из раздела сеть элемент mqtt out и перетащим его на основное поле после метки времени Barviha, двойным кликом перейдем к настройкам mqtt, прежде всего нужно настроить сервер mqtt куда будут посылаться сгенерированные данные от NodeRed:

<img src=pics/13.png>

Не забываем во вкладке безопасность задать пароль и юзера от Mosqutto 

<img src=pics/14.png>

После настроек нажимаем красную кнопку Добавить -> В теме не забываем указать MeteoStationBarviha и нажимаем красную кнопку Готово

<img src=pics/15.png>

3. Для отладки можем добавить третий элемент на поле из Общие debug, далее необходимо соединить метку времени Barviha с debug1 и с MeteoStationBarviha. После нажимаем основную красную кнопку Deploy (Развернуть). И нажав на жучка в правокй панели info (инфо) будем наблюдать свежесгенерированные данные 

<img src=pics/16.png>

#### Barviha

Вернемся в InfluxDB и обновим страницу Data Explorer

<img src=pics/17.png>

Наблюдаем вновь появившиеся данные с дачи в Борвихе, скопируем код запроса в Графану и добавим визуализацию +Add:


<img src=pics/18.png>

### Nginx

Благодаря конфигурации Nginx в качестве обратного прокси, мы можем использовать всего лишь один открытый порт 443 и обернуть передаваемые данные и трафик с систем визуализации в защищенное соединения, при этом используя только один общий сертификат безопасности.

/etc/nginx/conf.d/iot.conf

```
server {
    listen 443 ssl;
    server_name mosquitto-rrg-5471.gb-iot.ru;

    location / {
     proxy_http_version 1.1;    
     proxy_pass http://192.168.1.8:8081;
     proxy_set_header Upgrade $http_upgrade;
     proxy_set_header Connection "upgrade";   
     proxy_set_header Host $host;
  }

    ssl_certificate /etc/letsencrypt/live/gb-iot.ru/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/gb-iot.ru/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
}

server {
    listen 443 ssl;
    server_name influxdb-rrg-5471.gb-iot.ru;
    ssl_certificate /etc/letsencrypt/live/gb-iot.ru/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/gb-iot.ru/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
    
    location / {
	proxy_set_header X-Forwarded-For $remote_addr;
	proxy_pass http://192.168.1.8:8086;
	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
    }
}

server {
    listen 443 ssl;
    server_name grafana-rrg-5471.gb-iot.ru;
    ssl_certificate /etc/letsencrypt/live/gb-iot.ru/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/gb-iot.ru/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
    
    location / {
	proxy_set_header X-Forwarded-For $remote_addr;
	proxy_pass http://192.168.1.8:3000;
	proxy_set_header Host $host;
	proxy_set_header X-Real-IP $remote_addr;
    }
}

server {
    listen 443 ssl;
    server_name nodered-rrg-5471.gb-iot.ru;
    ssl_certificate /etc/letsencrypt/live/gb-iot.ru/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/gb-iot.ru/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
    
  location / {
    proxy_pass http://192.168.1.8:1880;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
  }
}
```
