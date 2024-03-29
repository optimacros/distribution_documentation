# Настройка DNS Воркспейса

По умолчанию в контейнере работает DNS с адресом 10.0.3.1:53

Обычно на данном адресе висит DNS сервис хостовой ОС (dnsmasq)

Если необходимо переопределить DNS для Воркспейса, то можно использовать свойство манифеста [`container.dns`](workspaceManifestInfo.md#DNS)

Если в DNS нужное доменное имя не резолвится, его можно прописать через свойство манифеста [`container.hosts`](workspaceManifestInfo.md#Hosts)

Например имеем адрес LC `адресЛогинЦентра.ru`, который не прописан в DNS, поэтому его укажем в блок `hosts`. В качестве IPv4 адреса используем адреc сервера на котором LC доступен, например`10.11.12.13`.

```
"hosts": {
        "адресЛогинЦентра.ru": "10.11.12.13"
 },
```

После данных настроек файла манифеста, если воркспейс уже был запущен, его нужно остановить и запустить заново с помощью 
последующего выполнения команд:

```/om/workspace-installer/current/install workspace --path /om/workspace1/manifest.json shutdown```  <= остановка 
работы воркспейса

```/om/workspace-installer/current/install workspace --path /om/workspace1/manifest.json up``` <= запуск работы 
воркспейса.

Если воркспейс не был запущен ранее, то просто запускаем его работу.

[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)
