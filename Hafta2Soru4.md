# soru 4)

```SQL
merge ozan_celikkol.content_category as target
using ozan_celikkol.content_category_20201222_00_59 as source
on target.id = source.id
when matched and target.cdc_date != source.cdc_date
then update set target.cdc_date = source.cdc_date, target.category = source.category
when matched and target.category != source.category
then update set target.category = source.category
when not matched by target 
then  insert (cdc_date, is_deleted, id, category) VALUES(source.cdc_date, source.is_deleted, source.id, source.category)
when not matched by source
then update set target.is_deleted = True;

/* Below from here is for check. */

with hash_table_1 as
(
select *, farm_fingerprint(to_json_string(original_target)) as hash_check_1
from ozan_celikkol.content_category_target as original_target
),

hash_table_2 as
(
select *, farm_fingerprint(to_json_string(created_target)) as hash_check_2
from ozan_celikkol.content_category as created_target
)

select *
from hash_table_1
right outer join hash_table_2
on hash_table_1.id = hash_table_2.id
where hash_table_1.hash_check_1 <> hash_table_2.hash_check_2
```
