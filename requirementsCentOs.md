# Требования к клиентским серверам для установки дистрибутива на CentOS :

```
#!/bin/bash

# Инструкция тестировалась на Centos 8
# Требует запуск под root пользователем
# Версия: 2

set -ex

cd /tmp

yum -y install epel-release
yum -y install nano tar zip unzip curl wget lxc lxc-templates dnsmasq

# Устанавливаем vagrant
dnf install https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
vagrant --version

# Включить USE_LXC_BRIDGE в /etc/sysconfig/lxc
sed -E 's|(USE_LXC_BRIDGE=")false(")|\1true\2|' -i /etc/sysconfig/lxc

# Запуск LXC
systemctl restart lxc-net
systemctl enable lxc-net

# Устанавливаем LXC плагин для vagrant
wget -c https://nextcloud.optimacros.com/s/eTrjXGSYDcCGyST/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/wqZpZcSD6YcE7LP/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
¸ есть проблема с дублированием блока конигурации в lxc файле после 
# рестарта контейнера, то можно использовать фикс 
# https://github.com/boltronics/vagrant-lxc/commit/ba8d6ac630ba04f9bf017b743af5e8251dd86c84

# Устанавливаем LXC плагин для vagrant
wget -c https://nextcloud.optimacros.com/s/eTrjXGSYDcCGyST/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

wget -c https://nextcloud.optimacros.com/s/wqZpZcSD6YcE7LP/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
```

Пояснения к требованиям, всё что описано в bash скрипте является требованиями к установке воркспейса.
Логин центру же необходима только установка Докера.

Дополнительная экспертиза:
```
Make sure to set the Linux kernel `overcommit` memory setting to `1`. Add `vm.overcommit_memory = 1` to `/etc/sysctl.conf` 
and then reboot or run the command `sysctl vm.overcommit_memory=1` for this to take effect immediately.
```



[Вернуться к содержанию <<](contents.md)

[Вернуться к оглавлению <<<](index.md)