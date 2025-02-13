# Интеграция Node-RED с сервисом DaData

Этот проект предоставляет пример потока Node-RED для интеграции с сервисом DaData. Интеграция позволяет получать актуальные данные о банках, компаниях, Индивидуальных предпринимателях и передавать их в вашу CRM систему для формирования договоров и других документов.
Пройдите простой процесс регистрации на сайте [DaData](https://dadata.ru/) и получите Токен. 
На бесплатном тарифе доступно до **10 000 запросов в день**. 

![Описание фото](https://github.com/NodeRedBPM/DaData-Node-Red/blob/main/dadataflow.png)
---
## Как использовать
1. Скачайте файл [dadataflows.json](https://github.com/NodeRedBPM/DaData-Node-Red/blob/main/dadataflow.json) из этого репозитория.

2. Импортируйте его в Node-RED.
   
![Описание фото](https://github.com/NodeRedBPM/MegaCRM-API-Node-Red/blob/main/importfile.png)
---
## Возможности интеграции

### Банки
- Получение данных о банках по БИК, SWIFT, ИНН.
- Подсказки по банкам.
- Информация о БИК, корр. счете, SWIFT.
- Данные об ИНН, КПП.
- Статус банка.

### Компании и ИП
- Проверка статуса бизнеса.
- Получение юридического адреса.
- Основной ОКВЭД.
- Данные об ИНН, КПП, ОГРН.
- Информация о ОКПО, ОКОГУ, ОКФС.
- Данные о ОКТМО, ОКАТО.

### Дополнительные возможности предоставленные блоком обработки ответа
- Автоматическое заполнение данных для формирования договоров и других документов.
- Вывод значений пола руководителя и данных Ф.И.О. в родительном падеже и в сокращённом виде для подписи:
  - `management`: "Иванова Марианна Владимировна"
  - `managementdoc`: "Ивановой Марианны Владимировны"
  - `managementdocsh`: "Иванова М.В."
  - `gender`: "female"

## Описание потока Node-RED

### Основные узлы потока

1. **Формирование запроса**:
   - Узел `function` для формирования HTTP-запроса к API DADATA.
   - Устанавливает заголовки и тело запроса.

2. **Запрос к API DADATA**:
   - Узел `http request` для отправки запроса к API DADATA.
   - Использует метод POST и возвращает ответ в формате JSON.

3. **Обработка ответа**:
   - Узел `function` для обработки ответа от API DADATA.
   - Извлекает необходимые данные и формирует структурированный ответ.

4. **Вывод результата**:
   - Узел `debug` для вывода результата в консоль Node-RED.

### Пример запроса и ответа

#### Запрос данных о банке по БИК
```json
{
  "headers": {
    "Content-Type": "application/json",
    "Authorization": "Token YOUR_API_KEY"
  },
  "payload": {
    "query": "044525225"
  }
}
```

#### Ответ от API DADATA
```json
{
  "name_payment": "Сбербанк",
  "name_full": "ПАО Сбербанк",
  "bic": "044525225",
  "swift": "SABRRUMM",
  "kassch": "30101810400000000225",
  "address1": "г Москва, ул Вавилова, д 19"
}
```
#### Запрос данных о компании по ИНН или ОГРН
```json
{
  "headers": {
    "Content-Type": "application/json",
    "Authorization": "Token YOUR_API_KEY"
  },
  "payload": {
    "query": "7707083893"
  }
}
```
#### Ответ от API DADATA
```json
{
  "type": "Организация",
  "name_short": "ПАО Сбербанк",
  "inn": "7707083893",
  "kpp": "773601001",
  "ogrn": "1027700132195",
  "address": "г Москва, ул Вавилова, д 19",
  "management": "Греф Герман Оскарович",
  "managementdoc": "Грефа Германа Оскаровича",
  "managementdocsh": "Греф Г.О.",
  "gender": "male"
}
```

#### Запрос данных о индивидуальном предпринимателе по ИНН или ОГРНИП
```json
{
  "headers": {
    "Content-Type": "application/json",
    "Authorization": "Token YOUR_API_KEY"
  },
  "payload": {
    "query": "1234567890"
  }
}
```

#### Ответ от API DADATA
```json
{
  "type": "Индивидуальный предприниматель",
  "name_short": "ИП Греф Г.О.",
  "inn": "1234567890",
  "kpp": "",
  "ogrn": "1234567890123",
  "address": "г Москва",
  "management": "Греф Герман Оскарович",
  "managementdoc": "Грефа Германа Оскаровича",
  "managementdocsh": "Греф Г.О.",
  "gender": "male",
  "name_ip": "ИНДИВИДУАЛЬНЫЙ ПРЕДПРИНИМАТЕЛЬ ГРЕФ ГЕРМАН ОСКАРОВИЧ",
  "name_short_up": "ИП ГРЕФ ГЕРМАН ОСКАРОВИЧ"
}
```
---
## Лицензия
Этот проект распространяется под лицензией MIT. Подробнее см. в файле [LICENSE](LICENSE).
