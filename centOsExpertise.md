# Экспертиза: Особенности касающиеся CentOS 8.

## CentOS 8:
### Перед установкой системы контейнеризации Docker на Centos 8, проверьте следующие пункты:
Если ваша ОС управляется firewalld, то проверьте наличие включенного маскарада. В случае включенного маскарада, 
результат команды `firewall-cmd --zone=public --query-masquerade` должен быть `yes`. Для включения маскарада используйте
 команду `firewall-cmd --zone=public --add-masquerade --permanent`
 

[Вернуться к экспертизе <](expertise.md)

[Вернуться к оглавлению <<](index.md)