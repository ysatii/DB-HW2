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
1. СУБД MYSQL установлена, установлен command line Client 
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1.jpg)  
 
2. Создадим учётную запись sys_temp.
```
CREATE USER 'sys_test'@'localhost' IDENTIFIED BY 'password';
```
3. Выполним запрос на получение списка пользователей в базе данных.
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_1.jpg)  
 
 
4. `Дадим все права пользователю sys_temp.`   
```
show grants for 'sys_test'@'localhost';
```

5. `Выполним запрос на получение списка прав для пользователя sys_temp`  
```
show grants for 'sys_test'@'localhost';  
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_2.jpg)  


6. `Переподключимся к базе данных от имени sys_temp.`  
```
mysql -u sys_test -p
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_3.jpg)  
 
7. `Восстановим дамп в базу данных`
```
C:\Program Files\MySQL\MySQL Server 8.4\bin>mysql -u sys_test -p sakila < C:\Users\admin\Downloads\sakila-db\sakila-db\sakila-schema.sql
Enter password: ********

C:\Program Files\MySQL\MySQL Server 8.4\bin>mysql -u sys_test -p sakila < C:\Users\admin\Downloads\sakila-db\sakila-db\sakila-data.sql
Enter password: ********
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_4.jpg)  
просмотрим список баз данных, переключимся на sakila и   
проверим список таблиц   


```
SHOW DATABASES
USE sakila;
SHOW TABLES;
```

 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_5.jpg)   
  
 посмотрим наличие базы в програме DBeaver   
  ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_6.jpg)  
  
 Диаграмма базы данных  
  ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image1_7.jpg)   

 
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
 
ответ  
```
actor        |actor_id                 
address      |address_id               
category     |category_id              
city         |city_id                  
country      |country_id               
customer     |customer_id              
film         |film_id                  
film_actor   |actor_id,film_id         
film_category|film_id,category_id      
film_text    |film_id                  
inventory    |inventory_id             
language     |language_id              
payment      |payment_id               
rental       |rental_id                
staff        |staff_id                 
store        |store_id                 
```
Таблица film_actor имеет два ключа actor_id, film_id  
Таблица film_category имеет два ключа film_id, category_id  


## Задание 3*
1. `Уберите у пользователя sys_temp права на внесение, изменение и удаление данных из базы sakila.`  
2. `Выполните запрос на получение списка прав для пользователя sys_temp. (скриншот)`

### Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.Результатом работы должны быть скриншоты обозначенных заданий, а также простыня со всеми запросами.


## Решение 3
```

REVOKE ALL PRIVILEGES, GRANT OPTION FROM 'sys_test'@'localhost';
show grants for 'sys_test'@'localhost';

GRANT INSERT, UPDATE, DELETE ON `sakila`.* TO 'sys_test'@'localhost';
show grants for 'sys_test'@'localhost';

REVOKE INSERT, UPDATE, DELETE ON `sakila`.* FROM 'sys_test'@'localhost';
show grants for 'sys_test'@'localhost';
```
 ![alt text](https://github.com/ysatii/DB-HW2/blob/main/img/image3.jpg)  
