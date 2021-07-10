# Ручное обновление данных Mysql.

При поднятии воркспейса может произойти прерывание процесса с ошибкой `The database must be manually upgraded`
Это означает что MySQL пакет и директория с данными имеют разные версии, что требует обновления директории с данными до версии mysql пакета

Переходим в директорию с контейнером с помощью команды (директория может быть другой):

`cd /om/workspace1/container`

Затем подключаемся к вагрант контейнеру с помощью команды:

`vagrant ssh`

После чего нам нужно перейти под root пользователя, вводим команду:

`sudo su`
 
Теперь нам необходимо перезагрузить сервис `mariadb` для этого введём команду: 

`systemctl restart mariadb`

Выполняем команду:

`mysql_upgrade`

![](./pictures/mysqlUpgrade.jpg)

Теперь снова нужно перезагрузить сервис `mariadb` для этого введём команду: 

`systemctl restart mariadb`

После этого нам нужно зайти в MySQL терминал с помощью команды:

`mysql`

И в MySQL терминале нужно выполнить команду:
 
`FLUSH PRIVILEGES;`

Затем выполняем команду сброса пользователя root:

```
ALTER USER `root`@`localhost`

IDENTIFIED VIA mysql_native_password

USING PASSWORD("syeAIuL4S5Sw9t9")

OR unix_socket;
```

Выходим из MySQL терминала с помощью команды:

`exit`

И выполняем команду:

`mysql_upgrade`

Видим что обновление проходит как и в первый раз. 

Выполнив еще раз команду:

`mysql_upgrade`

Мы получим сообщение, что обновление `already upgraded`, значит все успешно обновилось
 
Далее выходим из контейнера с помощью команды `exit`

![](./pictures/containerExit.jpg)

Далее нужно перейти в директорию с нашим инсталлятором воркспейса и выполнить штатную остановку воркспейса, но с флагом
`force` т.е. команда будет выглядеть таким образом:

`/om/workspace-installer/current/install workspace --path /om/workspace1/manifest.json shutdown --force`

После этого можно поднимать воркспейс:

`/om/workspace-installer/current/install workspace --path /om/workspace1/manifest.json up`

На этом всё, проблема решена.

[Вернуться к экспертизе <](expertise.md)

[Вернуться к оглавлению <<](index.md)


