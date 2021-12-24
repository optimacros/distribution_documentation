# Предварительные установки ПО необходимые для работы воркспейса:

### Для работы воркспейса на ОС Ubuntu требуется предварительная установка пакетов. 

Для установки пакетов нужно запустить bash скрипт с содержимым указанным ниже или выполнить эти команды последовательно:

!!!Пакеты vagrant устанавливаются именно тех версий которые указаны в инструкциях.

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
wget -c https://nextcloud.optimacros.com/s/qpJcRNg2AfbA52S/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem
```


### Для работы воркспейса на операционной системе CentOS.

Тело bash скрипта для установки пакетов на CentOS:

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
wget -c https://nextcloud.optimacros.com/s/qpJcRNg2AfbA52S/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/r4RRxyxNJnL7TzN/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
```

### Для работы воркспейса на операционной системе RHEL.

Тело bash скрипта для установки пакетов на RHEL:

```
#!/bin/bash

# Инструкция тестировалась на RHEL 8
# Требует запуск под root пользователем
# Версия: 2

set -ex

cd /tmp

# Должен быть предустановлен репозиторий Epel (Проверьте его наличие командой `yum repolist`)

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
wget -c https://nextcloud.optimacros.com/s/qpJcRNg2AfbA52S/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/Lrqo5HYa3dQa96E/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
```
  
[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)
