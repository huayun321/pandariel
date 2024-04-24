---
weight: 1
bookFlatSection: true
title: "SQL in 10 Minutes"
---

# 《SQL 必知必会》

![](../sql10.png)

## 检索数据

### 检索单个列
```sql
mysql> select prod_name from products;
+---------------------+
| prod_name           |
+---------------------+
| Fish bean bag toy   |
| Bird bean bag toy   |
| Rabbit bean bag toy |
| 8 inch teddy bear   |
| 12 inch teddy bear  |
| 18 inch teddy bear  |
| Raggedy Ann         |
| King doll           |
| Queen doll          |
+---------------------+
9 rows in set (0.00 sec)
```

### 检索多个列
```sql
mysql> select prod_id, prod_name, prod_price
    -> from products;
+---------+---------------------+------------+
| prod_id | prod_name           | prod_price |
+---------+---------------------+------------+
| BNBG01  | Fish bean bag toy   |       3.49 |
| BNBG02  | Bird bean bag toy   |       3.49 |
| BNBG03  | Rabbit bean bag toy |       3.49 |
| BR01    | 8 inch teddy bear   |       5.99 |
| BR02    | 12 inch teddy bear  |       8.99 |
| BR03    | 18 inch teddy bear  |      11.99 |
| RGAN01  | Raggedy Ann         |       4.99 |
| RYL01   | King doll           |       9.49 |
| RYL02   | Queen doll          |       9.49 |
+---------+---------------------+------------+
9 rows in set (0.00 sec)

```

### 检索所有列
```sql
mysql> select * from products;
+---------+---------+---------------------+------------+-----------------------------------------------------------------------+
| prod_id | vend_id | prod_name           | prod_price | prod_desc                                                             |
+---------+---------+---------------------+------------+-----------------------------------------------------------------------+
| BNBG01  | DLL01   | Fish bean bag toy   |       3.49 | Fish bean bag toy, complete with bean bag worms with which to feed it |
| BNBG02  | DLL01   | Bird bean bag toy   |       3.49 | Bird bean bag toy, eggs are not included                              |
| BNBG03  | DLL01   | Rabbit bean bag toy |       3.49 | Rabbit bean bag toy, comes with bean bag carrots                      |
| BR01    | BRS01   | 8 inch teddy bear   |       5.99 | 8 inch teddy bear, comes with cap and jacket                          |
| BR02    | BRS01   | 12 inch teddy bear  |       8.99 | 12 inch teddy bear, comes with cap and jacket                         |
| BR03    | BRS01   | 18 inch teddy bear  |      11.99 | 18 inch teddy bear, comes with cap and jacket                         |
| RGAN01  | DLL01   | Raggedy Ann         |       4.99 | 18 inch Raggedy Ann doll                                              |
| RYL01   | FNG01   | King doll           |       9.49 | 12 inch king doll with royal garments and crown                       |
| RYL02   | FNG01   | Queen doll          |       9.49 | 12 inch queen doll with royal garments and crown                      |
+---------+---------+---------------------+------------+-----------------------------------------------------------------------+
9 rows in set (0.00 sec)
```

### 检索不同的值
```sql
mysql> select vend_id from products;
+---------+
| vend_id |
+---------+
| BRS01   |
| BRS01   |
| BRS01   |
| DLL01   |
| DLL01   |
| DLL01   |
| DLL01   |
| FNG01   |
| FNG01   |
+---------+
9 rows in set (0.00 sec)

mysql> select distinct vend_id from products;
+---------+
| vend_id |
+---------+
| BRS01   |
| DLL01   |
| FNG01   |
+---------+
3 rows in set (0.00 sec)
```

### 限制结果
```sql
mysql> select prod_name from products limit 5;
+---------------------+
| prod_name           |
+---------------------+
| Fish bean bag toy   |
| Bird bean bag toy   |
| Rabbit bean bag toy |
| 8 inch teddy bear   |
| 12 inch teddy bear  |
+---------------------+
5 rows in set (0.00 sec)

mysql> select prod_name from products limit 5 offset 5;
+--------------------+
| prod_name          |
+--------------------+
| 18 inch teddy bear |
| Raggedy Ann        |
| King doll          |
| Queen doll         |
+--------------------+
4 rows in set (0.01 sec)
```

### 使用注释
```sql
select prod_name     -- 这是一条注释
from products;


# 这是一条注释
select prod_name     
from products;


/* select prod_name, vend_id
   from products;
*/
select prod_name
from products;
```

## 排序检索数据

### 排序数据
```sql
mysql> select prod_name from products;
+---------------------+
| prod_name           |
+---------------------+
| Fish bean bag toy   |
| Bird bean bag toy   |
| Rabbit bean bag toy |
| 8 inch teddy bear   |
| 12 inch teddy bear  |
| 18 inch teddy bear  |
| Raggedy Ann         |
| King doll           |
| Queen doll          |
+---------------------+
9 rows in set (0.00 sec)

mysql> select prod_name
    -> from products
    -> order by prod_name;
+---------------------+
| prod_name           |
+---------------------+
| 12 inch teddy bear  |
| 18 inch teddy bear  |
| 8 inch teddy bear   |
| Bird bean bag toy   |
| Fish bean bag toy   |
| King doll           |
| Queen doll          |
| Rabbit bean bag toy |
| Raggedy Ann         |
+---------------------+
9 rows in set (0.00 sec)
```

### 按多个列排序
```sql
mysql> select prod_id, prod_price, prod_name
    -> from products
    -> order by prod_price, prod_name;
+---------+------------+---------------------+
| prod_id | prod_price | prod_name           |
+---------+------------+---------------------+
| BNBG02  |       3.49 | Bird bean bag toy   |
| BNBG01  |       3.49 | Fish bean bag toy   |
| BNBG03  |       3.49 | Rabbit bean bag toy |
| RGAN01  |       4.99 | Raggedy Ann         |
| BR01    |       5.99 | 8 inch teddy bear   |
| BR02    |       8.99 | 12 inch teddy bear  |
| RYL01   |       9.49 | King doll           |
| RYL02   |       9.49 | Queen doll          |
| BR03    |      11.99 | 18 inch teddy bear  |
+---------+------------+---------------------+
9 rows in set (0.00 sec)
```

### 按列位置排序
```sql
mysql> select prod_id, prod_price, prod_name
    -> from products
    -> order by 2, 3;
+---------+------------+---------------------+
| prod_id | prod_price | prod_name           |
+---------+------------+---------------------+
| BNBG02  |       3.49 | Bird bean bag toy   |
| BNBG01  |       3.49 | Fish bean bag toy   |
| BNBG03  |       3.49 | Rabbit bean bag toy |
| RGAN01  |       4.99 | Raggedy Ann         |
| BR01    |       5.99 | 8 inch teddy bear   |
| BR02    |       8.99 | 12 inch teddy bear  |
| RYL01   |       9.49 | King doll           |
| RYL02   |       9.49 | Queen doll          |
| BR03    |      11.99 | 18 inch teddy bear  |
+---------+------------+---------------------+
9 rows in set (0.00 sec)
```

### 指定排序方向

```sql
mysql> select prod_id, prod_price, prod_name
    -> from products
    -> order by prod_price desc;
+---------+------------+---------------------+
| prod_id | prod_price | prod_name           |
+---------+------------+---------------------+
| BR03    |      11.99 | 18 inch teddy bear  |
| RYL01   |       9.49 | King doll           |
| RYL02   |       9.49 | Queen doll          |
| BR02    |       8.99 | 12 inch teddy bear  |
| BR01    |       5.99 | 8 inch teddy bear   |
| RGAN01  |       4.99 | Raggedy Ann         |
| BNBG01  |       3.49 | Fish bean bag toy   |
| BNBG02  |       3.49 | Bird bean bag toy   |
| BNBG03  |       3.49 | Rabbit bean bag toy |
+---------+------------+---------------------+
9 rows in set (0.00 sec)

mysql> select cust_id, order_num
    -> from orders
    -> order by cust_id asc,   order_date desc;
+------------+-----------+
| cust_id    | order_num |
+------------+-----------+
| 1000000001 |     20005 |
| 1000000001 |     20009 |
| 1000000003 |     20006 |
| 1000000004 |     20007 |
| 1000000005 |     20008 |
+------------+-----------+
5 rows in set (0.01 sec)
```

## 过滤数据

### 使用where子句

```sql
mysql> select prod_name, prod_price
    -> from products
    -> where prod_price = 3.49;
+---------------------+------------+
| prod_name           | prod_price |
+---------------------+------------+
| Fish bean bag toy   |       3.49 |
| Bird bean bag toy   |       3.49 |
| Rabbit bean bag toy |       3.49 |
+---------------------+------------+
3 rows in set (0.00 sec)


```

### where 子句操作符
#### 检查单个值
```sql
mysql> select prod_name, prod_price
    -> from products
    -> where prod_price < 10;
+---------------------+------------+
| prod_name           | prod_price |
+---------------------+------------+
| Fish bean bag toy   |       3.49 |
| Bird bean bag toy   |       3.49 |
| Rabbit bean bag toy |       3.49 |
| 8 inch teddy bear   |       5.99 |
| 12 inch teddy bear  |       8.99 |
| Raggedy Ann         |       4.99 |
| King doll           |       9.49 |
| Queen doll          |       9.49 |
+---------------------+------------+
8 rows in set (0.00 sec)
```
#### 不匹配检查
```sql
mysql> select vend_id, prod_name
    -> from products
    -> where vend_id <> 'DLL01';
+---------+--------------------+
| vend_id | prod_name          |
+---------+--------------------+
| BRS01   | 8 inch teddy bear  |
| BRS01   | 12 inch teddy bear |
| BRS01   | 18 inch teddy bear |
| FNG01   | King doll          |
| FNG01   | Queen doll         |
+---------+--------------------+
5 rows in set (0.00 sec)
```

#### 范围值检查
```sql
mysql> select prod_name, prod_price
    -> from products
    -> where prod_price between 5 and 10;
+--------------------+------------+
| prod_name          | prod_price |
+--------------------+------------+
| 8 inch teddy bear  |       5.99 |
| 12 inch teddy bear |       8.99 |
| King doll          |       9.49 |
| Queen doll         |       9.49 |
+--------------------+------------+
4 rows in set (0.00 sec)
```

####空值检查
```sql
mysql> select cust_name
    -> from customers
    -> where cust_email is null;
+---------------+
| cust_name     |
+---------------+
| Kids Place    |
| The Toy Store |
+---------------+
2 rows in set (0.00 sec)
```

## 高级数据过滤
### 组合where子句
#### and 操作符
```sql
mysql> select prod_id, prod_price, prod_name
    -> from products
    -> where vend_id = 'DLL01' and prod_price <= 4;
+---------+------------+---------------------+
| prod_id | prod_price | prod_name           |
+---------+------------+---------------------+
| BNBG01  |       3.49 | Fish bean bag toy   |
| BNBG02  |       3.49 | Bird bean bag toy   |
| BNBG03  |       3.49 | Rabbit bean bag toy |
+---------+------------+---------------------+
3 rows in set (0.00 sec)
```

#### or操作符
````sql
mysql> select prod_name, prod_price
    -> from products
    -> where vend_id = 'DLL01' or vend_id = 'BRS01';
+---------------------+------------+
| prod_name           | prod_price |
+---------------------+------------+
| 8 inch teddy bear   |       5.99 |
| 12 inch teddy bear  |       8.99 |
| 18 inch teddy bear  |      11.99 |
| Fish bean bag toy   |       3.49 |
| Bird bean bag toy   |       3.49 |
| Rabbit bean bag toy |       3.49 |
| Raggedy Ann         |       4.99 |
+---------------------+------------+
7 rows in set (0.00 sec)
````

#### 求值顺序
```sql
mysql> select prod_name, prod_price
    -> from products
    -> where vend_id = 'DLL01' or vend_id = 'BRS01'
    -> and prod_price >= 10;
+---------------------+------------+
| prod_name           | prod_price |
+---------------------+------------+
| 18 inch teddy bear  |      11.99 |
| Fish bean bag toy   |       3.49 |
| Bird bean bag toy   |       3.49 |
| Rabbit bean bag toy |       3.49 |
| Raggedy Ann         |       4.99 |
+---------------------+------------+
5 rows in set (0.01 sec)

mysql> select prod_name, prod_price
    -> from products
    -> where (vend_id = 'DLL01' or vend_id = 'BRS01')
    -> and prod_price >= 10;
+--------------------+------------+
| prod_name          | prod_price |
+--------------------+------------+
| 18 inch teddy bear |      11.99 |
+--------------------+------------+
1 row in set (0.00 sec)
```

### in 操作符
```sql
mysql> select prod_name, prod_price 
    -> from products
    -> where vend_id in ('DLL011', 'BRS01')
    -> order by prod_name;
+--------------------+------------+
| prod_name          | prod_price |
+--------------------+------------+
| 12 inch teddy bear |       8.99 |
| 18 inch teddy bear |      11.99 |
| 8 inch teddy bear  |       5.99 |
+--------------------+------------+
3 rows in set (0.00 sec)
```

### not 操作符
```sql
mysql> select prod_name
    -> from products
    -> where not vend_id = 'DLL01'
    -> order by prod_name;
+--------------------+
| prod_name          |
+--------------------+
| 12 inch teddy bear |
| 18 inch teddy bear |
| 8 inch teddy bear  |
| King doll          |
| Queen doll         |
+--------------------+
5 rows in set (0.00 sec)
```

## 用通配符进行过滤
### like 操作符
#### 百分号(%)通配符
```sql
mysql> select prod_id, prod_name
    -> from products
    -> where prod_name like 'Fish%';
+---------+-------------------+
| prod_id | prod_name         |
+---------+-------------------+
| BNBG01  | Fish bean bag toy |
+---------+-------------------+
1 row in set (0.00 sec)

mysql> select prod_id, prod_name
    -> from products
    -> where prod_name like '%bean bag%';
+---------+---------------------+
| prod_id | prod_name           |
+---------+---------------------+
| BNBG01  | Fish bean bag toy   |
| BNBG02  | Bird bean bag toy   |
| BNBG03  | Rabbit bean bag toy |
+---------+---------------------+
3 rows in set (0.00 sec)

mysql> select prod_name
    -> from products
    -> where prod_name like 'F%y';
+-------------------+
| prod_name         |
+-------------------+
| Fish bean bag toy |
+-------------------+
1 row in set (0.00 sec)


```

#### 下划线通配符
```sql
mysql> select prod_id, prod_name
    -> from products
    -> where prod_name like '__ inch teddy bear';
+---------+--------------------+
| prod_id | prod_name          |
+---------+--------------------+
| BR02    | 12 inch teddy bear |
| BR03    | 18 inch teddy bear |
+---------+--------------------+
2 rows in set (0.00 sec)

mysql> select prod_id, prod_name
    -> from products
    -> where prod_name like '% inch teddy bear';
+---------+--------------------+
| prod_id | prod_name          |
+---------+--------------------+
| BR01    | 8 inch teddy bear  |
| BR02    | 12 inch teddy bear |
| BR03    | 18 inch teddy bear |
+---------+--------------------+
3 rows in set (0.01 sec)
```

### 使用通配符的技巧
* 不过度使用通配符
* 在确实需要使用通配符时，也尽量不要把它们凡在搜索模式的开始处
* 仔细注意通配符的位置


## 创建计算字段
### 计算字段
转换计算或格式转化的字段

### 拼接字段

```sql
mysql> select concat(vend_name, '(', vend_country, ')') from vendors order by vend_name;
+-------------------------------------------+
| concat(vend_name, '(', vend_country, ')') |
+-------------------------------------------+
| Bear Emporium(USA)                        |
| Bears R Us(USA)                           |
| Doll House Inc.(USA)                      |
| Fun and Games(England)                    |
| Furball Inc.(USA)                         |
| Jouets et ours(France)                    |
+-------------------------------------------+
6 rows in set (0.00 sec)

mysql> select concat(rtrim(vend_name), '(', rtrim(vend_country), ')') from vendors  order by vend_name ;
+---------------------------------------------------------+
| concat(rtrim(vend_name), '(', rtrim(vend_country), ')') |
+---------------------------------------------------------+
| Bear Emporium(USA)                                      |
| Bears R Us(USA)                                         |
| Doll House Inc.(USA)                                    |
| Fun and Games(England)                                  |
| Furball Inc.(USA)                                       |
| Jouets et ours(France)                                  |
+---------------------------------------------------------+
6 rows in set (0.01 sec)
```

#### 使用别名
```sql
mysql> select concat(rtrim(vend_name), '(', rtrim(vend_country), ')')
    -> as vend_title
    -> from vendors
    -> order by vend_name;
+------------------------+
| vend_title             |
+------------------------+
| Bear Emporium(USA)     |
| Bears R Us(USA)        |
| Doll House Inc.(USA)   |
| Fun and Games(England) |
| Furball Inc.(USA)      |
| Jouets et ours(France) |
+------------------------+
6 rows in set (0.00 sec)
```

### 执行算术计算
```sql
mysql> select prod_id, quantity, item_price
    -> from orderItems
    -> where order_num = 20008;
+---------+----------+------------+
| prod_id | quantity | item_price |
+---------+----------+------------+
| RGAN01  |        5 |       4.99 |
| BR03    |        5 |      11.99 |
| BNBG01  |       10 |       3.49 |
| BNBG02  |       10 |       3.49 |
| BNBG03  |       10 |       3.49 |
+---------+----------+------------+
5 rows in set (0.00 sec)

mysql> select prod_id,
    -> quantity,
    -> item_price,
    -> quantity*item_price as expanded_price
    -> from orderItems
    -> where order_num = 20008;
+---------+----------+------------+----------------+
| prod_id | quantity | item_price | expanded_price |
+---------+----------+------------+----------------+
| RGAN01  |        5 |       4.99 |          24.95 |
| BR03    |        5 |      11.99 |          59.95 |
| BNBG01  |       10 |       3.49 |          34.90 |
| BNBG02  |       10 |       3.49 |          34.90 |
| BNBG03  |       10 |       3.49 |          34.90 |
+---------+----------+------------+----------------+
5 rows in set (0.00 sec)

```



## 资源
* [Sams Teach Yourself SQL in 10 Minutes](https://forta.com/books/0135182794/)
* [10 Best UML Diagram Software Tools in 2024](https://clickup.com/blog/uml-diagram-software/?utm_source=google-pmax&utm_medium=cpc&utm_campaign=gpm_cpc_ar_nnc_pro_trial_all-devices_tcpa_lp_x_all-departments_x_pmax&utm_content=&utm_creative=_____&gad_source=1&gclid=CjwKCAjwz42xBhB9EiwA48pT76ag9XqDHJmZQGdsLmJn_KnKQVz8ZXNdcj_sExTpyqzzu3A5aH2NdRoCKz4QAvD_BwE)