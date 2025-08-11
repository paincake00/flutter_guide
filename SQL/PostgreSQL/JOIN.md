
`JOIN` — это способ объединить строки из двух (или более) таблиц по определённому условию.

```sql
SELECT o.id, c.name
FROM orders o
JOIN customers c ON o.customer_id = c.id;
```

### INNER JOIN

Возвращает только те строки, где есть совпадение в обеих таблицах.
```sql
SELECT o.id, c.name
FROM orders o
INNER JOIN customers c ON o.customer_id = c.id;
```

### LEFT JOIN

Берёт все строки из **левой** таблицы и добавляет данные из правой, если совпадение есть. 
Если совпадения нет → справа будут `NULL`.
```sql
SELECT u.unique_id, e.name
FROM Employees e
LEFT JOIN EmployeeUNI u ON e.id = u.id;
```

Левая таблица - это Employees, правая - это EmployeeUNI.

Employees:
```
| id | name     | 
| -- | -------- | 
| 1  | Alice    | 
| 7  | Bob      |
| 11 | Meir     |
| 90 | Winston  |
| 3  | Jonathan |
```

EmployeeUNI:
```
| id | unique_id |
| -- | --------- |
| 3  | 1         |
| 11 | 2         |
| 90 | 3         |
```

Вывод:
```
| unique_id | name     |
| --------- | -------- |
| null      | Alice    | 
| null      | Bob      | 
| 2         | Meir     | 
| 3         | Winston  | 
| 1         | Jonathan |
```

C `RIGHT JOIN` аналогично, но надо учитывать что все значения будут браться из `EmployeeUNI`.

### FULL JOIN

Берёт все строки из обеих таблиц, заполняя `NULL`, если пары нет.
```sql
SELECT o.id, c.name
FROM orders o
FULL JOIN customers c ON o.customer_id = c.id;
```

### CROSS JOIN 

Декартово произведение

```sql
SELECT st.student_id, st.student_name, sb.subject_name
FROM Students st
CROSS JOIN Subjects sb
ORDER BY st.student_id, sb.subject_name;
```

Students table:
```
+------------+--------------+
| student_id | student_name |
+------------+--------------+
| 1          | Alice        |
| 2          | Bob          |
| 13         | John         |
| 6          | Alex         |
+------------+--------------+
```

Subjects table:
```
+--------------+
| subject_name |
+--------------+
| Math         |
| Physics      |
| Programming  |
+--------------+
```

Output:
```
| student_id | student_name | subject_name |
| ---------- | ------------ | ------------ |
| 1          | Alice        | Math         |
| 1          | Alice        | Physics      |
| 1          | Alice        | Programming  |
| 2          | Bob          | Math         |
| 2          | Bob          | Physics      |
| 2          | Bob          | Programming  |
| 6          | Alex         | Math         |
| 6          | Alex         | Physics      |
| 6          | Alex         | Programming  |
| 13         | John         | Math         |
| 13         | John         | Physics      |
| 13         | John         | Programming  |
```