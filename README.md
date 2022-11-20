Решения задач с https://sql-ex.ru

Задание: 23 (Serge I: 2003-02-14)
Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker

Решение:
select distinct a.maker
from (select maker, speed
      from Product inner join PC on Product.model = PC.model
      where PC.speed >= 750) as a
      inner join
      (select maker, speed
      from Product inner join Laptop on Product.model = Laptop.model
      where Laptop.speed >= 750) as b
      on a.maker = b.maker
      
Задание: 32 (Serge I: 2003-02-17)
Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2 десятичных знаков определите среднее значение mw для кораблей каждой страны, у которой есть корабли в базе данных.

Решение:
select country, cast(avg((bore*bore*bore)/2) as numeric(6,2)) 
from   (select country, name, bore 
        from ships as s join classes as c 
        on s.class=c.class
        union
        select country, ship, bore 
        from outcomes as o join classes as c 
        on o.ship = c.class) as t
group by country
      
Задание: 38 (Serge I: 2003-02-19)
Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').

Решение:
select country 
from Classes where type = 'bb'
intersect
select country 
from Classes where type = 'bc'


