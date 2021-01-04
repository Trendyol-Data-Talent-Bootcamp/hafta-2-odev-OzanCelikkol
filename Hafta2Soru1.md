# Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
select Sport, Year, Country, count(Medal) as Total_medals
from ozan_celikkol.summer_medals
where Year >= 2008
group by Sport, Year, Country
order by Sport, Year, Total_medals desc;


/* Find the countries got 1st, 3rd, 5th most medals for every Sport after 1980. */

select distinct Sport,
first_value(Country) over (partition by Sport order by  Sport, count(Medal) desc) as First_most_medals,
nth_value(Country, 3) over (partition by Sport order by  Sport, count(Medal) desc rows between unbounded preceding and 4 following) as Third_most_medals,
nth_value(Country, 5) over (partition by Sport order by  Sport, count(Medal) desc rows between unbounded preceding and 4 following) as Fifth_most_medals,
from ozan_celikkol.summer_medals
where Year >= 1980
group by Country, Sport
order by Sport desc;



```
