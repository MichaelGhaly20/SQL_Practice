```
select * from stratascrach_orders;
```

| id | cust_id | order_date | order_details | total_order_cost |
|----|---------|------------|---------------|------------------|
| 1  | 3       | 2019-03-04 | Coat          | 100              |
| 2  | 3       | 2019-03-01 | Shoes         | 80               |
| 3  | 3       | 2019-03-07 | Skirt         | 30               |
| 4  | 7       | 2019-02-01 | Coat          | 25               |
| 5  | 7       | 2019-03-10 | Shoes         | 80               |
| 6  | 15      | 2019-02-01 | Boats         | 100              |
| 7  | 15      | 2019-01-11 | Shirts        | 60               |
| 8  | 15      | 2019-03-11 | Slipper       | 20               |
| 9  | 15      | 2019-03-01 | Jeans         | 80               |
| 10 | 15      | 2019-03-09 | Shirts        | 50               |
| 11 | 5       | 2019-02-01 | Shoes         | 80               |
| 12 | 12      | 2019-01-11 | Shirts        | 60               |
| 13 | 12      | 2019-03-11 | Slipper       | 20               |
| 14 | 4       | 2019-02-01 | Shoes         | 80               |
| 15 | 4       | 2019-01-11 | Shirts        | 60               |
| 16 | 3       | 2019-04-19 | Shirts        | 50               |
| 17 | 7       | 2019-04-19 | Suit          | 150              |
| 18 | 15      | 2019-04-19 | Skirt         | 30               |
| 19 | 15      | 2019-04-20 | Dresses       | 200              |
| 20 | 12      | 2019-01-11 | Coat          | 125              |
| 21 | 7       | 2019-04-01 | Suit          | 50               |
| 22 | 7       | 2019-04-02 | Skirt         | 30               |
| 23 | 7       | 2019-04-03 | Dresses       | 50               |
| 24 | 7       | 2019-04-04 | Coat          | 25               |
| 25 | 7       | 2019-04-19 | Coat          | 125              |

```
select * from stratascrach_customers;
```

| id | first_name | last_name | city          | address              | phone_number |
|----|------------|-----------|---------------|----------------------|--------------|
| 8  | John       | Joseph    | San Francisco |                      | 928-386-8164 |
| 7  | Jill       | Michael   | Austin        |                      | 813-297-0692 |
| 4  | William    | Daniel    | Denver        |                      | 813-368-1200 |
| 5  | Henry      | Jackson   | Miami         |                      | 808-601-7513 |
| 13 | Emma       | Isaac     | Miami         |                      | 808-690-5201 |
| 14 | Liam       | Samuel    | Miami         |                      | 808-555-5201 |
| 15 | Mia        | Owen      | Miami         |                      | 808-640-5201 |
| 1  | Mark       | Thomas    | Arizona       | 4476 Parkway Drive   | 602-993-5916 |
| 12 | Eva        | Lucas     | Arizona       | 4379 Skips Lane      | 301-509-8805 |
| 6  | Jack       | Aiden     | Arizona       | 4833 Coplin Avenue   | 480-303-1527 |
| 2  | Mona       | Adrian    | Los Angeles   | 1958 Peck Court      | 714-409-9432 |
| 10 | Lili       | Oliver    | Los Angeles   | 3832 Euclid Avenue   | 530-695-1180 |
| 3  | Farida     | Joseph    | San Francisco | 3153 Rhapsody Street | 813-368-1200 |
| 9  | Justin     | Alexander | Denver        | 4470 McKinley Avenue | 970-433-7589 |
| 11 | Frank      | Jacob     | Miami         | 1299 Randall Drive   | 808-590-5201 |

Calculate the percentage of the total spend a customer spent on each order. Output the customer’s first name, order details,
and percentage of the order cost to their total spend across all orders.

Assume each customer has a unique first name (i.e., there is only 1 customer named Karen in the dataset) and that customers
place at most only 1 order a day.

Percentages should be represented as fractions

```
select first_name, order_details,
       round(100.0 * total_order_cost / sum(total_order_cost) over(partition by first_name),2) as percentage_of_total_spent
from stratascrach_orders o
join stratascrach_customers c
on o.cust_id = c.id;
```

| first_name | order_details | percentage_of_total_spent |
|------------|---------------|---------------------------|
| Eva        | Coat          | 60.98                     |
| Eva        | Slipper       | 9.76                      |
| Eva        | Shirts        | 29.27                     |
| Farida     | Coat          | 38.46                     |
| Farida     | Shoes         | 30.77                     |
| Farida     | Skirt         | 11.54                     |
| Farida     | Shirts        | 19.23                     |
| Henry      | Shoes         | 100                       |
| Jill       | Shoes         | 14.95                     |
| Jill       | Coat          | 4.67                      |
| Jill       | Suit          | 9.35                      |
| Jill       | Skirt         | 5.61                      |
| Jill       | Coat          | 4.67                      |
| Jill       | Dresses       | 9.35                      |
| Jill       | Suit          | 28.04                     |
| Jill       | Coat          | 23.36                     |
| Mia        | Shirts        | 11.11                     |
| Mia        | Jeans         | 14.81                     |
| Mia        | Shirts        | 9.26                      |
| Mia        | Boats         | 18.52                     |
| Mia        | Skirt         | 5.56                      |
| Mia        | Dresses       | 37.04                     |
| Mia        | Slipper       | 3.7                       |
| William    | Shoes         | 57.14                     |
| William    | Shirts        | 42.86                     |
