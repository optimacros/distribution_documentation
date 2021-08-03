# Особенности установки на чистый Ubuntu 18.04:

## Apparmor

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

## Настройки работы выделения памяти на сервере

Для стабильной работы Optimacros параметр сервера `vm.overcommit_memory` должен быть равен `1`

Данные параметр обычно на чистой ОС установлен по-умолчанию как `1`, в случае если у вас установлено не `1`, необходимо поменять на `1`

`sysctl -w -p vm.overcommit_memory=1`

[Вернуться к экспертизе <](expertise.md)

[Вернуться к оглавлению <<](index.md)
