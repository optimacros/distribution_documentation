# Описание манифест файла Воркспейса

Для работы Воркспейса используется манифест файл в формате JSON, 
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
Но бывают конфликты, для их исключания смотрите вывод команды `sudo lxc-ls -f`, 
так вы можете понять какие адреса уже заняты и обойти их

```
{
    "ip": "10.0.3.15",
    ...
}
```

### Gateway

`string`

Адрес шлюза контейнера в формате IPv4. Значение по умолчанию - 10.0.3.1

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

При использовании воркспейса с Логин Центром на одном хосте, обычно мы не используем переадресацию 
портов на воркспейсе контейнере, так как используется Nginx Логин Центра в качестве reverse proxy.

Обычно переадресация портов воркспейса используем при установке воркспейса отдельно от Логин Центра.

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

Для работы на конкретном интерфейсе, замените `0.0.0.0` на соответствующий IPv4 адрес.

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

`dns.nameServers[n].ip` - `required|string` IPv4 адрес DNS сервера

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

### Tunnel

`object`

`tunnel.status` - `required|boolean` если включен, система будет использовать vpn, настроенный на хост-машине. 

```
{
    ...
    "tunnel": {
        "status": true
    }
    ...
}
```

### Limits

`object`

`limits.nofile` - `integer` количество дескрипторов https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Process Properties

`limits.nproc` - `integer` ограничивает количество дочерних systemd процессов https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Process%20Properties

```
{
    ...
    "limits": {
        "nofile": 123,
        "nproc": 123
    }
    ...
}
```

### Synced Folders

`object`

`syncedFolders.^[a-z0-9_]+$` - Вместо регулярного выражения подставить id встроенной папки, по которому к ней можно будет обращаться.

`syncedFolders.^[a-z0-9_]+$.externalPath` - `required|string` Путь до встраиваемой папки на хост-машине (вне vagrant контейнера).

```
{
    ...
    "syncedFolders": {
       "^[a-z0-9_]+$": {
           "externalPath": "path/to/folder"
       }
    }
    ...
}
```

### Cgroup

`object`

`cgroup.version` - `enum: ["auto"|"v1"|"v2"]` В этом параметре нужно указать текущую версию cgroup для совместимости с lxc. Если есть затруднения с определением, лучше оставить значение "auto", которое выбирается по умолчанию.

```
{
    "cgroup": {
        "version": "auto"
    }
}
```

### Proxy

`object`

Параметр позволяет настроить proxy для сервисов. 

```
{
    ...
    "proxy": {
        "http": "http://test1.com",
        "https": "https://test2.com",
        "ftp": "ftp://test3.com",
        "ignore": [
            "http://test4.com",
            "http://test5.com"
        ]
    }
    ...
}
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

Идентификатор воркспейса из Логин Центра

```
{
    ...
    "id": "8522aedecc6b4219ee87ee28"
    ...
}
```

### Name

`required|string`

Имя воркспейса из Логин Центра

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

`web.ssl.key` - `required|string` Абсолютный путь к ключу от сертификата [X.509](https://en.wikipedia.org/wiki/X.509) на хосте

```
{
    ...
    "web": {
        "url": "https://test.example.com",
        "ssl": {
            "cert": "/om/workspace1/cert/bundle.crt",
            "key": "/om/workspace1/cert/crt.key"
        }
    }
    ...
}
```

### Login Center

`required|object`

Настройка взаимодействия воркспейса с Логин Центром

`loginCenter.url` - `required|string` Полный адрес Логин Центр, будет использоваться 
для формирования ссылок на профиль, выход из системы и переадресация пользователя при необходимости авторизации

`loginCenter.token` - `required|string` Секретный токен воркспейса из Логин Центра

`loginCenter.apiUrl` - `required|string` Адрес Websocker API точки Логин Центра для облуживания воркспейсов

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

### Console

`object`

Крэдэншэлы без которых команда не выполнится 
`commands.<command>.user` - `string` 
`commands.<command>.token` - `string`

`commands.frontend-build.autoBuildOnContainerStart` - `boolean` если true - собирает frontend при запуске контейнера

`commands.container-workspace-setting-update.excludeCubesInfo` - `boolean` если true - исключает параметр `cubeInfos` из информации по кубам
`commands.container-workspace-setting-update.excludeDashboardDefinition` - `boolean` если true - исключает параметр `dashboardsInfo.dashboardInfos.n.dashboardDefinitionJsonString` из информации по дашборду
`commands.container-workspace-setting-update.excludeContextTableDefinition` - `boolean` если true - исключает параметр `contextTablesInfo.contextTableInfos.n.contextTableDefinitionJsonString` из информации по контекстным таблицам
`container-php-config-update.opcache.status` - `boolean` если true - будет включено php кэширование через opcache

`container-app-config-update.lockModelStatus` - `boolean` может ли пользователь "залочить" модель, если false - то модель перейдет в "disabled" при попытке это сделать
`container-app-config-update.smtp.maxAttachmentSize` - `integer` максимальный размер файлов прикрепленных к отсылаемым по протоколу smtp письмам (указывается в количестве байт)

```
{
    ...
    "commands": {
        "cxp-build": {
            "user": "username",
            "token": "token123"
        },
        "encode-source": {
            "user": "username",
            "token": "token123"
        },
        "cppmw-build": {
            "user": "username",
            "token": "token123"
        },
        "frontend-build": {
            "user": "username",
            "token": "token123",
            "autoBuildOnContainerStart": true,
            "appConfig": {
                "socketSecurityPort": 1111,
                "socketPort": 2222
            },
            "relativeUrlPrefix": "urlprefix/"
        },
        "container-workspace-setting-update": {
            "excludeCubesInfo": true,
            "excludeDashboardDefinition": true,
            "excludeContextTableDefinition": true
        },
        "container-php-config-update": {
            "opcache": {
                "status": true
            }
        },
        "container-app-config-update": {
            "lockModelStatus": true,
            "smtp": {
                "maxAttachmentSize": 50000000
            }
        }
    }
    ...
}
```

### Backup Archive

`object`

`backupArchive.status` - `boolean` если true, то при удалении модели происходит предварительное сохранение ее бэкапа. 

```
{
    ...
    "backupArchive": {
        "status": true
    }
    ...
}
```

### Flags

`string[]`

Позволяет указать массив любых строк, которые будут отображаться в admin/settings в виде флагов. Например, можно автоматизированно передавать строку “ok”, если с воркспэйсом все в порядке. Кроме того, некоторые значения оказывают влияние на работу системы. Список таких значений:

`fun_loader_message_off` - отключение забавных надписей при загрузке приложения

Глобальные темы:
`interface_theme_optimacros`
`interface_theme_olapsoft`
`interface_theme_advexcel`

Цветовые темы:
`color_scheme_advexcel`
`color_scheme_optimacros`
`color_scheme_olapsoft`
`color_scheme_corplan`
`color_scheme_ovk`
`color_scheme_tvel`
`color_scheme_orange`
`color_scheme_domrf`
`color_scheme_dark`
`color_scheme_yadromain`
`color_scheme_yadrolight`

Языки:
`interface_language_RU`
`interface_language_EN`
`interface_language_DE`
Определяет список доступных на воркспейсе языков на выбор, последовательность указания флагов имеет значение: определяет порядок в выпадающем меню (первый язык в списке является дефолтным)

`support_email_support@optimacros.com` - где support@optimacros.com является почтой указываемой в модальных окнах при появлении ошибок в интерфейсе

`prod` - наличие этого флага убирает некоторые элементы в Header Menu для демонстрации, так же добавляет в некоторых местах модальное окно, которое просит ввести пароль ввиду триальной версии

```
{
    ...
    "flags": [
        "prod",
        "flag1",
        "flag2",
    ]
    ...
}
```

### WinAgent

`object`

`winAgent.commandUrl` - `required|string` устанавливает url агента, на который будут подаваться команды.

`winAgent.downloadUrl` - `required|string` устанавливает url, по которому можно будет скачивать результирующие документы.

`winAgent.auth.type` - `required|enum: ["basic"|"digest"|"ntlm"]` тип аутентификации, по которому можно будет подключиться к winAgent.

`winAgent.auth.user` - `required|string` имя пользователя, под которым можно установить соединение с winagent.

`winAgent.auth.password` - `required|string` пароль для этого пользователя.


```
{
    ...
    "winAgent": {
        "commandUrl": "url",
        "downloadUrl": "url",
        "auth": {
            "type": "basic|digest|ntlm",
            "user": "username",
            "password": "pass"
        }
    }
    ...
}
```

### Mysql

`object`

`mysql.memory` - `integer` то же самое, что и oltp.mysql.memory.

```
{
    ...
    "mysql": {
        "memory": 123,
        "userPasswords": {
            "root": "pass",
            "optimacros": "pass"
        }
    }
    ...
}
```

### Oltp

`object`

`oltp.mysql.memory` - `integer` объем памяти, используемый для кэширования. Чем больше выделено памяти на кэширование - тем больше производительность работы с базой. Под капотом устанавливает значение для innodb_buffer_pool_size.

`oltp.mysql.web.status` - `required|boolean` позволяет включить или выключить доступ к базе через phpmyadmin.

`oltp.mysql.web.captcha` - `object` позволяет настроить google recaptcha капчу при входе в phpmyadmin.

```
{
    ...
    "oltp": {
        "mysql": {
            "memory": 123,
            "userPasswords": {
                "root": "pass",
                "admin": "pass",
                "phpmyadmin": "pass",
                "writer": "pass",
                "reader": "pass"
            },
            "web": {
                "status": true,
                "captcha": {
                    "publicKey": "key",
                    "privateKey": "key"
                }
            }
        }
    }
    ...
}
```

### Mongod

`object`

```
{
    ...
    "mongodb": {
        "userPasswords": {
            "admin": "pass",
            "optimacros": "pass"
        }
    }
    ...
}
```

### Influxdb

`object`

```
{
    ...
    "influxdb": {
        "userPasswords": {
            "optimacros": "pass"
        }
    }
    ...
}
```

### Audit

`object`

`audit.status` - `boolean` позволяет включить или выключить аудит.

`audit.saveClientRequestMessage` - `boolean` сохранять ли пользовательские запросы в аудит.

`audit.saveClientResponseMessage` - `boolean` сохранять ли ответы, отсылаемые пользователям в аудит.

`audit.mongodb.dsn` - `string` определяет dsn для подключения к базе аудита mongodb (формат: `mongodb://{hostname}:{port}/{authenticationDatabase}`).

`audit.worker.count` - `integer` количество инстансов-сервисов воркеров аудита, которое будет запущено.

`audit.logger.count` - `integer` количество инстансов-сервисов логгеров аудита, которое будет запущено.

```
{
    ...
    "audit": {
        "status": true,
        "saveClientRequestMessage": true,
        "saveClientResponseMessage": true,
        "mongodb": {
            "dsn": "dsn"
        },
        "worker": {
            "count": 5
        },
        "logger": {
            "count": 5
        },
    }
    ...
}
```

### Nginx

`object`

`nginx.realIpHeader` - `string` http://nginx.org/ru/docs/http/ngx_http_realip_module.html#set_real_ip_from

`nginx.allowRealIpFrom` - `string` http://nginx.org/ru/docs/http/ngx_http_realip_module.html#real_ip_header

`nginx.wsExternalMode` - `boolean` не обязательное. определяет режим работы websockets. `true` - вебсокет будет работать на портах 8080/8081(в зависимости от ssl настроек). `false` - работа на портах 80/443

```
{
    ...
    "nginx": {
        "realIpHeader": "X-Forwarded-For",
        "allowRealIpFrom": [
            "255.255.255.255",
            "0.0.0.0"
        ],
        "wsExternalMode": false
    }  
    ...  
}
```

### Server Status

`object`

`serverStatus.status.refresh` - `integer` как часто монитор показателей сервера будет обновляться. Указывается в секундах. Значение по умолчанию - 1 секунда, минимум - 1 секунда.  

`serverStatus.status.retention` - `integer` определяет количество дней, после истечении которых, данные метрики influxdb считаются устаревшими и могут быть удалены. Значение по умолчанию - 7 дней, минимум - 1 день.

```
{
    ...
    "serverStatus": {
        "status": {
            "refresh": 1,
            "retention": 7
        },
    }
    ...
}
```

### Model Allowed Unsaved Interval

`integer`

Определяет количество времени от последнего сохранения модели, после исчерпания которого модель будет помечена как подверженная риску до следующего сохранения. Значение по умолчанию - 10800. 

```
{
    ...
    "modelAllowedUnsavedInterval": 10800
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
