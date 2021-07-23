# Cheat sheet elk_misconfig 
<img width="467" alt="image" src="https://user-images.githubusercontent.com/49746472/126661222-4deb00f4-920b-46a8-8d07-4e7c1eeba249.png">

## Configs


* **elasticsearch.yml**

* **kibana.yml**

* **logstash.yml**

-------------------------------------------
## Potential vulnerable parameters
```
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

+ kibana.yml – параметр elasticsearch.ssl.certificate, elasticsearch.ssl.key:

Пути к клиентскому сертификату X.509 в кодировке PEM и соответствующему закрытому ключу. Они используются Kibana для аутентификации при выполнении исходящих SSL/TLS-подключений к Elasticsearch. Чтобы этот параметр вступил в силу, для xpack.security.http.ssl.client_authentication параметра Elasticsearch также необходимо установить значение "required"или "optional" И запросить сертификат клиента у Kibana
```
-------------------------------------------
## Painless script can be used to attack to your system

#### Filter context
```sh
GET seats/_search
{
  "query": {
    "bool": {
      "filter": {
        "script": {
          "script": {
            "source": "doc['sold'].value == false && doc['cost'].value < params.cost",
            "params": {
              "cost": 25
            }
          }
        }
      }
    }
  }
}
```
#### Condition context
```sh
POST _watcher/watch/_execute
{
  "watch" : {
    "trigger" : { "schedule" : { "interval" : "24h" } },
    "input" : {
      "search" : {
        "request" : {
          "indices" : [ "seats" ],
          "body" : {
            "query" : {
              "term": { "sold": "true"}
            },
            "aggs" : {
              "theatres" : {
                "terms" : { "field" : "play" },
                "aggs" : {
                  "money" : {
                    "sum": { "field" : "cost" }
                  }
                }
              }
            }
          }
        }
      }
    },
    "condition" : {
      "script" :
      """
        return ctx.payload.aggregations.theatres.buckets.stream()       
          .filter(theatre -> theatre.money.value < 15000 ||
                             theatre.money.value > 50000)               
          .count() > 0                                                  
      """
    },
    "actions" : {
      "my_log" : {
        "logging" : {
          "text" : "The output of the search was : {{ctx.payload.aggregations.theatres.buckets}}"
        }
      }
    }
  }
}
```
