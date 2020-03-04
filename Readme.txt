Лабораторная работа №11

1.	Функции
a)	Ввести в таблицу sale записи за текущую дату, которые отличаются на 1 минуту
и вывести данные о продажах за текущий день, используя функцию getdate().

declare @counter int;
set @counter = 1;
while @counter <=10
begin
insert into sale values(dateadd(mi, @counter,
getdate()), 100, 2241, 10, 2);
set @counter=@counter+1;
end;

select * from sale 
where convert(varchar(12),data, 101)=
convert(varchar(12),getdate(), 101)

create function day_only(@data datetime)
returns varchar(12)
as
begin
return convert(varchar(12), @data, 101);
end

select * from sale 
where dbo.day_only(data)=
dbo.day_only(getdate())

b)	Вывести в результирующую таблицу: название товара, его продажную стоимость, среднюю продажную стоимость товаров и для каждого товара – разность между его продажной стоимостью и средней стоимостью товаров.

select p_desc, price,
(select avg(price) from product) average,
round(price - (select avg(price) from product), 2) 
differenc
from product;

create function average_price()
returns real
as
begin
return (select round(avg(price), 2) from product)
end

create function price_difference(@price real)
returns real
as
begin
return round(@price - dbo.average_price(), 2)
end

select p_desc, price,
dbo.average_price() average,
dbo.price_difference(price)
differenc
from product;


c)	Создать функцию, которая возвращает таблицу со следующими данными: код клиента, фамилия клиента и его город, разделенные пробелом.
create function custom_list()
returns table 
as
return (select c_id,
c_name + ' ' + address name_address
from customer);

select * from custom_list()


1.	Написать функцию для определения количества сотрудников в указанном отделе.
2.	Написать функцию, которая возвращает таблицу служащих, которые прямо или косвенно подчиняются  сотруднику Terii Cardon (sp_id, sp_name).


