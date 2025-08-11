
Alias (псевдоним) - это временное имя для таблицы или колонки, которое действует только в пределах запроса.

### Для колонки

Можно изменять результат запроса для колонок, а не только указывать существующие в SELECT как обычно. Можно использовать двойные кавычки, чтобы сохранять форматирование внутри строки. 

Делается с помощью `AS`:
```sql
SELECT status AS "order status"
FROM orders;
```
или
```sql
SELECT status order_status
FROM orders;
```

Можно также добавить псевдоним к функции:
```sql
SELECT COUNT(*) AS total_orders
FROM orders;
```

### Для таблицы

```sql
SELECT o.status
FROM orders AS o
WHERE o.status = 'paid';
```
или
```sql
SELECT o.status
FROM orders o
WHERE o.status = 'paid';
```

### Alias с `JOIN`:

```sql
SELECT o.id, c.name -- вывод только id из orders и имени из customers
FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

### Alias в `DISTINCT ON` или `ORDER BY`:

```sql
SELECT DISTINCT ON (o.status) o.status AS st, o.id, o.created_at
FROM orders o
ORDER BY o.status, o.created_at DESC;
```

### Alias для подзапроса / CTE

`sub` это имя подзапроса:
```sql
SELECT *
FROM (
	SELECT status, COUNT(*) AS cnt
	FROM orders
	GROUP BY status
) sub;
```

Common Table Expressions (CTE) - обобщенные табличные выражения. Результаты табличных выражений можно временно сохранять в памяти и обращаться к ним повторно.
```sql
WITH stats AS (
	SELECT status, COUNT(*) AS cnt
	FROM orders
	GROUP BY status
)
SELECT * FROM stats;
```


