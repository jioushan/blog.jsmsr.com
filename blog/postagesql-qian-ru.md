# Postagesql 浅入

## `创建table`

### 創建datebase test

```
CREATE DATEBASE <test>;
```

### 刪除database test

```
DROP DATEBASE <test>;
```

### 創建table test;

```
CREATE TABLE <table_name>;
```

### 刪除table preson;

```
DROP TABLE preson;
```

### 創建表名/表內容;

```
CREATE TABLE person1 (
id BIGSERIAL not null primary key,
first_name VARCHAR(50) not null,
last_name VARCHAR(50) not null,
gender VARCHAR(50) not null,
date_of_birth date not null,
email VARCHAR(150)
);
​
```

#### 創建表內容;

```
INSERT INTO person  (
id,
first_name,
last_name,
gender,
date_of_birth
)
VALUES (
'2',
'Anne',
'smith',
'FEMALE',
date '1988-01-01'
);
select * from person;
```

### 查看表

```
-- 查看当前序列名称
SELECT pg_get_serial_sequence('person1', 'id');
​
-- 假设序列名为 person1_id_seq，重置序列
ALTER SEQUENCE person1_id_seq RESTART WITH 1;
```

```
select * from person;
```

#### `查看表内容`

```
select first_name from person;
```

`查看`person（table）中first\_name(column)内容

```
select first_name, last_name from person;
```

***

### 用法

#### 升序/降序 `ASC`/`DESC`

```
-- 降序
select * from person2 order by country_of_birth DESC;  
-- 升序
select * from person2 order by country_of_birth ASC;
-- 默认
select * from person2 order by country_of_birth;
```

#### `DISTINCT` 去重

```
SELECT DISTINCT country_of_birth FROM person2 ORDER BY country_of_birth;
--DISTINCT去重 country_of_birth order by（排序）默认升序
```

```
select distinct country_of_birth from person2 order by country_of_birth DESC;
--DISTINCT去重 country_of_birth order by（排序）降序
```

***

#### `Where` 查找

`Where Clause and AND`

```
select * from person2 where gender = 'Female';
```

通过where gender 'female'查找表中女性. `where and运用`

```
*select * from person2 where gender = 'Female' and country_of_birth = 'China';
```

查找女性,China在table person2;

`where and or`

```
select * from person2 where gender = 'Female' and (country_of_birth = 'China' or country_of_birth = 'Panama');
```

查找女性,China和Panama在table person2;

`where and or and` 和 /或/和

```
select * from person2 where gender = 'Female' and (country_of_birth = 'China' or country_of_birth = 'Panama') and last_name ='Tritton';
```

查找女性,China和Panama在table person2的Tritton;

***

#### `Comparison Operators`

```
SELECT 1 < 2; T
```

```
1=1 T?F T
```

```
1>2 T?F F
```

```
1<2 T?F T
```

```
1<>1 T?F F
```

```
1<>2 T?F T
```

`<>不等于`

***

#### `limit` & `offset` & `IN`

`LIMIT` 至少だけ　のみ

```
SELECT * from person2 limit 10;
```

`OFFSET` 第几行开始/跳过前几行

```
SELECT * from person2 offset 5 limit 10;
```

```
SELECT * from person2 offset 5 fetch FIRST 5 row only;
```

offset 5 跳过前5行,`fetch FIRST 5 row only`; 只返回接下来五行结果.

`IN`

```
SELECT * from person2 where country_of_birth = 'China' or country_of_birth = 'France' or country_of_birth = 'Brazil';
```

使用 `WHERE` `OR`、如果使用`IN`

```
select *
from person2 
where country_of_birth in ('China', 'Brazil', 'France', 'Mexico', 'Portugal');
```

使用`IN`也可以完成这样的`OR查找`

```
select *
from person2 
where country_of_birth in ('China', 'Brazil', 'France', 'Mexico', 'Portugal')
order by country_of_birth;
```

#### `Between` & `Like`

`Between` 区间

```
select *
from person2 
where date_of_birth 
between date '2000-01-01' and '2015-01-01'
order by date_of_birth;
```

`order by` x; 按照什么 排序;

`Like and iLike`

```
select *
from person2 
where email like '%.com';
```

查看email中com的邮箱

```
select *
from person2 
where email like '%a.%';
```

携带a的邮箱

```
select *
from person2 
where email like '______@%';
```

`______@%`@前面6个字符

#### `Group`

`group`

```
select country_of_birth
from person2 group by country_of_birth;
```

```
select distinct country_of_birth from person2;
```

两者都可以列出table中country\_of\_birth;

```
select country_of_birth, count(*)
from person2 group by country_of_birth;
```

#### `count(*)` 计数

#### `group by having`

```
select country_of_birth, count(*)
from person2 group by country_of_birth
having count(*) > 5 order by country_of_birth;
```

`having count(*) > 5`

最终显示 count(\*) > 5 的国家

```
having count(*) >= 5
```

***

### Adding New Table And Data Using Mockaroo

[https://www.mockaroo.com](https://www.mockaroo.com/) 使用这个网站我们可以创建一些table来便于我们体会到这些用法.

#### `Input table` 导入表

```
\i /path/car.sql
```

导入`car.sql` `path`为你的路径

#### `MAX` & `MIN` & AVG & `SUM` & `ROUND()`

`Max` 最大 用法

```
select max(price) from car;
```

`Min` 最小 用法同理

```
select min(price) from car;
```

`avg` 平均值

```
select avg(price) from car;
```

`sum` 求和

```
select sum(price) from car;
```

`round()`四舍五入 整数

```
select round(sum(price)) from car;
```

查看每一组品牌+型号 最低价格

```
SELECT make, model, MIN(price)
FROM car
GROUP BY make, model;
```

#### `运算`

price 乘以 0.10（也就是 10%）,返回直

```
SELECT id, make, model, price * .10
FROM car;
```

查询所有汽车的信息，并计算每辆车“价格的 10%，保留两位小数”，显示值

```
select id, make ,model, price, round(price * .10, 2) from car;
```

**原价减 10% 后的价格（打 9 折），并四舍五入到整数。**

例子：

price = 20000：

10% 是 2000

price - 10% = 18000

`ROUND(18000)` = **18000**

| 列                             | 含义        |
| ----------------------------- | --------- |
| id                            | ID        |
| make                          | 品牌        |
| model                         | 型号        |
| price                         | 原价        |
| round(price \* 0.10, 2)       | 原价的 10%   |
| round(price - price\*0.10, 2) | 打 9 折后的价格 |

```
select id, make ,model, price, round(price * .10, 2),
ROUND(price - (price * .10)) from car;
```

```
SELECT id,
      make,
      model,
      price,
      ROUND(price * 0.10, 2) AS discount_10_percent,
      ROUND(price - (price * 0.10), 2) AS price_after_discount
FROM car;
​
```

`Alias`

```
select id, make ,model, price as original_price,
round(price * .10, 2) as ten_percent,
round(price - (price * .10), 2) as discount_after_10_percent
from car;
​
```

`coalesce`

```
select coalesce(email, 'Email not provided') from person2;
```

`NULL IF`

#### `NOW()`

sql时间

```
2025-12-01 17:07:25.883 +0900
```

`NOW()DATE;`

```
2025-12-01
```

`NOW()::time;`

```
17:08:19
```

Now()减去一年时间

```
select NOW() - interval '1 year';
```

```
2024-12-01 17:10:55.796 +0900
```

```
select NOW() - interval '10 year';
```

```
select NOW() - interval '1 months';
```

```
select NOW() + interval '10 day';
```

```
select NOW()::date + interval '10 day';
```

```
2025-12-11 00:00:00.000
```

\
\
\
<br>
