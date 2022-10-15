Say you have access to all the transactions for a given merchant account. Write a query to print the cumulative balance of the merchant account at the end of each day, with the total balance reset back to zero at the end of the month. Output the transaction date and cumulative balance.

```
select * from transactions
```
| transaction_id | type       | amount | transaction_date    |
|----------------|------------|--------|---------------------|
| 19153          | deposit    | 65.90  | 07/10/2022 10:00:00 |
| 53151          | deposit    | 178.55 | 07/08/2022 10:00:00 |
| 29776          | withdrawal | 25.90  | 07/08/2022 10:00:00 |
| 16461          | withdrawal | 45.99  | 07/08/2022 13:00:00 |
| 77134          | deposit    | 32.60  | 07/10/2022 10:00:00 |
| 41515          | withdrawal | 16.31  | 06/01/2022 10:00:00 |
| 624804         | deposit    | 165.00 | 06/17/2022 10:00:00 |
| 757995         | deposit    | 7.50   | 06/30/2022 10:00:00 |
| 112465         | withdrawal | 295.95 | 06/28/2022 10:00:00 |
| 996414         | withdrawal | 67.00  | 06/05/2022 10:00:00 |
| 946461         | deposit    | 815.00 | 06/01/2022 10:00:00 |
| 125614         | withdrawal | 300.00 | 07/02/2022 10:00:00 |
| 146641         | withdrawal | 100.00 | 07/13/2022 10:00:00 |
| 136414         | deposit    | 599.30 | 07/02/2022 10:00:00 |

Solution:

```
with t1 as (
SELECT type, 
sum(case when type = 'deposit' then amount
when type = 'withdrawal' then -amount end) as total,
transaction_date FROM transactions
group by type,transaction_date
order by transaction_date
)
SELECT distinct(date(transaction_date)), 
sum(total) over(partition by extract(month from transaction_date)
order by extract(day from transaction_date)) as balance
from t1
group by transaction_date, total
;
```

| date                | balance |
|---------------------|---------|
| 06/01/2022 00:00:00 | 798.69  |
| 06/05/2022 00:00:00 | 731.69  |
| 06/17/2022 00:00:00 | 896.69  |
| 06/28/2022 00:00:00 | 600.74  |
| 06/30/2022 00:00:00 | 608.24  |
| 07/02/2022 00:00:00 | 299.30  |
| 07/08/2022 00:00:00 | 405.96  |
| 07/10/2022 00:00:00 | 504.46  |
| 07/13/2022 00:00:00 | 404.46  |
