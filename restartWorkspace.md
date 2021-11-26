# Перезагрузка воркспейса

`WS_DIR` - воркспейс установлен в директорию `/opt/workspace1` (Ваша директория может быть другой)

`WS_INSTALLER_DIR` - инсталятор воркспейса установлен в директорию `/opt/workspace-installer/current` (Ваша директория может быть другой)

Останавливаем воркспейс:

```
sudo <WS_INSTALLER_DIR>/install workspace --path <WS_DIR>/manifest.json shutdown
```

Запускаем воркспейс:

```
sudo <WS_INSTALLER_DIR>/install workspace --path <WS_DIR>/manifest.json up
```

## Force Shutdown

Форсированная остановка контейнера с воркспейсом
Используется в различных экстренных случаях, например при зависании воркспейса и его панели администрирования

! При форсированной остановке бекап последнего состояния модели сделан не будет, при последующем запуске воркспейса, модели восстановятся на состояние последних доступных бекапов

```
sudo <WS_INSTALLER_DIR>/install workspace --path <WS_DIR>/manifest.json --force shutdown
```
