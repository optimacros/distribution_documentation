# Обновление воркспейса на версию >= 2.3.2

Перед запуском обновления воркспейса следует проверить файл `manifest.json`

## Блок container 
### Ports
Если настроено перенаправление портов, с версии 2.3.2 и выше порты `8080`, `8081` более не используются. Поэтому строки подобного вида убираем.
Например было:
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
Приводится к виду:
```
{
    ...
    "ports": {
        "0.0.0.0:80": 80,
        "0.0.0.0:443": 443
    },
    ...
}
```

## Если логин центр установлен на одном сервере с воркспейсом

Переходим в каталог с установленным логин центром и останавливаем его коммандой
    
    ./ om stop

Переходим в каталог `data/nginx/templates`
Шаблон с именем ws `ws8081.conf.template` удаляем, и заменяем 

[ws443.conf.template](wsProxyTemplates/ws443.conf.new.template)

После этого запускаем логин центр

    ./ om start

И возвращаемся к обновлению воркспейса. 

[Продолжить обновление](refreshWs.md)

[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)