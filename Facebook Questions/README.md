### Problem1

```
select * from fb_friend_requests;
```

| user_id_sender | user_id_receiver | date       | action   |
|----------------|------------------|------------|----------|
| ad4943sdz      | 948ksx123d       | 2020-01-04 | sent     |
| ad4943sdz      | 948ksx123d       | 2020-01-06 | accepted |
| dfdfxf9483     | 9djjjd9283       | 2020-01-04 | sent     |
| dfdfxf9483     | 9djjjd9283       | 2020-01-15 | accepted |
| ffdfff4234234  | lpjzjdi4949      | 2020-01-06 | sent     |
| fffkfld9499    | 993lsldidif      | 2020-01-06 | sent     |
| fffkfld9499    | 993lsldidif      | 2020-01-10 | accepted |
| fg503kdsdd     | ofp049dkd        | 2020-01-04 | sent     |
| fg503kdsdd     | ofp049dkd        | 2020-01-10 | accepted |
| hh643dfert     | 847jfkf203       | 2020-01-04 | sent     |
| r4gfgf2344     | 234ddr4545       | 2020-01-06 | sent     |
| r4gfgf2344     | 234ddr4545       | 2020-01-11 | accepted |

What is the overall friend acceptance rate by date? Your output should have the rate of acceptances by the date the request was sent.
Order by the earliest date to latest.

Assume that each friend request starts by a user sending (i.e., user_id_sender) a friend request to another user (i.e., user_id_receiver)
that's logged in the table with action = 'sent'. If the request is accepted, the table logs action = 'accepted'. If the request is not accepted,
no record of action = 'accepted' is logged.

```
with reciever as (
select date, user_id_sender, user_id_receiver
from fb_friend_requests f1
where action = 'accepted'
), sender as 
(
select date, user_id_sender, user_id_receiver
from fb_friend_requests f1
where action = 'sent'
)
select s.date, 1.0*count(r.user_id_receiver) / 
count(s.user_id_sender) as percentage
from reciever r
right join sender s on r.user_id_sender=s.user_id_sender and
r.user_id_receiver=s.user_id_receiver
group by s.date;
```

| date       | percentage |
|------------|------------|
| 2020-01-04 | 0.75       |
| 2020-01-06 | 0.667      |


### Problem2

Assume you have the table below containing information on Facebook user actions. Write a query to obtain the active user retention in July 2022.
Output the month (in numerical format 1, 2, 3) and the number of monthly active users (MAUs).

Hint: An active user is a user who has user action ("sign-in", "like", or "comment") in the current month and last month.

user_actions table:

| user_id | event_id | event_type | event_date          |
|---------|----------|------------|---------------------|
| 445     | 7765     | sign-in    | 05/31/2022 12:00:00 |
| 742     | 6458     | sign-in    | 06/03/2022 12:00:00 |
| 445     | 3634     | like       | 06/05/2022 12:00:00 |
| 742     | 1374     | comment    | 06/05/2022 12:00:00 |
| 648     | 3124     | like       | 06/18/2022 12:00:00 |

Solution:

```
with total as(
SELECT extract(month from event_date) as monthh, user_id
FROM user_actions
), final as (
select t1.monthh, t1.user_id, count(t1.user_id)
from total t1 join total t2 on t1.user_id = t2.user_id
--where t1.monthh = 7 and user_id in (select )
group by t1.monthh, t1.user_id
order by t1.monthh
)
select monthh as month, count(user_id) as monthly_active_users
from final
where monthh = 7 and 
user_id in (select user_id from final where monthh = 6)
group by month
order by month;
```
Output:

| month | monthly_active_users |
|-------|----------------------|
| 7     | 2                    |

