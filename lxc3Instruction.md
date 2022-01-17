# Установка lxc 3.0 на CentOS 7:

Создадим новый файл репозитория с помощью команды:

`nano /etc/yum.repos.d/lxc3.0.repo`

Заполним его следующим содержимым и затем сохраним:

```
[thm-lxc3.0]
name=Copr repo for lxc3.0 owned by thm
baseurl=https://copr-be.cloud.fedoraproject.org/results/thm/lxc3.0/epel-7-$basearch/
type=rpm-md
skip_if_unavailable=True
gpgcheck=1
gpgkey=https://copr-be.cloud.fedoraproject.org/results/thm/lxc3.0/pubkey.gpg
repo_gpgcheck=0
enabled=1
enabled_metadata=1
```

Установим репозиторий epem-release с помощью команды:

`yum install epel-release`

Устанавливаем сам lxc 3.0 c помощью команды:

`yum install debootstrap lxc lxc-templates lxc-extra libcap-devel libcgroup`

Так же после всего можно проверить готовность lxc к работе с помощью команды:

`lxc-checkconfig`

Кроме двух строк после: "User namespace: enabled" параметр у всех строк должен быть enabled.

Установка завершена.


[Вернуться к странице: Предварительные установки ПО  <](softInstallWs.md)
  
[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)