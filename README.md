# elk_misconfig
<img width="467" alt="image" src="https://user-images.githubusercontent.com/49746472/126661222-4deb00f4-920b-46a8-8d07-4e7c1eeba249.png">

Configs:


* **elasticsearch.yml**

* **kibana.yml**

* **logstash.yml**

+ kibana.yml – параметр server.ssl.enabled: 
Включает SSL/TLS для входящих подключений к Kibana. Если установлено значение true, необходимо предоставить сертификат и соответствующий ему закрытый ключ. Их можно указать с помощью server.ssl.keystore.path или комбинации server.ssl.certificate и server.ssl.key. По умолчанию: false

+ kibana.yml – параметр server.xsrf.disableProtection:
Установка этого значения true полностью отключит защиту от подделки межсайтовых запросов в Kibana. Это не рекомендуется. По умолчанию:false

+ kibana.yml – параметр elasticsearch.ssl.verificationMode:
Управляет проверкой сертификата сервера, который получает Kibana при создании исходящего SSL/TLS-соединения с Elasticsearch. Допустимые значения "full", "certificate"и "none". Использование "full"выполняет проверку имени хоста, использование "certificate"пропускает проверку имени хоста, а использование "none"полностью пропускает проверку. По умолчанию:"full"

+ kibana.yml – параметр elasticsearch.requestHeadersWhitelist:

Список клиентских заголовков Kibana для отправки в Elasticsearch. Чтобы отправить не на стороне клиента заголовки, установите значение [] (пустой список). Удаление authorization заголовка из белого списка означает, что вы не можете использовать базовую аутентификацию в Kibana. По умолчанию:[ 'authorization' ]

+ kibana.yml – параметр csp.strict:

Блокирует доступ Kibana к любому браузеру, который не применяет даже элементарные правила CSP. На практике это отключает поддержку старых, менее безопасных браузеров, таких как Internet Explorer. По умолчанию:true
