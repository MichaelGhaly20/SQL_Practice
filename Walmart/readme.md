Assume you are given the following tables on Walmart transactions and products. 
Find the top 3 products that are most frequently bought together (purchased in the same transaction).

Output the name of product #1, name of product #2 and number of combinations in descending order.

transactions table:

| transaction_id | product_id | user_id | transaction_date    |
|----------------|------------|---------|---------------------|
| 231574         | 111        | 234     | 03/01/2022 12:00:00 |
| 231574         | 444        | 234     | 03/01/2022 12:00:00 |
| 231574         | 222        | 234     | 03/01/2022 12:00:00 |
| 137124         | 111        | 125     | 03/05/2022 12:00:00 |
| 137124         | 444        | 125     | 03/05/2022 12:00:00 |

products table:

| product_id | product_name    |
|------------|-----------------|
| 111        | apple           |
| 222        | soy milk        |
| 333        | instant oatmeal |
| 444        | banana          |
| 555        | chia seed       |

Example Output:

| product1 | product2 | combo_num |
|----------|----------|-----------|
| banana   | apple    | 2         |
| banana   | soy milk | 1         |
| soy milk | apple    | 1         |

Solution:

```
with total as (
SELECT t.transaction_id, t.user_id, p.product_name, p.product_id
FROM products p join transactions t on p.product_id = t.product_id
order by t.transaction_id, t.user_id
)
select t1.product_name as product1, t2.product_name as product2,
count(*) as combo_num
from total t1 join total t2  
on t1.transaction_id = t2.transaction_id 
and t1.product_id > t2.product_id
group by t1.product_name, t2.product_name
order by combo_num desc
limit 3
```




