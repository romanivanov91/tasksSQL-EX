<h1>Решения задач с https://sql-ex.ru</h1>

<h4>Задание: 23 (Serge I: 2003-02-14)</h4>
 <p>Найдите производителей, которые производили бы как ПК
со скоростью не менее 750 МГц, так и ПК-блокноты со скоростью не менее 750 МГц.
Вывести: Maker</p>

 <h4>Решение:</h4>
<p>select distinct a.maker
from (select maker, speed
      from Product inner join PC on Product.model = PC.model
      where PC.speed >= 750) as a
      inner join
      (select maker, speed
      from Product inner join Laptop on Product.model = Laptop.model
      where Laptop.speed >= 750) as b
      on a.maker = b.maker</p>
      
<h3>Задание: 32 (Serge I: 2003-02-17)</h3>
<p>Одной из характеристик корабля является половина куба калибра его главных орудий (mw). С точностью до 2 десятичных знаков определите среднее значение mw для кораблей каждой страны, у которой есть корабли в базе данных.</p>

<h4>Решение:</h4>
<p>select country, cast(avg((bore*bore*bore)/2) as numeric(6,2)) 
from   (select country, name, bore 
        from ships as s join classes as c 
        on s.class=c.class
        union
        select country, ship, bore 
        from outcomes as o join classes as c 
        on o.ship = c.class) as t
group by country</p>

<h4>Задание: 33 (Serge I: 2002-11-02)</h4>
<p>Укажите корабли, потопленные в сражениях в Северной Атлантике (North Atlantic). Вывод: ship</p>

<h4>Решение:</h4>
<p>select ship
from Outcomes
where Outcomes.battle = 'North Atlantic' and Outcomes.result = 'sunk'</p>
      
<h4>Задание: 38 (Serge I: 2003-02-19)</h4>
<p>Найдите страны, имевшие когда-либо классы обычных боевых кораблей ('bb') и имевшие когда-либо классы крейсеров ('bc').</p>

<h4>Решение:</h4>
<p>select country 
from Classes where type = 'bb'
intersect
select country 
from Classes where type = 'bc'</p>

<h4>Задание: 39 (Serge I: 2003-02-14)</h4>
<p>Найдите корабли, `сохранившиеся для будущих сражений`; т.е. выведенные из строя в одной битве (damaged), они участвовали в другой, произошедшей позже.</p>

<p>Почему-то система мое решение не принимает. Если есть возможность прошу подсказать. Подсказка выдает такой текст: Следует исключить дубликаты, т.к. корабль может быть поврежден в нескольких сражениях. С distinct также не принимает решение</p>

<h4>Решение:</h4>
<p>select distinct ship
from Outcomes as o1
     inner join
     Battles as b1
     on o1.battle = b1.name
where o1.result = 'ok' 
      and ship in (select distinct ship
                   from Outcomes as o2
                   inner join
                   Battles as b2
                   on o2.battle = b2.name and o2.result = 'damaged'
                   where b2.date < b1.date)</p>



