# Экспертиза по обновлению версии ядра linux на CentOS.

Проведение обновления версии ядра linux на CentOS:

Первое что нужно сделать это установить репозиторий elrepo с помощью последовательного выполнения команд:

`sudo dnf -y install https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm`

`rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org`

Далее можно убедиться, что репозиторий добавлен (не обязательный пункт):

`cat /etc/yum.repos.d/elrepo.repo`

Потому как устанавливать нужно версию ядра lt посмотрим список доступных версий ядра с помощью команды:

`sudo dnf --disablerepo="*" --enablerepo="elrepo-kernel" list available | grep kernel-lt`

Можем видеть что доступна lt версия 5.4

![](./pictures/coreVersions.png)

Установим её с помощью команды:

`sudo dnf --enablerepo=elrepo-kernel install kernel-lt`

![](./pictures/installCoreLt.png)

После того как увидим Complete в выводе после установки, переходим к установке dev и header пакетов ядра, вводим 
команду:

`sudo dnf --enablerepo=elrepo-kernel install kernel-lt-{devel,headers}`

![](./pictures/installHeader.png)

После чего нужно перезагрузить сервер.

`sudo reboot`

В случае, если после перезагрузки сервера мы видим следующее, то мы как раз переходим к ключевой цели написания данной 
инструкции.

Бывает такое, что после перезагрузки сервера, вводя команду вывода версии ядра:

`uname -r`

![](./pictures/unameR.jpg)

В таком случае нам нужно обновить загрузчик.

Смотрим содержимое файла grub.cfg: 

`cat /boot/grub2/grub.cfg`

На самом деле нам важен больше факт его наличия, если он есть, это говорит о том что установлен загрузчик grub2.

Обновляем загрузчик последовательным выполнением команд:

`grub2-set-default 0`

`grub2-mkconfig -o /boot/grub2/grub.cfg`

Перезагружаемся:

`sudo reboot`

Смотрим версию ядра, видим что загрузчик подцепил её и видим 5.4. После чего вновь перезагрузимся, чтобы убедиться что 
версия ядра загружается персистентно:


![](./pictures/newCoreVersion.png)

На этом всё...


[Вернуться к экспертизе <](expertise.md)

[Вернуться к оглавлению <<](index.md)


