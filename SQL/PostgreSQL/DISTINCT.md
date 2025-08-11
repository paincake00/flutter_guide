
Удаляет дубликаты из результата запроса на уровне SELECT, а не на уровне таблицы.

```sql
SELECT DISTINCT status
FROM ordres;
```

- Вернёт **уникальные значения** колонки `status`.
- Если в таблице есть 5 строк `'paid'`, `'paid'`, `'pending'`, `'pending'`, `'shipped'` — результат:
```sql
paid
pending
shipped
```

Уникальность по комбинации колонок:
```sql
SELECT DISTINCT status, customer_id
FROM orders;
```

### DISTINCT ON

Оставляем только первую строку для уникального значения status. Для этого сортируем через ORDER BY сначала по status. Внутри каждой группы status сортируем уже по дате создания (`created_at`). DESC - по убыванию, то есть сначала самые новые заказы.

```sql
SELECT DISTINCT ON (status) status, id, created_at
FROM orders
ORDER BY status, created_at DESC;
```

### Performance

- `DISTINCT` требует сортировки или хэширования, чтобы удалить дубликаты.

- При больших таблицах может быть медленно — тогда лучше использовать `GROUP BY` или индексы.