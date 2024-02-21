# Распределенные системы и сети

## Урок 5. Mesh-сети

### Домашнее задание

Проведение радиообследования Wi-Fi покрытия и предоставление итоговых результатов, с их интерпретацией и рекомендациями по улучшению.

## Введение 
Домашняя локальной сет основана на витой паре, из соображений безопасности, помехоустойчивости, латентности и скорости. К этой сети подключены ПК, ноутбуки, телевизоры, принтеры, приставки, ip-телефоны и т.д. 

Личные смартфоны имеют мобильный интернет и не используются для работы или просмотра фильмов.

Единственная точка доступа wifi расположена у входа и настроена только на гостевом режим, без возможности доступа в локалку, ее мощности достаточно для покрытия сигналом гостиной и кухни, где обычно бывают гости.  Телевидение и стационарный телефон доставляются в спальни по кабелю.

Но, по нашей легенде будем подключать IoT устройства, и в них же, вся суть проблемы, ее раскрою постепенно.

## Решение 

На семинаре рекомендовано ПО [Wi-Fi Surveyor](https://github.com/ecoAPM/WiFiSurveyor), последняя бинарная сборка которого, состоялась полтора года назад. Но, судя по гитхабу, проект хоть и не очень активный, но все еще живой и пулл-реквесты мерджатся примерно раз в неделю. Можно собрать самому из исходников более свежую программу, но этот шаг осилят не все админы, не говоря уже про обычного пользователя.

### Установка ПО

<img src=pics/00.png width=300>

Долгое сканирование virustotal ничего не нашло, что не исключает обфускацию злоумышленником бинарного файла. Сам exe-файл подозрительно огромный, ни намека на модульности и MVC.

### RSSI

<img src=pics/01.png>

На картинке отчетливо видно, что сигнал преемлимый только на кухне и в гостинной. Чем дальше от точки доступа тем хуже сигнал.

### SNR

<img src=pics/02.png>

Картина примерно аналогичная, только, по неведомым пока причинам сигнала нет в гигиенических комнатах.

### Варианты усиления сигнала

Если бы задача стояла покрыть вайфаем не гостевую зону а всю квартиру, то варианта, исходя из картинок, только два
1. Перемещение точки доступа ближе к спальням, что неминуемо ведет к перекладке всех кабелей, включая силовой.
2. Установка второй точки, например на лоджии, что тоже влечет за собой расходы на прокладку слаботочных и силовых кабелей.
Выводы втекающие из полученных диаграмм, такие: потратить определенную сумму денег и труда на оборудование и коммуникации.
Но, есть не менее эффективное и бесплатное решение. Прежде чем перейти к его описанию, нужно упомянуть не только недостатки программы, но и метода "сканирование ноутбуком".

### Сканирование ноутбуком

Считаю метод сканирования с ноутбуком не оптимальным по раду причин:
1. Вес и габариты
2. Мощность
3. Не IoT

#### 1. Вес и габариты

Когда дело идет о собственной квартире, то легко организовать сканирование и с помощью стационарного ПК.

Но, ситуация меняется, когда данный процесс становится частым, профессиональным или с выездами. Помимо инструментов (дрель, отвертка, пассатижи, обжимка и т.д.) и устанавливаемого оборудования, придется с собой всегда возить ноутбук. Не говоря уже про тот файкт, что те же датчики (пожарные, сигнализации и т.д.) в основном устанавливаются высоко под потолком, трудноописуемая ситуация на 5 метровой стремянке с развернутым ноутбуком ))

#### 2. Мощность 
Мощность антенны и самого встроенного mini-pciexpress адаптера вайфай в ноутбуке заметно больше чему у IoT устройств, из-за тех же габаритов и небольшой энергоемкости.

#### 3. Не IoT

Ноутбук не IoT.
Смартфон наиболее приближен по размерам, мощностям и зачастую архитектурой процессора (ARM), к IoT.
В плане компактности и легкости впереди самых легких ноутбуков.
Со смартфоном можно измерять скорость сети или мобильного интернета даже сидя на дереве. 

### Интерференция

Подсвеченная карта WiFiSurveyor, это предположительная оценка, которая может совершенно не соответствовать действительности. Т.к. данная карта не учитывает толщину стен, из какого материала изготовлена мебель и т.д. 
Посмотрим на рисунок из раздела RSSI, из него следует что у соседей сквозь толстую несущую стену должен быть лучше интернет чем на моей кухне!?!

Самое главное что не учитывает WiFiSurveyor и то, что было упомянуто на семинаре это интерференция.

Интерференция возникает из-за самого протокола IEEE 802.11, который предполагает разделение частотного диапазона на каналы, которые очень часто выбираются автоматически самим устройством, не учитываю текущую загруженность эфира. Помехи соседских точек доступа, работающих на том же канале, что наша точка доступа, более весомый момент, чем даже несущие стены. И непредсказуемость возникновения интерференции еще объясняется, тем что соседи могут разместить точку доступа в любом месте своей территории. Такое влияние заметно на картинке из раздела SNR, где описываю отсутсвтие сигнала в санузлах. Или, более показательный пример, в нижнем правом углу на лоджии -3, а в гостиной в нижнем правом -9, расстояние более чем в два раза различается.


### Сканирование с помощью смартфона

Есть большое количество ПО для работы с wifi для андроида. Использовал и продолжаю переодически пользоваться швейцарским ножем в сетевой диагностики программой [PingTools](https://play.google.com/store/apps/details?id=ua.com.streamsoft.pingtools).

<img src=pics/03.png width=300>

В свое время приобрел профессиональную за 50 рублей ). 

<img src=pics/04.jpg>

В связи с политикой гуглмаркета приобрести версию про не получится, но и бесплатной должно хватить. 
В данном случае нам нужен только пару инструментов из множества, это сканер файвай каналов, и iperf.

#### WiFi Scaner до смены канала

Все замеры будут производится в одной точке:

<img src=pics/05.png width=300>

На картинке выбран диапазон сканирования сетей вайфай на частоте 2.4.Ггц. Сканер позволяет работать и в других диапазонах, это зависит от чипа вашего смартфона.

STR - наша точка доступа, она работает на 6 канале, частотного диапазона 2.4Ггц. Мощность сигнала -51дБ. 
Первое что бросается в глаза, что большое количество точек доступа, наслаиваются друг на друга, их названия затерты в соответсвии с законом о персданных, но тут показаны только те точки доступа, которые программа смартфона смогла зафиксировать за пару минут своей работы, на самом деле их еще больше. 
Второе что бросается в глаза - это полный штиль на 8-ом канале.

#### Iperf до смены канала

PingTools содержит также утилиту iperf, которую можно запускать как в режиме клиента, так и в режиме сервера.

<img src=pics/06.png width=300>

Скорости выгрузки и получения одинаковы, примерно 17 мега бит в секунду.

Изменим настройки точки доступа, и назначим ей свободный 8-канал.

#### WiFi Scaner после смены канала

<img src=pics/07.png width=300>

На графике видно, что STR точка доступа занимает теперь восьмой канал, и даже немного увеличился уровень сигнала, который был -51 дБ, а стал -49дБ. 
Это можно было бы списать на допустимую погрешность, но что покажет замер скорости iperf

#### Iperf после смены канала

<img src=pics/08.png width=300>

Итого: увеличение скорости более чем в три раза.

### Заключение

Данный пример показал, что без материальных и трудовых затрат можно существенно увеличить скорость в конкретно заданной точке. 
И не факт, что в других точках скорость увеличится. Так как мы имеем дело даже не с трехмерным пространством, а с пятимерным, где четвертым параметром является мощность сигнала, пятым время.
Для просчета всех возможных вариантов мощности сигнала к заданный промежуток времени в конкретной точке нашего объемного помещения, нам потребуются искуственные нейронные сети работающие не мощных серверах с GPU.
Поэтому производители вайфай оборудования пошли путем добавления частот сначала 5Ггц, далее 6Ггц, в планах другие частоты.

И как не странно, все вышеописанное не имеет смысла, т.к. IoT устройства, в большинстве случаев не будут работать по протоколу вайфай. Они работают на более низких частотах чем файвай и я не знаю решений для выявления интерференции например в сетях LoRaWan. В этом секторе тоже произойдет перенасыщение и придется как с вайфаем выделять еще частоты.