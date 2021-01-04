# Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

```SQL
select *
from ozan_celikkol.summer_medals
where Athlete = 'ASHFORD, Evelyn';
/* Find the active user number for every minute.*/

select distinct Athlete, Sport
from
(select Athlete,Sport, Year,
case
when Year - lag(Year, 1) over (partition by Athlete order by Athlete, Year) = 4 
and Year - lag(Year, 2) over (partition by Athlete order by Athlete, Year) = 8
then 'Yes'
else 'No'
end Got_medals_for_last_three
from ozan_celikkol.summer_medals
where Year > 1980
group by Sport, Athlete, Year
order by Athlete, Year)
where Got_medals_for_last_three = 'Yes'
order by Athlete;
```
