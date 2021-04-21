# Особенности установки на чистый Ubuntu 18.04:

Для Ubuntu 18.04 пакет lxc требует наличия пакета apparmor.
Без него система контейнеров не запустит контейнер.
Обычно в релизе Ubuntu 18 данный пакет уже установлен.
Для проверки наличия можно использовать команду:

`dpkg -l | grep apparmor`

Ответ будет примерно таким:

`
root@host:~# dpkg -l | grep apparmor
ii  apparmor                         2.12-4ubuntu5.1                     amd64        user-space parser utility for AppArmor
ii  libapparmor1:amd64               2.12-4ubuntu5.1                     amd64        changehat AppArmor library
`

Команда установки из репозитория:

`apt-get install apparmor`

После установки, необходимо перезапустить сервис lxc, делается с помощью команды: 

`systemctl restart lxc`


[Вернуться к экспертизе <](expertise.md)

[Вернуться к оглавлению <<](index.md)