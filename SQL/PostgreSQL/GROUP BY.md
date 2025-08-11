
`GROUP BY` - это способ **сгруппировать** строки по значению одной или нескольких колонок и **выполнить** над каждой группой агрегатные функции (`COUNT`, `SUM`, `AVG`, `MAX`, `MIN` и т.д.).

### Базовый синтаксис 

```sql
SELECT column_name, aggregate_function(...)
FROM table_name
GROUP BY column_name;
```

- `column_name` - колонка (или несколько), по которой группируем
- `aggregate_function` - какую функцию применяем к каждой группе

### `GROUP BY` для колонки

```sql
SELECT status, COUNT(*) AS cnt
FROM orders
GROUP BY status;
```

Вывод:
```
|status  | cnt |
----------------
|paid    |  15 |
|pending |  7  |
|shipped |  3  |
```

### `GROUP BY` для нескольких колонок

```sql
SELECT status, customer_id, COUNT(*) AS cnt
FROM orders
GROUP BY status, customer_id;
```

Уникальность группы определяется комбинацией колонок:  
`"paid" + 123` — одна группа, `"paid" + 456` — другая.

### Фильтрация групп через `Having`

`WHERE` фильтрует **строки до группировки**,  
`HAVING` фильтрует **группы после группировки**.
```sql
SELECT customer_id, COUNT(*) AS cnt
FROM orders
GROUP BY customer_id
HAVING cnt >= 3;
```

Фильтрует чтобы были только группы, в которых `COUNT(*) >= 3`.

### Порядок выполнения

PostgreSQL обрабатывает запрос примерно так:
1. `FROM` — берёт таблицу
2. `JOIN` — объединяет таблицы по условиям `ON`
3. `WHERE` — фильтрует строки
4. `GROUP BY` — группирует строки
5. Агрегатные функции считают значения по каждой группе
6. `HAVING` — фильтрует группы
7. `SELECT` — формирует результат
8. `ORDER BY` — сортирует результат