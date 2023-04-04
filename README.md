## Данный плейбук позволяет установить clickhouse  vector и lighthouse

## Особенности
<table>
	<tr>
	    <th>Переменная</th>
	    <th>Описание</th>
	</tr >
	<tr >
	    <td>**/playbook/group_vars/**</td>
	    <td>Переменные, которые можно при желании изменить (например версию ПО)  </td>
	</tr>
	<tr>
	    <td>**/group_vars/clickhouse/vars.yml**</td>
	    <td>здесь переменные, отвечающие за кликхаус. Например мы можем поменять версию clickhouse_version: "22.3.3.44" на что-нибудь поновее, например или наоборот постарее https://packages.clickhouse.com/rpm/stable/ Таким образом мы избегаем хардкода и меняем в одном месте
clickhouse-client, clickhouse-server, clickhouse-common-static - имена пакетов (при переходе по ссылке выше можно проверить). Таким способом мы уменьшаем количество строк, поскольку используем цикл (with_items: "{{ clickhouse_packages }}") и тем самым добиваемся лучшей читабельности.  </td>
	</tr>
  	<tr >
	    <td>**/group_vars/vector/vars.yml**</td>
	    <td>здесь переменные, отвечающие за вектор. vector_version: "0.21.1" - это версия вектора, а vector_config_dir: "{{ ansible_user_dir }}/vector_config/"  это директория, которая будет создана и где сгенерируется конфигурационный файл из джинжи темплейта. </td>
	</tr>
	<tr>
	    <td>**/group_vars/lighthouse/vars.yml** </td>
	    <td>здесь переменные, отвечающие за лайтхаус. lighthouse_repo - ссылка на гит репозиторий, lighthouse_location_dir - директория, где распологается лайнхаус.  </td>
 	<tr>
	    <td>**/inventory/prod.yml**</td>
	    <td>переменные хостов и параметров подключения  </td>
	</tr>
 	<tr>
	    <td>**/templates**</td>
	    <td>шаблоны конфигурационных файлов  </td>
	</tr>
</table>


Плейбук позволяет отправить логи nginx c лайтхауса с помощью вектора в кликхаус.  
Пример сбора логов:  
```
ginx-lighthouse-01.ru-central1.internal :) SELECT * FROM nginxdb.access_logs

SELECT *
FROM nginxdb.access_logs

Query id: 5ed10207-9a5a-49b8-9def-0c8cc6079c04

┌─message────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 188.170.XX.XX - - [12/Mar/2023:15:17:02 +0000] "GET / HTTP/1.1" 200 6766 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                                                           │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:03 +0000] "GET /css/bootstrap.css HTTP/1.1" 200 127247 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                   │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:03 +0000] "GET /css/bootstrap.css HTTP/1.1" 200 127247 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                   │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:03 +0000] "GET /img/loading.svg HTTP/1.1" 200 531 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                        │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:03 +0000] "GET /js/ag-grid.min.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                        │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:03 +0000] "GET /js/ace-min/ace.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                        │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /js/ace-min/clickhouse_highlight_rules.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0" │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /js/ace-min/ch_completions_help.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"        │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /js/ace-min/ext-language_tools.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"         │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /jquery.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                                │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /js/bootstrap.js HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                          │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /app.js?v10 HTTP/1.1" 304 0 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                               │
│ 188.170.XX.XX - - [12/Mar/2023:15:17:04 +0000] "GET /favicon.ico HTTP/1.1" 404 153 "http://158.160.60.229/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:109.0) Gecko/20100101 Firefox/110.0"                            │
└────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────┘

13 rows in set. Elapsed: 0.002 sec.

```
# Команда для запуска:  
```
ansible-playbook -i inventory/prod.yml site.yml
```

## Примечание: playbook работает только с пакетным менеджером yum  

## Теги:  
- clickhouse  
- vector  
- lighthouse  

