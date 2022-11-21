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
      
<h4>Задание: 32 (Serge I: 2003-02-17)</h4>
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

<h4>Задание: 41 (Serge I: 2019-05-31)</h4>
<p>Для каждого производителя, у которого присутствуют модели хотя бы в одной из таблиц PC, Laptop или Printer,
определить максимальную цену на его продукцию.
Вывод: имя производителя, если среди цен на продукцию данного производителя присутствует NULL, то выводить для этого производителя NULL,
 иначе максимальную цену.</p>
 
 <h4>Решение:</h4>
 <p>select maker, case when count(price) = count(*) then max(price) end
from Product as a
     inner join 
     (select model, price from PC
      union
      select model, price from Laptop
      union
      select model, price from Printer) as b on a.model=b.model
  group by maker</p>

<h4>Задание: 43 (qwrqwr: 2011-10-28)</h4>
<p>Укажите сражения, которые произошли в годы, не совпадающие ни с одним из годов спуска кораблей на воду.</p>

<h4>Решение</h4>
<p>select name
from Battles
where year(date) not in (select launched
                   from Ships
                   where launched is not null)</p>

<h4>Задание: 44 (Serge I: 2002-12-04)</h4>
<p>Найдите названия всех кораблей в базе данных, начинающихся с буквы R.</p>

<h4>Решение:</h4>
<p>select name
from Ships     
where name like 'R%'
union
select ship
from Outcomes
where ship like 'R%'</p>

<h4>Задание: 45 (Serge I: 2002-12-04)</h4>
<p>Найдите названия всех кораблей в базе данных, состоящие из трех и более слов (например, King George V).
Считать, что слова в названиях разделяются единичными пробелами, и нет концевых пробелов.</p>

<h4>Решение</h4>
<p>select name
from Ships     
where name like '% % %'
union
select ship
from Outcomes
where ship like '% % %'
</p>

<h4>Задание: 46 (Serge I: 2003-02-14)<h4>
<p>Для каждого корабля, участвовавшего в сражении при Гвадалканале (Guadalcanal), вывести название, водоизмещение и число орудий.</p>
 
 <h4>Решение:</h4>
 <p>select o.ship, displacement, numGuns 
from (select name AS ship, displacement, numGuns
     from Ships as s 
     JOIN 
     Classes as c 
     on c.class=s.class
     union
     select class AS ship, displacement, numGuns
     from Classes as c) as a
     right join 
     Outcomes as o
     on o.ship=a.ship
where battle = 'Guadalcanal'</p>
 
 <h4>Задание: 48 (Serge I: 2003-02-16)</h4>
 <p>Найдите классы кораблей, в которых хотя бы один корабль был потоплен в сражении.</p>
 
 <h4>Решение</h4>
 <p>select distinct c.class
from Classes as c
     left join 
     Ships as s ON s.class = c.class
where c.class in (select ship
                  from Outcomes
                  where result = 'sunk') or
      s.name in (select ship
                  from Outcomes
  where result = 'sunk')</p>
  
<h4>Задание: 49 (Serge I: 2003-02-17)</h4>
<p>Найдите названия кораблей с орудиями калибра 16 дюймов (учесть корабли из таблицы Outcomes).</p>

<h4>Решение:</h4>
<p>select name
from (select name, bore
      from Classes
      right join
      Ships
      on Classes.class = Ships.class
      union
      select ship as name, bore
      from Classes
      right join
      Outcomes 
      on Outcomes.ship = Classes.class) as cso
where bore = '16'</p>
