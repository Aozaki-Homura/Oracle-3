# tset1
- 查询一
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
 分析:查询一的目的是查找部门名为“IT”和“sales”的部门人数和平均公司,查询一先通过部门名得到部门id，通过部门id找到对应的员工，并计算人数和平均工资，再按照部门名排序
 ![](https://github.com/accountw/oracle/blob/master/test1/1.png)
 - 查询二
 ```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
分析:查询二的目的是查找部门名为“IT”和“sales”的部门人数和平均公司，查询二先查询出每个部门的员工，人数和平均工资，再按部门名排序，然后找出it和sales的部门
 ![](https://github.com/accountw/oracle/blob/master/test1/2.png)
 我认为查询二是最优的
  ![](https://github.com/accountw/oracle/blob/master/test1/3.png)
  - 查询三
   ```SQL
SELECT e.employee_id as"工号", e.first_name as"姓",e.last_name as"名",
e.phone_number as"电话", e.email as"邮箱"
from hr.employees e
where e.employee_id in (100，107)
order by e.employee_id;
```
分析:查询三的目的是查找员工号为100和107员工的工号姓名电话邮箱，先在员工表里找到id为100和107的员工，再查找姓名电话邮箱
查询三
  ![](https://github.com/accountw/oracle/blob/master/test1/4.png)
  优化
   ![](https://github.com/accountw/oracle/blob/master/test1/5.png)
