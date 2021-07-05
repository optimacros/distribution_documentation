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
