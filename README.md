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
+ kibana.yml – параметр elasticsearch.requestHeadersWhitelist:
+ kibana.yml – параметр csp.strict:
