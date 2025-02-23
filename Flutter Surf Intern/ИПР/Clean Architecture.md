Source: https://codewithandrea.com/articles/flutter-app-architecture-riverpod-introduction/

![[Pasted image 20240801181345.png|280]]
![[Pasted image 20240813111849.png|350]]

Clean Architecture - это архитектурный подход, направленный разделение приложения на несколько слоев, каждый из которых имеет свою конкретную ответственность. Это позволяет сделать приложение более масштабируемым, гибким и легким в поддержке.

### Layers: 

#### The Presentation Layer

Слой презентации или UI содержит следующие компоненты (types):

- Widgets - содержит классы, из которых строится UI
- Controllers - имеет основные классы контроллеров для реализации управления состоянием в приложении

#### The Domain Layer

Слой домена, содержащий интерфейсы, сущности, usecases, которые оборачивают данные, возвращаемые с слоя данных, для слоя презентации.

- Entities - сущности, представляющие собой обертку для данных для классов моделей 
- Repository - содержит интерфейсы репозиториев, используемые в usecases
- Usecase - классы-сервисы, которые позволяют удобно обращаться к желаемым данным без нужды вызова репозиториев вручную.

#### The Data Layer

Слой данных отвечающий за работу с удаленными (API) и локальными (DB) хранилищами, получение данных и их обработку.

- Data_sources - содержит remote (удаленные хранилища: API services) и local (локальные хранилища: DB, local storages)
- Models - классы, содержащие полученные данные с источников. Имеют методы для сериализации и десериализации.
- Repository - классы, занимающиеся обработкой (например, на ошибки) и предоставлением данных

Также clean architecture может реализовываться во Flutter посредством разбиения сначала на фичи (by-features), затем на слои (by-layers), и потом на типы (by-types). 

Отдельно в структуре папок проекта может использоваться компонент core, содержащий разнообразные абстрактные классы, расширения, константы, которые могут быть применены разными фичами.

**Отличия от стейт-менеджмента**

Clean architecture содержит логику управления состоянием в слое презентации. Это лишь часть слоя данной архитектуры.

**Отличия от архитектурных паттернов (MVP, MVC)**

Clean architecture имеет более гибкую структуру (кольцевую), позволяющую создавать легко-поддерживаемые и легко-масштабируемые приложения в сравнении с MVC, MVP.