# Требования к клиентским серверам для установки дистрибутивов:

[Предварительная установка ПО необходимое для работы Воркспейса Optimacros](softInstallWs.md)

[Предварительная установка ПО необходимое для работы Логин Центра Optimacros](softInstallLc.md)

## Настройки sysctl

Проверьте что значение параметра `vm.overcommit_memory` равно 0 или 1

```
cat /proc/sys/vm/overcommit_memory
```

Если это не так, то установите корректное значение `vm.overcommit_memory = 1` в файл `/etc/sysctl.conf`, после чего перезагрузите машину или выполните команду обновления sysctl `sysctl -p`

[Вернуться к содержанию <](contents.md)

[Вернуться к оглавлению <<](index.md)
