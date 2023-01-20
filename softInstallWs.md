# Предварительные установки ПО необходимые для работы воркспейса:

### Для работы воркспейса на ОС Debian требуется предварительная установка пакетов.

Для установки пакетов нужно запустить bash скрипт с содержимым указанным ниже или выполнить эти команды последовательно:

#### ! Версии пакетов vagrant устанавливаются именно те, что указаны в скрипте.

```
#!/bin/bash

# Инструкция тестировалась на Debian 10
# Требуются права root пользователя
# Версия: 4

# Флаг для переключения скрипта в strict mode (остановка скрипта при ошибках)
set -ex

# Обновляем информацию о пакетах
apt-get update

# Устанавливаем новые пакеты
apt-get install -y software-properties-common lxc lxc-templates bridge-utils redir tar zip unzip curl wget

# Устанавливаем vagrant пакет
cd /tmp
wget -c https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
dpkg -i vagrant_2.2.19_x86_64.deb

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Настраиваем сеть lxc-net

cat <<EOT > /etc/lxc/default.conf
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx
EOT

cat <<EOT > /etc/default/lxc-net
USE_LXC_BRIDGE="true"
LXC_BRIDGE="lxcbr0"
LXC_ADDR="10.0.3.1"
LXC_NETMASK="255.255.255.0"
LXC_NETWORK="10.0.3.0/24"
LXC_DHCP_RANGE="10.0.3.2,10.0.3.254"
LXC_DHCP_MAX="253"
EOT

systemctl enable lxc-net
systemctl restart lxc-net
```

### Для работы воркспейса на ОС Ubuntu требуется предварительная установка пакетов.

Для установки пакетов нужно запустить bash скрипт с содержимым указанным ниже или выполнить эти команды последовательно:

!!! Пакеты vagrant устанавливаются именно тех версий которые указаны в инструкциях.

```
#!/bin/bash

# Инструкция тестировалась на Ubuntu 18.04/20.04 LTS
# Требует запуск под root пользователем
# Версия: 4

# Флаг для переключения скрипта в strict mode (остановка скрипта при ошибках)
set -ex

# Обновляем информацию о пакетах
apt-get update

# Устанавливаем новые пакеты
apt-get install -y software-properties-common lxc lxc-templates bridge-utils redir tar zip unzip curl wget

# Устанавливаем vagrant пакет
cd /tmp
wget -c https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
dpkg -i vagrant_2.2.19_x86_64.deb

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem
```


### Для работы воркспейса на операционной системе CentOS 8.

Тело bash скрипта для установки пакетов на CentOS:

```
#!/bin/bash

# Инструкция тестировалась на Centos 8, а так же для CentOS Stream release 8
# Требует запуск под root пользователем
# Версия: 2

set -ex

cd /tmp

yum -y install epel-release
yum -y install nano tar zip unzip curl wget lxc lxc-templates

# Устанавливаем vagrant
dnf install https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
vagrant --version

# Включить USE_LXC_BRIDGE в /etc/sysconfig/lxc
sed -E 's|(USE_LXC_BRIDGE=")false(")|\1true\2|' -i /etc/sysconfig/lxc

# Запуск LXC
systemctl restart lxc-net
systemctl enable lxc-net

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/j3Xwn2MGzsLRTES/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
```

### Для работы воркспейса на операционной системе CentOS 7.

Тело bash скрипта для установки пакетов на CentOS:

!!! Читайте комментарий после тела скрипта!!!

```
#!/bin/bash

# Инструкция тестировалась на CentOS Linux release 7.9.2009 (Core)
# Требует запуск под root пользователем
# Версия: 2

set -ex

cd /tmp

yum -y install epel-release
yum -y install nano tar zip unzip curl wget lxc lxc-templates

# Устанавливаем vagrant
yum install https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
vagrant --version

# Включить USE_LXC_BRIDGE в /etc/sysconfig/lxc
sed -E 's|(USE_LXC_BRIDGE=")false(")|\1true\2|' -i /etc/sysconfig/lxc

# Запуск LXC
systemctl restart lxc-net
systemctl enable lxc-net

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/j3Xwn2MGzsLRTES/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir

```
!!! По поводу версии lxc, она по умолчанию устанавливается в данном случае первой версии, а нам нужна третья.
Для её установки переходим по [ссылке с инструкцией](lxc3Instruction.md)

После установки 3-ей версии lxc проблем с воркспейсами не возникнет.

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

yum -y install nano tar zip unzip curl wget lxc lxc-templates

# Устанавливаем vagrant
dnf install https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
vagrant --version

# Включить USE_LXC_BRIDGE в /etc/sysconfig/lxc
sed -E 's|(USE_LXC_BRIDGE=")false(")|\1true\2|' -i /etc/sysconfig/lxc

# Запуск LXC
systemctl restart lxc-net
systemctl enable lxc-net

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/j3Xwn2MGzsLRTES/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir
```

## Для работы воркспейса на операционной системе Astra Linux

Тело bash скрипта для установки пакетов на Astra Linux:

```
# Инструкция тестировалась на Astra Linux 1.7 SE (Смоленск)
# Требует запуск под root пользователем
# Версия: 1

# Флаг для переключения скрипта в strict mode (остановка скрипта при ошибках)
set -ex

# раскомментировать или добавить параметр в /etc/systemd/logind.conf

if [[ $(grep 'KillUserProcesses' /etc/systemd/logind.conf) ]]; then sed -i '/KillUserProcesses/s/^#//' /etc/systemd/logind.conf; sed -i '/KillUserProcesses/s/yes/no/' /etc/systemd/logind.conf; else echo 'KillUserProcesses=no' >> /etc/systemd/logind.conf; fi

# после правки logind.conf необходимо перезапустить сервис
systemctl restart systemd-logind.service

cd /tmp

# Устанавливаем корневые сертификаты

apt-get install ca-certificates

# Устанавливаем пакет управления сетевыми мостами

apt-get install bridge-utils

# Устанавливаем дополнительный пакет для работы DHCP сети LXC

apt-get install dnsmasq-base

# Устанавливаем LXC

wget http://ftp.ru.debian.org/debian/pool/main/l/lxc/liblxc1_3.1.0+really3.0.3-8_amd64.deb
wget http://ftp.ru.debian.org/debian/pool/main/l/lxc/lxc_3.1.0+really3.0.3-8_amd64.deb

dpkg -i liblxc1_3.1.0+really3.0.3-8_amd64.deb
dpkg -i lxc_3.1.0+really3.0.3-8_amd64.deb

# Устанавливаем пакет для проброса портов LXC контейнеров

wget http://ftp.ru.debian.org/debian/pool/main/r/redir/redir_3.2-1_amd64.deb
dpkg -i redir_3.2-1_amd64.deb

# Устанавливаем утилиту Vagrant для управления LXC контейнерами
wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
wget https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz

dpkg -i vagrant_2.2.19_x86_64.deb
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Настраиваем сеть lxc-net

cat <<EOT > /etc/lxc/default.conf
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx
EOT

cat <<EOT > /etc/default/lxc-net
USE_LXC_BRIDGE="true"
LXC_BRIDGE="lxcbr0"
LXC_ADDR="10.0.3.1"
LXC_NETMASK="255.255.255.0"
LXC_NETWORK="10.0.3.0/24"
LXC_DHCP_RANGE="10.0.3.2,10.0.3.254"
LXC_DHCP_MAX="253"
EOT

systemctl enable lxc-net
systemctl restart lxc-net
```

> ! ВНИМАНИЕ. В случае если нету доступа к интернет репозиториям /astra/stable/1.7_x86-64/repository-extended main
Пользуйтесь разделом "# Устанавливаем LXC" из инструкции для сборки Смоленск ("Инструкция тестировалась на Astra Linux 1.7 SE (Смоленск)")

Проверить список подключеных репозиториев можно командой:
```
grep ^[^#] /etc/apt/sources.list /etc/apt/sources.list.d/*
```

```
# Инструкция тестировалась на Astra Linux 1.7 SE (Воронеж)
# Требует запуск под root пользователем

# Флаг для переключения скрипта в strict mode (остановка скрипта при ошибках)
set -ex

# раскомментировать или добавить параметр в /etc/systemd/logind.conf

if [[ $(grep 'KillUserProcesses' /etc/systemd/logind.conf) ]]; then sed -i '/KillUserProcesses/s/^#//' /etc/systemd/logind.conf; sed -i '/KillUserProcesses/s/yes/no/' /etc/systemd/logind.conf; else echo 'KillUserProcesses=no' >> /etc/systemd/logind.conf; fi

# после правки logind.conf необходимо перезапустить сервис
systemctl restart systemd-logind.service

cd /tmp

# Устанавливаем корневые сертификаты

apt-get install ca-certificates

# Устанавливаем пакет управления сетевыми мостами

apt-get install bridge-utils

# Устанавливаем дополнительный пакет для работы DHCP сети LXC

apt-get install dnsmasq-base

# Устанавливаем LXC из интернет репозитория /astra/stable/1.7_x86-64/repository-extended 1.7_x86-64/main

apt install lxc lxc-astra

# Устанавливаем пакет для проброса портов LXC контейнеров

wget http://ftp.ru.debian.org/debian/pool/main/r/redir/redir_3.2-1_amd64.deb
dpkg -i redir_3.2-1_amd64.deb

# Устанавливаем утилиту Vagrant для управления LXC контейнерами
wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
wget https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz

dpkg -i vagrant_2.2.19_x86_64.deb
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem
```

## Для работы воркспейса на операционной системе ALT Linux 9

[Особенности установки под ALT Linux 9](installToAltServer.md)

## Для работы воркспейса на операционной системе РЕД ОС

Тело bash скрипта для установки пакетов на РЕД ОС:

```
#!/bin/bash

# Инструкция тестировалась на РЕД ОС 7.3
# Требует запуск под root пользователем
# Версия: 1

set -ex

cd /tmp

yum -y install nano tar zip unzip curl wget dnsmasq lxc lxc-templates

# Устанавливаем vagrant
dnf install https://releases.hashicorp.com/vagrant/2.2.6/vagrant_2.2.6_x86_64.rpm
vagrant --version

# Включить USE_LXC_BRIDGE в /etc/sysconfig/lxc
sed -E 's|(USE_LXC_BRIDGE=")false(")|\1true\2|' -i /etc/sysconfig/lxc

# Настраиваем сеть lxc-net

cat <<EOT > /etc/lxc/default.conf
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
lxc.net.0.hwaddr = 00:16:3e:xx:xx:xx
EOT

# Запуск LXC
systemctl restart lxc-net
systemctl enable lxc-net

# Устанавливаем LXC плагин для vagrant
wget -c https://github.com/optimacros/vagrant-lxc/releases/download/v1.4.5/vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem

# Устанавливаем redir
wget -c https://nextcloud.optimacros.com/s/j3Xwn2MGzsLRTES/download -O redir_3.3_centos8_x86_64.zip
unzip redir_3.3_centos8_x86_64.zip
cp redir /usr/bin/redir
chmod +x /usr/bin/redir

```





<hr>

[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)
