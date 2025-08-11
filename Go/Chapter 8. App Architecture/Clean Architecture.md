
### I. Separation of concerns

Код должен разделять на основные слои.
- Transport Layer (handlers, middlewares, routers and etc.)
- Service Layer (usecases, mappers, DTO and etc.)
- Storage Layer (repositories, DAO and etc.)

### II. Dependency Inversion Principle (DIP)

Зависимости внедряются в слои, а не те вызывают их. 

Слабая связанность полезна для удобного покрытия тестами.

### III. Adaptability to Change

Код организуется модульно и гибко, что позволяет легко масштабировать и поддерживать его.
Должна быть простота в изменении/дополнении.

### IV. Focus On Business Value

Концентрация на бизнес-требованиях, которые важны для конечного пользователя. 
Новый код и архитектура никак не должны урезать функционал необходимый для юзеров. 

