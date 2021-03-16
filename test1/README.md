# oracle
姓名：赵辉

学号：201810414229	

班级：18软工2班

实验目的：分析SQL执行计划，执行SQL语句的优化指导。理解分析SQL语句的执行计划的重要作用。

实验内容：

\- 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。

\- 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。

\- 设计自己的查询语句，并作相应的分析，查询语句不能太简单。

（1）两个不同的查询。

![image-20210315163010990](C:\Users\16508\AppData\Roaming\Typora\typora-user-images\image-20210315163010990.png)

第一种方法查询时间为1.92s。首先通过where选择对应的行，并且进行了筛选，然后通过group by组合筛选结果。

![image-20210315163110225](C:\Users\16508\AppData\Roaming\Typora\typora-user-images\image-20210315163110225.png)

第二种方法查询 时间为0.068s.首先根据where选择对应的行，其次根据group by字句组合行，最后根据根据having字句筛选组为“IT”,"Sales"。

（2）教材文档分析

第一种查询要比第二种查询更优，第一 中查询除了consistent比第二个稍差了一些，其他参数都优于第二种查询。但是第一种查询可以通过在departments表上创建一个基于department_name和department_id字段的索引，这样就可以加快查询department_name的速度。

（3）自定义查询

`SELECT d.department_name,count(e.job_id)as "部门总人数",
avg(e.salary)as "平均工资"
from hr.departments d
 left join hr.employees e on
d.department_id = e.department_id
where d.department_name in ('IT','Sales')
GROUP BY d.department_name;`

LEFT JOIN是以左表的记录为基础的，可以将hr.departments看成左表，hr.employees看成右表，它的结果集是左表的全部数据，再加上左右两表匹配的数据。