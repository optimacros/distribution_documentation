# Особенности установки на ALT Server 9

`! Установка Optimacros производится под пользователем root`

## Provision

Установим базовые утилиты для дальнейшей работы

```
apt-get install nano zip unzip net-tools telnet htop
```

### Docker

Устанавливаем из репозитория

```
apt-get install docker-ce
```

Запускаем сервис и добавляем его в автозапуск

```
systemctl start docker
systemctl enable docker
```

### LXC

Устанавливаем из репозитория

```
apt-get install lxc lxc-net bridge-utils
```

Редактируем подсеть `lxc-net`

```
nano /etc/sysconfig/lxc-net
```

Меняем подсеть

```
LXC_BRIDGE="lxcbr0"
LXC_ADDR="10.0.3.1"
LXC_NETMASK="255.255.255.0"
LXC_NETWORK="10.0.3.0/24"
LXC_DHCP_RANGE="10.0.3.2,10.0.3.254"
LXC_DHCP_MAX="253"
```

Запускаем сервис и добавляем его в автозапуск

```
systemctl start lxc-net
systemctl enable lxc-net
systemctl start lxc
systemctl enable lxc
```


### Vagrant

Устанавливаем vagrant из пакета

```
cd /tmp
wget https://releases.hashicorp.com/vagrant/2.2.4/vagrant_2.2.4_x86_64.rpm
rpm -ivh vagrant_2.2.4_x86_64.rpm
```

Устанавливаем vagrant-lxc

```
cd /tmp
wget -c https://nextcloud.optimacros.com/s/eTrjXGSYDcCGyST/download -O vagrant-lxc.tar.gz
tar -zxvf vagrant-lxc.tar.gz
vagrant plugin install  --plugin-clean-sources vagrant-lxc.gem
```

## Установка Login Center

Установка не отличается от стандартной

## Установка Workspace

Предварительно нужно уставить утилиту redir, далее установка не отличается от стандартной

### Redir

Для работы форвардинга портов на воркспейсе необходима утилита `redir`. 
В стандартном репозитории ее нет, поэтому необходимо скачать 
исходники и установить ее из исходников

```
apt-get install autoconf automake gcc git
cd /opt
git clone https://github.com/troglobit/redir
cd redir
./autogen.sh
./configure --prefix=/usr
make -j5
make install-strip
```

Можно проверить наличие утилиты
```
which redir
```