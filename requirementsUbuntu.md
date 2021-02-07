# Требования к клиентским серверам для установки дистрибутива на Ubuntu:

```
#!/bin/bash

# Инструкция тестировалась на Ubuntu 18.04 LTS
# Требует запуск под root пользователем
# Версия: 4

# Флаг для переключения скрипта в strict mode (остановка скрипта при ошибках)
set -ex

# Обновляем информацию о пакетах
apt-get update

# Устанавливаем новые пакеты
apt-get install -y build-essential sudo md5deep uuid zip unzip curl dirmngr software-properties-common \
lxc lxc-templates cgroup-lite libvirt-clients debootstrap redir bridge-utils libc6 \
dnsmasq nano

# Устанавливаем vagrant пакет
cd /tmp
wget -c https://releases.hashicorp.com/vagrant/2.2.4/vagrant_2.2.4_x86_64.deb
dpkg -i vagrant_2.2.4_x86_64.deb

# Устанавливаем LXC плагин для vagrant
wget -c https://nextcloud.optimacros.com/s/eTrjXGSYDcCGyST/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem
```

Пояснения к требованиям, всё что описано в bash скрипте является требованиями к установке воркспейса.
Логин центру же необходима только установка Докера.

[Вернуться к содержанию <<](contents.md)

[Вернуться к оглавлению <<<](index.md)