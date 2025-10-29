# SQL-130

Задание 1
```
SELECT model, speed, hd
FROM PC
WHERE price < 500
```

Задание 2
```
SELECT DISTINCT maker
FROM Product
WHERE type = 'Printer'
```

Задание 3
```
SELECT model, ram, screen
FROM Laptop
WHERE price > 1000;
```

Задание 4
```
SELECT *
FROM Printer
WHERE color = 'y';
```

Задание 5
```
SELECT model, speed, hd
FROM PC
WHERE cd IN ('12x', '24x') AND price < 600;
```

Задание 6
```
SELECT DISTINCT p.maker, l.speed
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.type = 'Laptop' AND l.hd >= 10;
```

Задание 7
```
SELECT p.model, pr.price
FROM Product p
JOIN PC pr ON p.model = pr.model
WHERE p.maker = 'B'
UNION
SELECT p.model, l.price
FROM Product p
JOIN Laptop l ON p.model = l.model
WHERE p.maker = 'B'
UNION
SELECT p.model, pr.price
FROM Product p
JOIN Printer pr ON p.model = pr.model
WHERE p.maker = 'B';
```

Задание 8
```
SELECT DISTINCT p.maker
FROM Product p
WHERE p.type = 'PC'
AND p.maker NOT IN (
    SELECT DISTINCT p2.maker
    FROM Product p2
    WHERE p2.type = 'Laptop'
);
```

Задание 9
```
SELECT DISTINCT p.maker
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.type = 'PC' AND pc.speed >= 450;
```

Задание 10
```
SELECT model, price
FROM Printer
WHERE price = (SELECT MAX(price) FROM Printer);
```

Задание 11
```
SELECT AVG(speed) AS avg_speed
FROM PC;
```

Задание 12
```
SELECT AVG(speed) AS avg_speed
FROM Laptop
WHERE price > 1000;
```

Задание 13
```
SELECT AVG(pc.speed) AS avg_speed
FROM Product p
JOIN PC pc ON p.model = pc.model
WHERE p.maker = 'A';
```

Задание 14
```
SELECT s.class, s.name, c.country
FROM Ships s
JOIN Classes c ON s.class = c.class
WHERE c.numGuns >= 10;
```

Задание 15
```
SELECT hd
FROM PC
GROUP BY hd
HAVING COUNT(hd) >= 2;
```

Задание 16
```

```

Задание 17
```

```

Задание 18
```

```

Задание 19
```

```

Задание 20
```

```

Задание 21
```

```

Задание 22
```

```

Задание 23
```

```

Задание 24
```

```

Задание 25
```

```

Задание 26
```

```

Задание 27
```

```

Задание 28
```

```

Задание 29
```

```

Задание 30
```

```

Задание 31
```

```

Задание 32
```

```

Задание 33
```

```
Задание 34
```

```

Задание 35
```

```

Задание 36
```

```

Задание 37
```

```

Задание 38
```

```

Задание 39
```

```

Задание 40
```

```

Задание 41
```

```

Задание 42
```

```

Задание 43
```

```

Задание 44
```

```

Задание 45
```

```

Задание 46
```

```

Задание 47
```

```

Задание 48
```

```

Задание 49
```

```

Задание 50
```

```

Задание 51
```

```
Задание 52
```

```

Задание 53
```

```

Задание 54
```

```

Задание 55
```

```

Задание 56
```

```

Задание 57
```

```

Задание 58
```

```

Задание 59
```

```

Задание 60
```

```

Задание 61
```

```

Задание 62
```

```

Задание 63
```

```

Задание 64
```

```

Задание 65
```

```

Задание 66
```

```

Задание 67
```

```

Задание 68
```

```

Задание 69
```

```
Задание 70
```

```

Задание 71
```
SELECT DISTINCT maker
FROM Product
WHERE type = 'PC'
AND maker NOT IN (
    SELECT maker
    FROM Product
    WHERE type = 'PC'
    AND model NOT IN (SELECT model FROM PC)
);
```

Задание 72
```
SELECT p.name, COUNT(*) as trip_Qty
FROM Passenger p
JOIN Pass_in_trip pit ON p.ID_psg = pit.ID_psg
JOIN Trip t ON pit.trip_no = t.trip_no
GROUP BY p.ID_psg, p.name
HAVING COUNT(DISTINCT t.ID_comp) = 1
AND COUNT(*) = (
    SELECT MAX(flight_count)
    FROM (
        SELECT COUNT(*) as flight_count
        FROM Passenger p2
        JOIN Pass_in_trip pit2 ON p2.ID_psg = pit2.ID_psg
        JOIN Trip t2 ON pit2.trip_no = t2.trip_no
        GROUP BY p2.ID_psg
        HAVING COUNT(DISTINCT t2.ID_comp) = 1
    ) as single_company_passengers
)
ORDER BY trip_Qty DESC, p.name;
```

Задание 73
```
WITH AllCountries AS (
    SELECT DISTINCT country FROM Classes
),
CountryBattles AS (
    SELECT DISTINCT 
        COALESCE(c.country, c2.country) as country,
        o.battle
    FROM Outcomes o
    LEFT JOIN Ships s ON o.ship = s.name
    LEFT JOIN Classes c ON s.class = c.class
    LEFT JOIN Classes c2 ON o.ship = c2.class
    WHERE COALESCE(c.country, c2.country) IS NOT NULL
)
SELECT ac.country, b.name
FROM AllCountries ac
CROSS JOIN Battles b
WHERE NOT EXISTS (
    SELECT 1 
    FROM CountryBattles cb 
    WHERE cb.country = ac.country AND cb.battle = b.name
)
ORDER BY ac.country, b.name
```

Задание 74
```
SELECT country, class
FROM Classes
WHERE country = 'Russia'
UNION ALL
SELECT country, class
FROM Classes
WHERE NOT EXISTS (SELECT * FROM Classes WHERE country = 'Russia')
```

Задание 75
```
WITH MakerPrices AS (
    SELECT 
        p.maker,
        CASE WHEN p.type = 'Laptop' THEN l.price END AS laptop_price,
        CASE WHEN p.type = 'PC' THEN pc.price END AS pc_price, 
        CASE WHEN p.type = 'Printer' THEN pr.price END AS printer_price
    FROM Product p
    LEFT JOIN Laptop l ON p.model = l.model
    LEFT JOIN PC pc ON p.model = pc.model
    LEFT JOIN Printer pr ON p.model = pr.model
    WHERE l.price IS NOT NULL OR pc.price IS NOT NULL OR pr.price IS NOT NULL
)
SELECT 
    maker,
    MAX(laptop_price) AS laptop,
    MAX(pc_price) AS pc,
    MAX(printer_price) AS printer
FROM MakerPrices
GROUP BY maker
ORDER BY maker
```

Задание 76
```
SELECT p.name, 
       SUM(DATEDIFF(minute, time_out, 
           CASE WHEN time_out > time_in THEN DATEADD(day, 1, time_in) 
                ELSE time_in END)) AS minutes
FROM Passenger p
JOIN Pass_in_trip pit ON p.ID_psg = pit.ID_psg
JOIN Trip t ON pit.trip_no = t.trip_no
WHERE p.ID_psg IN (
    SELECT ID_psg
    FROM Pass_in_trip
    GROUP BY ID_psg
    HAVING COUNT(DISTINCT place) = COUNT(*)
)
GROUP BY p.ID_psg, p.name
ORDER BY minutes DESC
```

Задание 77
```
WITH RostovFlights AS (
    SELECT pit.date, COUNT(DISTINCT pit.trip_no) AS flight_count
    FROM Pass_in_trip pit
    JOIN Trip t ON pit.trip_no = t.trip_no
    WHERE t.town_from = 'Rostov'
    GROUP BY pit.date
)
SELECT flight_count, date
FROM RostovFlights
WHERE flight_count = (SELECT MAX(flight_count) FROM RostovFlights)
```

Задание 78
```
SELECT 
    name,
    CONVERT(VARCHAR(10), DATEFROMPARTS(YEAR(date), MONTH(date), 1), 120) AS first_day,
    CONVERT(VARCHAR(10), EOMONTH(date), 120) AS last_day
FROM Battles
ORDER BY name
```

Задание 79
```
WITH FlightTimes AS (
    SELECT 
        p.ID_psg,
        p.name,
        SUM(DATEDIFF(minute, t.time_out, 
            CASE WHEN t.time_out > t.time_in THEN DATEADD(day, 1, t.time_in) 
                 ELSE t.time_in END)) AS total_minutes
    FROM Passenger p
    JOIN Pass_in_trip pit ON p.ID_psg = pit.ID_psg
    JOIN Trip t ON pit.trip_no = t.trip_no
    GROUP BY p.ID_psg, p.name
)
SELECT name, total_minutes
FROM FlightTimes
WHERE total_minutes = (SELECT MAX(total_minutes) FROM FlightTimes)
```

Задание 80
```
SELECT DISTINCT maker
FROM Product
WHERE maker NOT IN (
    SELECT maker
    FROM Product
    WHERE type = 'PC'
    AND model NOT IN (SELECT model FROM PC)
)
AND maker IN (
    SELECT maker FROM Product WHERE type IN ('PC', 'Laptop', 'Printer')
)
```

Задание 81
```
WITH MonthlyOutcome AS (
    SELECT 
        YEAR(date) AS year,
        MONTH(date) AS month,
        SUM(out) AS total_out
    FROM Outcome
    GROUP BY YEAR(date), MONTH(date)
),
MaxMonthlyOutcome AS (
    SELECT MAX(total_out) AS max_total
    FROM MonthlyOutcome
)
SELECT o.*
FROM Outcome o
JOIN MonthlyOutcome mo ON YEAR(o.date) = mo.year AND MONTH(o.date) = mo.month
WHERE mo.total_out = (SELECT max_total FROM MaxMonthlyOutcome)
```

Задание 82
```
WITH NumberedPC AS (
    SELECT code, price,
           ROW_NUMBER() OVER (ORDER BY code) AS rn
    FROM PC
)
SELECT 
    n1.code AS first_code,
    AVG(n2.price) AS avg_price
FROM NumberedPC n1
JOIN NumberedPC n2 ON n2.rn BETWEEN n1.rn AND n1.rn + 5
GROUP BY n1.code, n1.rn
HAVING COUNT(*) = 6
ORDER BY n1.rn
```

Задание 83
```
SELECT name
FROM Ships s
JOIN Classes c ON s.class = c.class
WHERE 
    CASE WHEN c.numGuns = 8 THEN 1 ELSE 0 END +
    CASE WHEN c.bore = 15 THEN 1 ELSE 0 END +
    CASE WHEN c.displacement = 32000 THEN 1 ELSE 0 END +
    CASE WHEN c.type = 'bb' THEN 1 ELSE 0 END +
    CASE WHEN s.launched = 1915 THEN 1 ELSE 0 END +
    CASE WHEN s.class = 'Kongo' THEN 1 ELSE 0 END +
    CASE WHEN c.country = 'USA' THEN 1 ELSE 0 END >= 4
```

Задание 84
```
SELECT 
    c.name AS company,
    SUM(CASE WHEN DAY(pit.date) BETWEEN 1 AND 10 THEN 1 ELSE 0 END) AS decade1,
    SUM(CASE WHEN DAY(pit.date) BETWEEN 11 AND 20 THEN 1 ELSE 0 END) AS decade2,
    SUM(CASE WHEN DAY(pit.date) BETWEEN 21 AND 30 THEN 1 ELSE 0 END) AS decade3
FROM Company c
JOIN Trip t ON c.ID_comp = t.ID_comp
JOIN Pass_in_trip pit ON t.trip_no = pit.trip_no
WHERE YEAR(pit.date) = 2003 AND MONTH(pit.date) = 4
GROUP BY c.ID_comp, c.name
ORDER BY c.name
```

Задание 85
```

```

Задание 86
```

```

Задание 87
```

```
