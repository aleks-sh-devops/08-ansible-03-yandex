## Данный плейбук позволяет установить clickhouse  vector и lighthouse

## Особенности
**/playbook/group_vars/** - переменные, которые можно при желании изменить (например версию ПО)  

**/group_vars/clickhouse/vars.yml** - здесь переменные, отвечающие за кликхаус. Например мы можем поменять версию clickhouse_version: "22.3.3.44" на что-нибудь поновее, например или наоборот постарее https://packages.clickhouse.com/rpm/stable/ Таким образом мы избегаем хардкода и меняем в одном месте
clickhouse-client, clickhouse-server, clickhouse-common-static - имена пакетов (при переходе по ссылке выше можно проверить). Таким способом мы уменьшаем количество строк, поскольку используем цикл (with_items: "{{ clickhouse_packages }}") и тем самым добиваемся лучшей читабельности.  

**/group_vars/vector/vars.yml** - здесь переменные, отвечающие за вектор. vector_version: "0.21.1" - это версия вектора, а vector_config_dir: "{{ ansible_user_dir }}/vector_config/"  это директория, которая будет создана и где сгенерируется конфигурационный файл из джинжи темплейта.  

**/group_vars/lighthouse/vars.yml** - здесь переменные, отвечающие за лайтхаус. lighthouse_repo - ссылка на гит репозиторий, lighthouse_location_dir - директория, где распологается лайнхаус.  

**/inventory/prod.yml** - переменные хостов и параметров подключения  
**/templates** - шаблоны конфигурационных файлов  

Плейбук позволяет отправить логи nginx c лайтхауса с помощью вектора в кликхаус.  

# Команда для запуска:  
```
ansible-playbook -i inventory/prod.yml site.yml
```

## Примечание: playbook работает только с пакетным менеджером yum  

## Теги:  
- clickhouse  
- vector  
- lighthouse  

