# Домашнее задание к занятию "4.3. Языки разметки JSON и YAML"

## Обязательные задания

1. Мы выгрузили JSON, который получили через API запрос к нашему сервису:
	```json
    { "info" : "Sample JSON output from our service\t",
        "elements" :[
            { "name" : "first",
            "type" : "server",
            "ip" : 7175 
            },
            { "name" : "second",
            "type" : "proxy",
            "ip : 71.78.22.43
            }
        ]
    }
	```
  Нужно найти и исправить все ошибки, которые допускает наш сервис

```json
{
    "info": "Sample JSON output from our service",
    "elements": [
        {
            "name": "first",
            "type": "server",
            "ip": "7175"
        },
        {
            "name": "second",
            "type": "proxy",
            "ip": "71.78.22.43"
        }
    ]
}
IP-адрес "first" явно определен неверно, данных для исправления недостаточно. 
```

>2. В прошлый рабочий день мы создавали скрипт, позволяющий опрашивать веб-сервисы и получать их IP. К уже реализованному функционалу нам нужно добавить возможность записи JSON и YAML файлов, описывающих наши сервисы. Формат записи JSON по одному сервису: { "имя сервиса" : "его IP"}. Формат записи YAML по одному сервису: - имя сервиса: его IP. Если в момент исполнения скрипта меняется IP у сервиса - он должен так же поменяться в yml и json файле.

```python
#!/usr/bin/python3

import json
import yaml
import socket

Service_list = ['drive.google.com', 'mail.google.com', 'google.com']
ip_check = dict.fromkeys(Service_list)

try:
  with open ('service_ip', 'r') as file:
    for line in file:
      key, value = line.rstrip('\n').split(' - ')
      ip_check[key] = value
except (IOError, ValueError):
  pass

with open ('service_ip', 'w') as file:
  for service in Service_list:
    ip_addr = socket.gethostbyname(service)
    output = service + ' - ' + ip_addr
    if ip_check[service] != ip_addr: print('[ERROR] ', output)
    else: print(output)
    file.write(output+'\n')

ip_check_detached = [{key:ip_check[key]} for key in ip_check]

with open ('service.json', 'w') as file_json:
  json.dump(ip_check_detached, file_json, indent = 4)
with open ('service.yml', 'w') as file_yaml:
  yaml.dump(ip_check_detached, file_yaml, indent = 4)
```

## Дополнительное задание (со звездочкой*) - необязательно к выполнению

Так как команды в нашей компании никак не могут прийти к единому мнению о том, какой формат разметки данных использовать: JSON или YAML, нам нужно реализовать парсер из одного формата в другой. Он должен уметь:
   * Принимать на вход имя файла
   * Проверять формат исходного файла. Если файл не json или yml - скрипт должен остановить свою работу
   * Распознавать какой формат данных в файле. Считается, что файлы *.json и *.yml могут быть перепутаны
   * Перекодировать данные из исходного формата во второй доступный (из JSON в YAML, из YAML в JSON)
   * При обнаружении ошибки в исходном файле - указать в стандартном выводе строку с ошибкой синтаксиса и её номер
   * Полученный файл должен иметь имя исходного файла, разница в наименовании обеспечивается разницей расширения файлов

---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
