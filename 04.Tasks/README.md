<div style="page-break-after: always;"></div>

# Распределенные системы и сети

## Урок 4. Распределённые системы

### Домашнее задание

1. Прислать результаты выполненных запросов через Swagger UI

2. Postman

3. Прислать скрипт на любом удобном языке программирования, где через API будут созданы 10 устройств (все переменные должны задаваться в коде, списком или массивом, но одной строкой), будет запрошена информация о 10 устройствах, будут обновлены имена 10 устройств, будут удалены 10 устройств.

Желательно использовать Python через Google Collab. Для сдачи ДЗ студент должен предоставить преподавателю ссылку с доступом к блокноту с кодом (перед отправкой ДЗ проверьте, что ссылка открывается в приватном режиме браузера).

# Решение

## 1. Swagger UI

<div style="page-break-after: always;"></div>


### Создание API-token

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/01.png?raw=true">

### Applications

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/03.png?raw=true">

<div style="page-break-after: always;"></div>

### Swagger API

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/04.png?raw=true'>

### Bearer token

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/05.png?raw=true'>


### GET

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/06.png?raw=true'>


#### Responses

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/07.png?raw=true'>

### POST

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/10.png?raw=true'>

#### Application id

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/08.png?raw=true'>

#### Device profile id

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/09.png?raw=true'>

#### DevEUI

Любое 64-битное значение в HEX: 0123456789abcdef

#### Responses

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/11.png?raw=true'>

#### Result

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/12.png?raw=true'>

### PUT

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/13.png?raw=true'>

#### Responses

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/13.png?raw=true'>

#### Result

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/14.png?raw=true'>

### DELETE

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/15.png?raw=true'>

#### Responses

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/16.png?raw=true'>


#### Result

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/17.png?raw=true'>


### POST Queue to real device

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/19.png?raw=true'>


#### Data to Base64

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/18.png?raw=true'>

#### Responses

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/20.png?raw=true'>

#### Result

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/21.png?raw=true'>

<img src='https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/22.png?raw=true'>

## 2. Postman

### GET

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/23.png?raw=true">

#### Authorization

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/24.png?raw=true">

#### Save & Send

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/25.png?raw=true">

### POST

#### Body

RAW -> JSON

#### Save & Send

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/26.png?raw=true">

### PUT

#### Body

RAW -> JSON

#### Save & Send

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/27.png?raw=true">

### DELETE

#### Save & Send

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/28.png?raw=true">

### POST Downlink

#### Save & Send

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/29.png?raw=true">

#### Result in Grafana

<img src="https://github.com/allseenn/mesh/blob/main/04.Tasks/pics/30.png?raw=true">

## 3. Python

### Создание 10 устройств

```
import requests
import json

token="eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJhdWQiOiJjaGlycHN0YWNrIiwiaXNzIjoiY2hpcnBzdGFjayIsInN1YiI6ImMxMzk5MGRkLTMyN2ItNGZjYy1iZTU3LWQzNWVmZTY1MzI1MSIsInR5cCI6ImtleSJ9.irLcZ138B8-UTrYhWZG-gFXwvsDMheqz8uiozaOyGzs"
url = 'https://chirpstack-api.iotserv.ru/api/devices'
applicationId = "6bb2cce7-abe9-4e98-980e-f5ada088d177"
deviceProfileId = "29d26ba4-2bf0-452e-9341-2671f442c7da"
devQuantity = 10
devicesList = [f"{i:02d}23456789ABCDEF" for i in range(devQuantity)]
```

```
def create(url, token, applicationId, deviceProfileId, name, devEui):
    headers = {
        'accept': 'application/json',
        'Grpc-Metadata-Authorization': f'Bearer {token}',
        'Content-Type': 'application/json',
    }
    data = {
        "device": {
            "applicationId": applicationId,
            "description": name,
            "devEui": devEui,
            "deviceProfileId": deviceProfileId,
            "isDisabled": False,
            "joinEui": "0000000000000000",
            "name": name,
            "skipFcntCheck": False
        }
    }
    return requests.post(url, json=data, headers=headers)


for i in range(len(devicesList)):
  print(create(url, token, applicationId, deviceProfileId, f"RRG Device {i:02d}", devicesList[i]))
```

### Запрос информации о 10 устройствах

```
def info(url, token, applicationId, limits):
    headers = {
        'accept': 'application/json',
        'Grpc-Metadata-Authorization': f'Bearer {token}',
    }
    params = {
        'limit': limits,
        'applicationId': applicationId
    }
    response = requests.get(url, headers=headers, params=params)
    return response.json()

print(json.dumps(info(url, token, applicationId, devQuantity+1), indent=4))
```

### Обновить имена 10 устройств

```
def update(url, token, applicationId, deviceProfileId, newName, devEui):
    headers = {
        'accept': 'application/json',
        'Grpc-Metadata-Authorization': f'Bearer {token}',
        'Content-Type': 'application/json',
    }
    data = {
        "device": {
            "applicationId": applicationId,
            "deviceProfileId": deviceProfileId,
            "name": newName,
            "description": newName
        }
    }
    put_url = f"{url}/{devEui}"
    return requests.put(put_url, json=data, headers=headers)

for i in range(len(devicesList)):
  print(update(url, token, applicationId, deviceProfileId, f"RRG Device {i:02d} UPDATED", devicesList[i]))

print(json.dumps(info(url, token, applicationId, devQuantity+1), indent=4))
```

<div style="page-break-after: always;"></div>

### Удаление 10 устройств

```
def delete(url, token, devEui):
    headers = {
        'accept': 'application/json',
        'Grpc-Metadata-Authorization': f'Bearer {token}',
    }
    return requests.delete(f"{url}/{devEui}", headers=headers)


for i in range(len(devicesList)):
  print(delete(url, token, devicesList[i]))
```