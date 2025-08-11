
В PostgreSQL enum реализуется через создание собственного типа.

Создание enum, через создание нового типа `order_status`:
```sql
CREATE TYPE order_status AS ENUM ('pending', 'paid', 'shipped', 'canceled');
```

Создание таблицы:
```sql
CREATE TABLE orders (
	id SERIAL PRIMARY KEY,
	status order_status NOT NULL DEFAULT 'pending'
);
```

Вставка данных:
```sql
INSERT INTO orders (status) VALUES ('paid');
INSERT INTO orders DEFAULT VALUES; -- вставит 'pending'
```

Обновление:
```sql
UPDATE orders
SET status = 'shipped'
WHERE id = 1;
```

Выборка:
```sql
SELECT * FROM orders WHERE status = 'paid';
```

Добавление нового значения (не через ALTER TABLE, а через изменение типа - ALTER TYPE):
```sql
ALTER TYPE order_status ADD VALUE 'returned';
```