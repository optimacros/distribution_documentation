# Описание манифест файла воркспейса Optimacros

Для работы воркспейса Optimacros, используется манифест файл в формате JSON, 
который описывает основные настройки воркспейса, требуемые для его работы.

Манифест воркспейса может иметь любое имя, по умолчанию используется имя `manifest.json`.

Расположение манифеста определяет директорию, которая содержит файловую 
подсистему для работы воркспейса в Linux контейнере.

## Пример базового варианта манифест файла

```
{
  "container": {
    "ip": "10.0.3.15",
    "cpu": 6,
    "memory": 33685016576,
    "ports": {},
    "hosts": {},
  },
  "workspace": {
    "id": "8522aedecc6b4219ee87ee28",
    "name": "TEST",
    "web": {
      "url": "https://om.test.workspace.ru"
    },
    "loginCenter": {
      "url": "https://lc.company.ru/",
      "token": "4aed337a0ac34dd13716c476a4c7",
      "apiUrl": "wss://lc.company.ru/api/ws/v1/"
    },
    "admin": {
      "email": "admin@optimacros.com"
    }
  }
}
```

## Блок `container`

Блок `container` позволяет настроить работу Linux контейнера.

```
{
    "ip": "10.0.3.15",
    "cpu": 6,
    "memory": 33685016576,
    "ports": {},
    "hosts": {},
}
```

### IPv4 адрес контейнера

`required|string`

Для работы Linux контейнера используется LXC подсеть 10.0.3.0/24, 
которая автоматически назначает IPv4 адрес контейнеру. 
Для эмуляции статической привязки адреса, при поднятии контейнера 
мы дополнительно привязываем IPv4 адрес, выбор которого за вами.

Обычно мы выбираем адреса начиная с 10.0.3.15, 
а LXC обычно назначает автоматически контейнерам адреса начиная с 10.0.3.100.

```
{
    "ip": "10.0.3.15",
    ...
}
```

### CPU

`required|integer`

Ограничивает количество воркеров служб Optimacros.\
Не ограничивает память Linux контейнера.

Обычно указываем количество ядер хост сервера.

```
{
    ...
    "cpu": 6,
    ...
}
```

### Memory

`required|integer`

Память в байтах.\
Используется для некоторых ограничений служб Optimacros.\
Используется для формирования счетчика памяти Optimacros расположенного на Drive Landing.\
Не используется для ограничения Linux контейнера.

При работе одного воркспейса на сервере, обычно указывается значение из вывода команды `free -b`.\
При работе нескольких воркспейсов, указывается в нужных пропорциях между всеми воркспейсами

```
{
    ...
    "memory": 33685016576,
    ...
}
```

### Ports

`object`

Параметр позволяет настроить переадресацию портов. При этом можно гибко указать конкретный адрес интерфейса хоста.

При использовании воркспейса с логин центром на одном хосте, обычно мы не используем переадресацию 
портов на воркспейсе контейнере, так как используется Nginx логин центра в качестве reverse proxy.

Обычно переадресация портов воркспейса используем при установке воркспейса отдельно от логин центра.

Для внешнего взаимодействия с воркспейсом импользуются порты: 80, 443, 8080, 8081.

```
{
    ...
    "ports": {
        "0.0.0.0:80": 80,
        "0.0.0.0:443": 443,
        "0.0.0.0:8080": 8080,
        "0.0.0.0:8081": 8081
    },
    ...
}
```

Для работы на конкретном интерфейсе, замените `0.0.0.0` на соответствующий ipv4 адрес.

Для переадресации портов должна быть установлена утилита [redir](https://github.com/troglobit/redir)

### Hosts

Параметр позволяет настроить файл контейнера `/etc/hosts` ([man](https://man7.org/linux/man-pages/man5/hosts.5.html))

```
{
    ...
    "hosts": {
        "test.example.com": "10.0.5.34",
        "ftp.example.com": "192.168.1.44"
    },
    ...
}
```

### DNS

`object`

Параметр позволяет настроить DNS для контейнера, файл 
`/etc/resolv.conf` ([man](https://man7.org/linux/man-pages/man5/resolv.conf.5.html)).

`dns.search` - `string[]` См. в `man` пункт о `search`.

`dns.nameServers` - `object[]` Каждый объект 
коллекции содержит информацию об одном DNS сервере

`dns.nameServers[n].ip` - `required|string` IpV4 адрес DNS сервера

```
{
    ...
    "dns": {
        "search": [
            "example.com"
        ],
        "nameServers": [
            {
                "ip": "8.8.8.8"
            },
            {
                "ip": "8.8.4.4"
            }
        ]
    },
    ...
}
```

Можно использовать `dnsmasq` ([man](https://wikipedia.org/wiki/Dnsmasq)) на хосте:

```
{
    ...
    "dns": {
        "nameServers": [
            {
                "ip": "10.0.3.1"
            }
        ]
    },
    ...
}
```

### Routes

`object[]`

Данный параметр позволяет кастомизировать таблицу маршрутов 
контейнера ([man](https://man7.org/linux/man-pages/man8/ip-route.8.html))

```
{
    ...
    "routes": [
        {
            "destination": "192.168.1.50/32",
            "gateway": "10.0.3.1"
        }
    ],
    ...
}
```

### FreeTDS

`object[]`

Параметр позволяет настроить программный интерфейс `freetds` ([man](https://www.freetds.org/)) 
для работы Optimacros с MS SQL

Данный интерфейс используется Optimacros макросами при пользовании 
коннектором MS SQL с драйвером DBLIB (Выбран по умолчанию)

`freetds[n].hosts` - `required|string[]` Хост MS SQL. Каждый элемент порождает отдельную запись конфигурации `freetds`

`freetds[n].port` - `integer` Порт MS SQL

`freetds[n].ntlmv2` - `boolean` При использовании Active Directory авторизации,
возможно потребуется включить в true. См. [man](https://www.freetds.org/)

```
{
    ...
    "freetds": [
        {
            "hosts": [
                "instance1.example.com",
                "instance2.example.com"
            ],
            "port": 1433,
            "ntlmv2": true
        }
    ],
    ...
}
```

Пример сгенерированной конфигурации `freetds` на основе параметров выше:

```
...
[instance1.example.com]
host = instance1.example.com
port = 1433
use ntlmv2 = yes

[instance2.example.com]
host = instance2.example.com
port = 1433
use ntlmv2 = yes
```

## Блок `workspace`

Блок `workspace` позволяет настроить работу воркспейс приложения Optimacros внутри контейнера.

```
{
    "id": "8522aedecc6b4219ee87ee28",
    "name": "TEST",
    "web": {
      "url": "https://om.test.workspace.ru"
    },
    "loginCenter": {
      "url": "https://lc.company.ru/",
      "token": "4aed337a0ac34dd13716c476a4c7",
      "apiUrl": "wss://lc.company.ru/api/ws/v1/"
    },
    "admin": {
      "email": "admin@optimacros.com"
    }
}
```

### Id

`required|string`

Идентификатор воркспейса из логин центра

```
{
    ...
    "id": "8522aedecc6b4219ee87ee28"
    ...
}
```

### Name

`required|string`

Имя воркспейса из логин центра

```
{
    ...
    "name": "TEST"
    ...
}
```

### Web

`required|object`

`web.url` - `required|string` Полный web адрес воркспейса, включая схему `http|https`.\

`web.ssl` - `object` Настройки SSL сертификата

#### SSL

`object`

Настройки SSL сертификата. 

При использовании SSL сертификата, внешнее взаимодействие 
с воркспейсом возможно на портах 80, 443 и 8081

При отсутствии SSL сертификата внешнее взаимодействие с воркспейсом 
возможно на портах 80 и 8080

`web.ssl.cert` - `required|string` Абсолютный путь к сертификату [X.509](https://en.wikipedia.org/wiki/X.509) на хосте

`web.ssl.key` - `required|string` Абсолютный путь к ключу от сертификата X.509 на хосте

```
{
    ...
    "web": {
        "url": "https://test.example.com",
        "ssl": {
            "cert": "/opt/om/workspace1/cert/bundle.crt",
            "key": "/opt/om/workspace1/cert/crt.key"
        }
    }
    ...
}
```

### Login Center

`required|object`

Настройка взаимодействия воркспейса с логин центром

`loginCenter.url` - `required|string` Полный адрес логин центр, будет использоваться 
для формирования ссылок на профиль, выход из системы и переадресация при необходимости авторизации

`loginCenter.token` - `required|string` Секретный токен воркспейса из логин центра

`loginCenter.apiUrl` - `required|string` Адрес Websocker API точки логин центра для облуживания воркспейсов

```
{
    ...
    "loginCenter": {
        "url": "https://lc.company.ru/",
        "token": "4aed337a0ac34dd13716c476a4c7",
        "apiUrl": "wss://lc.company.ru/api/ws/v1/"
    }
    ...
}
```

### Admin

`required|object`

Настройка супер администратора воркспейса или сервисной учетной записи

`admin.email` - `required|string` Email пользователя

```
{
    ...
    "admin": {
        "email": "admin@optimacros.com"
    }
    ...
}
```

### Shared Folders

`object`

Подключение хост папок для использования в системе макросов

```
{
    ...
    "sharedFolders": {
        "myFolder1": {
            "src": "/mnt/samba1"
        },
        "anotherFolder": {
            "src": "/mnt/ftp1"
        }
    }
    ...
}
```

`myFolder1` или `anotherFolder` - Это пример, как может называться папка, которая будет доступна в системе макросов

`sharedFolders.n.src` - `required|string` Абсолютный путь к папке на хосте, 
которая будет доступна в макросах под именем `n`