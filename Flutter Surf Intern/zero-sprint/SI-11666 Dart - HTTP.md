
Source: https://skillbox.ru/media/code/rest-api-chto-eto-takoe-i-kak-rabotaet/

#### REST API

Rest API - это архитектурный подход, устанавливающий ограничения в разработке API: как они должны быть устроены и какие функции поддерживать.

#### Termines

REST - Respresentational State Transfer (Передача состояния представления).
RESTful - прилагательное для веб-сервисов, соответствующих архитектуре REST.

#### 6 принципов архитектуры REST API

Всего в REST есть шесть требований к проектированию API. Пять из них обязательные, одно — опциональное:

- Клиент-серверная модель (client-server model) - четкое разделение клиента и сервера

- Отсутствие состояния (statelessness) - на сервере не хранится никаких данных о прошлых взаимодействиях с клиентом. То есть, каждый запрос должен содержать всю информацию для его обработки.

- Кэширование (cacheability) - сохранение части частозапрашиваемых данных у клиента или на промежуточных серверах.

- Единообразие интерфейса (uniform interface) - Должен быть **единый способ обращения** к каждому ресурсу. То есть, в ответ на запрос приходит такая же информация, как и для всех, с допустимыми функциями (можно ли изменять, удалять ресурс и т.д.).

- Многоуровневая система (layered system) - наличие промежуточных узлов прокси-серверов, которые используются для кэширования, обеспечения безопасности и т.д.

- Код по требованию (code on demand) — сервер в ответ на запрос может **отправить исходный код**, который выполняется уже на стороне клиента. Благодаря этому можно передавать целые сценарии (НЕОБЯЗАТЕЛЬНО).

#### dart:http | code example

```dart
import 'dart:convert' as convert;

import 'package:http/http.dart' as http;

void main(List<String> arguments) async {
  // This example uses the Google Books API to search for books about http
  var url =
      Uri.https('www.googleapis.com', '/books/v1/volumes', {'q': '{http}'});

  var response = await http.get(url);
  if (response.statusCode == 200) {
    var jsonResponse =
        convert.jsonDecode(response.body) as Map<String, dynamic>;
    var itemCount = jsonResponse['totalItems'];
    print('Number of books about http: $itemCount.');
  } else {
    print('Request failed with status: ${response.statusCode}.');
  }
}
```

! ATTENTION !
----------------------
Ссылка на репозиторий с примером реализации API сервисов (retrofit, dio), HTTP методов (GET, POST, PUT, DELETE), сериализации/десериализации данных: 
https://github.com/paincake00/retrofit-and-dio