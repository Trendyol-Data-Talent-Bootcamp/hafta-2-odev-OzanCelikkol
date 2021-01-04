# Soru 3) "sample.pageview": tablosunda 1 gün içerisinde trendyol.com a gelen tüm ziyaretlerin logu var.

```SQL
with user_count as(
select *
from(
select
timestamp_trunc(view_ts, minute) as user_by_minute, HLL_COUNT.init(deviceid) as user_devices
from ozan_celikkol.pageview
group by user_by_minute
order by user_by_minute)),

time_window as(
select
user_by_minute, array_agg(user_devices) over (rows between 4 preceding and current row) as user_by_window
from user_count)

select
user_by_minute, (select HLL_COUNT.merge(user_by_minute)
from UNNEST(user_by_window) as user_by_minute) as active_user
from time_window
order by user_by_minute

/* Thanks to Faruk Aşçı for his help. */
```
