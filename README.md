# Домашнее задание к занятию «Работа с данными (DDL/DML)» - Мельник Юрий Александрович


## Задание 1

### Задание можно выполнить как в любом IDE, так и в командной строке.
 

1. `Поднимите чистый инстанс MySQL версии 8.0+. Можно использовать локальный сервер или контейнер Docker.`
2. `Создайте учётную запись sys_temp.`
3. `Выполните запрос на получение списка пользователей в базе данных. (скриншот)`
4. `Дайте все права для пользователя sys_temp.`
5. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`
6. `Переподключитесь к базе данных от имени sys_temp.`
  Для смены типа аутентификации с sha2 используйте запрос:  
```
ALTER USER 'sys_test'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```
  
7. `Восстановите дамп в базу данных`
8. `При работе в IDE сформируйте ER-диаграмму получившейся базы данных. При работе в командной строке используйте команду для получения всех таблиц базы данных. (скриншот)`

### Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.

## Решение 1  
1. `Анализ задачи`  
 


## Задание 2

Составьте таблицу, используя любой текстовый редактор или Excel, в которой должно быть два столбца:  
 в первом должны быть названия таблиц восстановленной базы, во втором названия первичных ключей этих таблиц.
 Пример: (скриншот/текст)
 
```
Название таблицы | Название первичного ключа
customer         | customer_id
```

## Решение 2
В данной Базе присутсвуют таблицы и представления!  
```
SHOW TABLES;

```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image2.jpg)  
 
 
 нам нужны именно таблицы!  
```
SHOW FULL TABLES;
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image2_1.jpg)  

Таблицы имеют тип - BASE TABLE  
```
SELECT table_name from
information_schema.tables 
WHERE table_type = 'BASE TABLE' and TABLE_SCHEMA = 'sakila';
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image2_2.jpg)  
 Таблиц всего 16! остальное представления
 
 Используя служебные таблицы можем узнать колличество первичных индексов у каждой таблицы
```
select
	TABLE_NAME,
	COLUMN_NAME
from
	INFORMATION_SCHEMA.COLUMNS
where
	TABLE_SCHEMA = 'sakila'
	and COLUMN_KEY = 'PRI'
	and TABLE_NAME in 
(
	select
		table_name
	from
		information_schema.tables
	where
		table_type = 'BASE TABLE'
		and TABLE_SCHEMA = 'sakila')
order by
	table_name;
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image2_3.jpg)  
 
```
 select
	TABLE_NAME,
	GROUP_CONCAT(COLUMN_NAME)
from
	INFORMATION_SCHEMA.KEY_COLUMN_USAGE
where
	TABLE_SCHEMA = 'sakila'
	and CONSTRAINT_NAME = 'PRIMARY'
group by
	TABLE_NAME;
```

 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image2_4.jpg)  


https://github.com/ysatii/DB-HW2/blob/main/img/image1.jpg
## Задание 3*
1. `Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.`  
2. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`

### Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.
