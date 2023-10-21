### Задание 1

Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию: 
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

```
SELECT CONCAT(s2.first_name, ' ', s2.last_name) AS Name, a.address AS Address, COUNT(c.store_id) AS Customers
FROM store s 
JOIN customer c ON s.store_id = c.store_id 
JOIN staff s2 ON s.manager_staff_id = s2.staff_id 
JOIN address a ON s.address_id = a.address_id 
GROUP BY c.store_id 
HAVING COUNT(c.store_id) > 300;
```
<img width="793" alt="Снимок экрана 2023-10-21 в 19 55 06" src="https://github.com/otuzi/spqpart2/assets/61628386/71f5e11b-fa02-4943-a1c1-eb31a8a21859">


### Задание 2

Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
SELECT (SELECT  AVG(`length`) from film) AS Average, (SELECT COUNT(1) from film) AS 'All films', COUNT(1) AS 'Long Films'
FROM film 
WHERE `length` > (SELECT AVG(`length`) from film) ;
```

<img width="792" alt="Снимок экрана 2023-10-21 в 19 55 43" src="https://github.com/otuzi/spqpart2/assets/61628386/4c8a70aa-5079-44dd-af27-a3301732ff69">


### Задание 3

Получите информацию, за какой месяц была получена наибольшая сумма платежей, и добавьте информацию по количеству аренд за этот месяц.

```
SELECT MONTH(payment_date) AS Month, COUNT(payment_id) As Payments, SUM(amount) AS Amount
FROM payment
GROUP BY MONTH(payment_date) 
ORDER BY COUNT(payment_id)  DESC LIMIT 1 ;
```

<img width="763" alt="Снимок экрана 2023-10-21 в 19 56 21" src="https://github.com/otuzi/spqpart2/assets/61628386/dc25a626-7750-4d3d-adce-acaec41c3e8d">



## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 4*

Посчитайте количество продаж, выполненных каждым продавцом. Добавьте вычисляемую колонку «Премия». Если количество продаж превышает 8000, то значение в колонке будет «Да», иначе должно быть значение «Нет».

```
SELECT CONCAT(s.first_name, ' ', s.last_name) AS Name, COUNT(1) AS Sales,
	CASE
		WHEN COUNT(1) > 8000 THEN 'Yes'
		ELSE 'No'
	END AS Premium
FROM payment p 
JOIN staff s ON p.staff_id = s.staff_id 
GROUP BY p.staff_id;
```
<img width="715" alt="Снимок экрана 2023-10-21 в 19 56 57" src="https://github.com/otuzi/spqpart2/assets/61628386/91695ed8-b01a-4abc-8a17-a645573ca5f7">


### Задание 5*

Найдите фильмы, которые ни разу не брали в аренду.

```
SELECT f.title AS 'Bad product'
FROM film f
LEFT JOIN inventory i ON f.film_id = i.film_id
LEFT JOIN rental r ON i.inventory_id = r.inventory_id
WHERE r.inventory_id IS NULL;
```

<img width="383" alt="Снимок экрана 2023-10-21 в 19 57 33" src="https://github.com/otuzi/spqpart2/assets/61628386/a46426f3-27ec-434c-890e-e194b447b14d">
