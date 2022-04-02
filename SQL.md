
#### 1.1 Вывести список офисов (Location) с количеством активных сотрудников в каждом (по убыванию кол-ва сотрудников).
~~~sql
select Location, count(Name) as employees_count  
from Employees  
where IsActive is True  
GROUP BY Location  
order by employees_count desc;  
~~~

#### 1.2 Вывести список емейлов и имен сотрудников, у которых в подчинении больше трех человек.
~~~sql
select em.Manager_fk as email, Name  
from EmployeeManagers em  
join Employees e on em.Manager_fk = e.PrimaryEmail  
group by Manager_fk, Name  
having count(em.Employees_fk) > 3;
~~~

#### 1.3 Вывести сотрудников самой большой команды (под командой подразумевается Manager и его подчиненные)
~~~sql
select em.Manager_fk, em.Employees_fk, e.Name, e.SalaryYear  
from EmployeeManagers em  
join Employees e on e.PrimaryEmail = em.Employees_fk  
where em.Manager_fk = (  
    select Manager_fk  
    from EmployeeManagers  
    group by Manager_fk  
    order by count(Employees_fk) desc  
    limit 1)  
order by e.SalaryYear desc;
~~~

#### 1.4 Вывести пары сотрудник-менеджер, которые находятся в разных офисах.
~~~sql
select case  
    when e_manager.Location != e_employee.Location then em.Manager_fk end, em.Employees_fk  
from EmployeeManagers em  
join Employees e_manager on e_manager.Manager_fk = em.PrimaryEmail  
join Employees e_employee on e_employee.Employees_fk = em.PrimaryEmail;  
~~~

#### 1.5 Вывести процентное соотношение количества сотрудников в офисах
~~~sql
select  
       (select count(PrimaryEmail) from Employees where Location = 'Perm')::float / count(PrimaryEmail)::float * 100 as Perm_employees,  
       (select count(PrimaryEmail) from Employees where Location = 'LA')::float / count(PrimaryEmail)::float * 100 as LA_employees,  
       (select count(PrimaryEmail) from Employees where Location = 'Seoul')::float / count(PrimaryEmail)::float * 100 as Seoul_employees  
from Employees;  
~~~

#### 3 Приведен SQL запрос с ошибками. Постарайтесь исправить все ошибки в запросе.
~~~sql
select c.id as company_id,  
       count(o.id) as opportunities_count  
from Companies c  
         left join Opportunities o on o.id = c.id  
         left join Meetings m on m.Company = c.id  
where o.Stage not like 'Closed%'  
group by c.id;
~~~
