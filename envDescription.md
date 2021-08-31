# Описание свойств .env файла для Логин Центра Optimacros:

VERSION - Версия дистрибутива Логин Центра !изменять нельзя
HOSTNAME - !изменяется при смене адреса Login Center
DB_USERNAME - !изменяется при смене имени пользователя MongoDB
DB_PASSWORD - !изменяется при смене пароля MongoDB
GRAYLOG_PASSWORD - !достаточно для смены пароля администратора http://HOSTNAME/logs 
GRAYLOG_PASSWORD_SHA2 - !всегда должен соответвовать SHA256 параметра GRAYLOG_PASSWORD
ADMIN_USERNAME - !не используется после первого запуска Login Center
ADMIN_PASSWORD - !не используется после первого запуска Login Center
WORKSPACE_NAME - !не используется после первого запуска Login Center
WORKSPACE_HOSTNAME - при смене адреса воркспейса


!поля GRAYLOG в последних версиях Логин Центра более не нужны, но их описания на всякий случай оставлены в документации

[Вернуться к оглавлению <<](index.md)