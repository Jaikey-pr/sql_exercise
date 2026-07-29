

```markdown
# Обучающий этап: Задачи 1–20 (База данных "Компьютерная фирма" и "Корабли")

> **СУБД:** MS SQL Server 
> **Источник:** [sql-ex.ru](https://sql-ex.ru/learn_exercises.php?LN=21)

---

## 1

**Permalink:** [1](#1)

Найдите номер модели, скорость и размер жесткого диска для всех ПК стоимостью менее 500 дол.  
Вывести: `model`, `speed` и `hd`.

```sql
SELECT 
    p.model,
    pc.speed,
    pc.hd
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE pc.price < 500;
```

| model | speed | hd   |
| --- | --- | --- |
| 1232 | 500 | 10.0 |
| 1232 | 450 | 8.0  |
| 1232 | 450 | 10.0 |
| 1260 | 500 | 10.0 |

---

## 2

**Permalink:** [2](#2)

Найдите производителей принтеров.  
Вывести: `maker`.

```sql
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer';
```

| maker |
| --- |
| A |
| D |
| E |

---

## 3

**Permalink:** [3](#3)

Найдите номер модели, объем памяти и размеры экранов ноутбуков, цена которых превышает 1000 дол.  
Вывести: `model`, `ram`, `screen`.

```sql
SELECT 
    p.model,
    l.ram,
    l.screen
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE l.price > 1000;
```

| model | ram | screen |
| --- | --- | --- |
| 1750 | 128 | 14 |
| 1298 | 64  | 15 |
| 1752 | 128 | 14 |

---

## 4

**Permalink:** [4](#4)

Найдите все записи таблицы `Printer` для цветных принтеров.

```sql
SELECT *
FROM Printer
WHERE color = 'y';
```

| code | model | color | type | price  |
| --- | --- | --- | --- | --- |
| 3 | 1434 | y | Jet | 290.0000 |
| 2 | 1433 | y | Jet | 270.0000 |

---

## 5

**Permalink:** [5](#5)

Найдите номер модели, скорость и размер жесткого диска ПК, имеющих 12x или 24x CD и цену менее 600 дол.  
Вывести: `model`, `speed`, `hd`.

```sql
SELECT 
    p.model,
    pc.speed,
    pc.hd
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE (pc.cd = '12x' OR pc.cd = '24x') 
  AND pc.price < 600;
```

| model | speed | hd   |
| --- | --- | --- |
| 1232 | 500 | 10.0 |
| 1232 | 450 | 8.0  |
| 1232 | 450 | 10.0 |
| 1260 | 500 | 10.0 |

---

## 6

**Permalink:** [6](#6)

Для каждого производителя, выпускающего ноутбуки с объёмом жесткого диска не менее 10 Гбайт, найти скорости таких ноутбуков.  
Вывод: `maker`, `speed`.

```sql
SELECT DISTINCT 
    p.maker,
    l.speed
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE l.hd >= 10;
```

| maker | speed |
| --- | --- |
| A | 450 |
| A | 600 |
| A | 750 |
| B | 750 |

---

## 7

**Permalink:** [7](#7)

Найдите номера моделей и цены всех имеющихся в продаже продуктов (любого типа) производителя B (латинская буква).  
Вывести: `model`, `price`.

```sql
SELECT DISTINCT p.model, pc.price
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.maker = 'B'

UNION

SELECT DISTINCT p.model, l.price
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.maker = 'B'

UNION

SELECT DISTINCT p.model, pr.price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE p.maker = 'B';
```

| model | price    |
| --- | --- |
| 1121 | 850.0000 |
| 1750 | 1200.0000|

---

## 8

**Permalink:** [8](#8)

Найдите производителя, выпускающего ПК, но не ноутбуки.  
Вывести: `maker`.

```sql
SELECT DISTINCT maker
FROM Product
WHERE type = 'PC'

EXCEPT

SELECT DISTINCT maker
FROM Product
WHERE type = 'Laptop';
```

> **Примечание:** Оператор `EXCEPT` поддерживается в PostgreSQL и MS SQL Server. В MySQL следует использовать `NOT IN` или `NOT EXISTS`.

| maker |
| --- |
| E |

---

## 9

**Permalink:** [9](#9)

Найдите производителей ПК с процессором не менее 450 Мгц.  
Вывести: `maker`.

```sql
SELECT DISTINCT p.maker
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE pc.speed >= 450;
```

| maker |
| --- |
| A |
| B |
| E |

---

## 10

**Permalink:** [10](#10)

Найдите модели принтеров, имеющих самую высокую цену.  
Вывести: `model`, `price`.

```sql
SELECT DISTINCT 
    p.model,
    pr.price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE pr.price = (SELECT MAX(price) FROM Printer);
```

| model | price    |
| --- | --- |
| 1276 | 400.0000 |
| 1288 | 400.0000 |

---

## 11

**Permalink:** [11](#11)

Найдите среднюю скорость ПК.

```sql
SELECT AVG(speed) AS avg_speed
FROM PC;
```

| avg_speed |
| --- |
| 608 |

---

## 12

**Permalink:** [12](#12)

Найдите среднюю скорость ноутбуков, цена которых превышает 1000 дол.

```sql
SELECT AVG(speed) AS avg_speed
FROM Laptop
WHERE price > 1000;
```

| avg_speed |
| --- |
| 700 |

---

## 13

**Permalink:** [13](#13)

Найдите среднюю скорость ПК, выпущенных производителем A.

```sql
SELECT AVG(pc.speed) AS avg_speed
FROM PC pc
JOIN Product p ON pc.model = p.model
WHERE p.maker = 'A';
```

| avg_speed |
| --- |
| 606 |

---

## 14

**Permalink:** [14](#14)

Найдите класс, имя и страну для кораблей из таблицы `Ships`, имеющих не менее 10 орудий.  
*(База данных "Корабли")*  
Вывести: `class`, `name`, `country`.

```sql
SELECT 
    c.class,
    s.name,
    c.country
FROM Ships s
JOIN Classes c ON s.class = c.class
WHERE c.numGuns >= 10;
```

| class        | name           | country |
| --- | --- | --- |
| Tennessee    | California     | USA |
| North Carolina | North Carolina | USA |
| North Carolina | South Dakota   | USA |
| Tennessee    | Tennessee      | USA |
| North Carolina | Washington     | USA |

---

## 15

**Permalink:** [15](#15)

Найдите размеры жестких дисков, совпадающих у двух и более PC.  
Вывести: `hd`.

```sql
SELECT hd
FROM PC
GROUP BY hd
HAVING COUNT(hd) >= 2;
```

> **Примечание:** `DISTINCT` здесь не нужен, так как `GROUP BY` уже гарантирует уникальность строк в результате.

| hd   |
| --- |
| 5.0  |
| 8.0  |
| 10.0 |
| 14.0 |
| 20.0 |

---

## 16

**Permalink:** [16](#16)

Найдите пары моделей PC, имеющих одинаковые скорость и RAM. В результате каждая пара указывается только один раз, т.е. (i,j), но не (j,i).  
Порядок вывода: модель с большим номером, модель с меньшим номером, скорость и RAM.

```sql
SELECT DISTINCT 
    p1.model AS model_high,
    p2.model AS model_low,
    p1.speed,
    p1.ram
FROM PC p1
JOIN PC p2 ON p1.speed = p2.speed 
          AND p1.ram = p2.ram 
          AND p1.model > p2.model;
```

> **Примечание:** Условие `p1.model > p2.model` элегантно решает задачу исключения дубликатов пар (j,i) и самоисключений (i,i).

| model_high | model_low | speed | ram |
| --- | --- | --- | --- |
| 1233 | 1121 | 750 | 128 |
| 1233 | 1232 | 500 | 64  |
| 1260 | 1232 | 500 | 32  |

---

## 17

**Permalink:** [17](#17)

Найдите модели ноутбуков, скорость которых меньше скорости каждого из ПК.  
Вывести: `type`, `model`, `speed`.

```sql
SELECT DISTINCT 
    p.type,
    l.model,
    l.speed
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE l.speed < ALL (SELECT speed FROM PC);
```

| type   | model | speed |
| --- | --- | --- |
| Laptop | 1298 | 35 |

---

## 18

**Permalink:** [18](#18)

Найдите производителей самых дешевых цветных принтеров.  
Вывести: `maker`, `price`.

```sql
SELECT DISTINCT 
    p.maker,
    pr.price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE pr.color = 'y' 
  AND pr.price <= (SELECT MIN(price) FROM Printer WHERE color = 'y');
```

| maker | price    |
| --- | --- |
| D | 270.0000 |

---

## 19

**Permalink:** [19](#19)

Для каждого производителя, имеющего модели в таблице `Laptop`, найдите средний размер экрана выпускаемых им ноутбуков.  
Вывести: `maker`, средний размер экрана.

```sql
SELECT 
    p.maker,
    AVG(l.screen) AS avg_screen
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.type = 'Laptop'
GROUP BY p.maker;
```

| maker | avg_screen |
| --- | --- |
| A | 13 |
| B | 14 |
| C | 12 |

---

## 20

**Permalink:** [20](#20)

Найдите производителей, выпускающих по меньшей мере три различных модели ПК.  
Вывести: `maker`, число моделей ПК.

```sql
SELECT 
    p.maker,
    COUNT(p.model) AS model_count
FROM Product p
WHERE p.type = 'PC'
GROUP BY p.maker
HAVING COUNT(p.model) >= 3;
```

| maker | model_count |
| --- | --- |
| E | 3 |

```

