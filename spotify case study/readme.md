```
CREATE table spotify_activity
(
user_id varchar(20),
event_name varchar(20),
event_date date,
country varchar(20)
);
--delete from activity;
insert into spotify_activity values (1,'app-installed','2022-01-01','India')
,(1,'app-purchase','2022-01-02','India')
,(2,'app-installed','2022-01-01','USA')
,(3,'app-installed','2022-01-01','USA')
,(3,'app-purchase','2022-01-03','USA')
,(4,'app-installed','2022-01-03','India')
,(4,'app-purchase','2022-01-03','India')
,(5,'app-installed','2022-01-03','SL')
,(5,'app-purchase','2022-01-03','SL')
,(6,'app-installed','2022-01-04','Pakistan')
,(6,'app-purchase','2022-01-04','Pakistan');
```

-- the activity table shows the app-installed and app purchase activities for spotify app along with country details

```
select * from spotify_activity;
```


Question 1: find total active users each day
event date total_active_users

2022-01-01 3

2022-01-02 1

2022-01-03 3

2022-01-04 1
 
 ```
 select event_date, count(distinct user_id) as total_active_users
from spotify_activity
group by event_date
order by event_date;
```

Question 2: find total active users each week
week_number total active_users

1            3

2            5

 ```
 select extract(week from event_date), count(distinct user_id) as active_users  from spotify_activity
group by extract(week from event_date);
```

 Question 3: date wise total number of users who made the purchase same day they installed the app
event date no_of_users_same_day_purchase

2022-01-01 0

2022-01-02 0

2022-01-03 2

2022-01-04 1
 
 ```
with t1 as(
    select user_id, event_date,
        case when count(distinct event_name) = 2 then user_id else null end as new_user
from spotify_activity
group by user_id, event_date
)
 select event_date, count(new_user) as no_of_users
 from t1
group by event_date
order by event_date;
```

Question 4: percentage of paid users in India, USA and any other country should be tagged as others
country percentage_users

India 40

USA 20

others 40

```
select * from spotify_activity;

with t1 as (
    select case when country in ('India', 'USA') then country else 'others' end as country,
           count(distinct user_id) as user_count
    from spotify_activity
    where event_name = 'app-purchase'
    group by case when country in ('India', 'USA') then country else 'others' end
),
total as (
 select sum(user_count) as total_users from t1
 )
select country, 100.0*(user_count / total_users) as percent_user
from t1, total;
```

Question 5: Among all the users who installed the app on a given day, how many did in app purchased on the very next day
event date cnt_users

2022-01-01 0

2022-01-02 1

2022-01-03 0

2022-01-04 0

```
select * from spotify_activity;

with prev_data as (
    select *,
           lag(event_name, 1) over (partition by user_id order by event_date) as previous_event_name,
           lag(event_date, 1) over (partition by user_id order by event_date) as previous_event_date
    from spotify_activity
),
temp1 as (
    select event_date,
           case
               when (event_name = 'app-purchase' and previous_event_name = 'app-installed'
                   and event_date - previous_event_date = 1)
                   then 1
               else 0 end as count_users
    from prev_data
)
select event_date, sum(count_users)
from temp1
group by event_date
order by event_date;
```



