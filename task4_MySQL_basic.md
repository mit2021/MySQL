项目十六 
### 项目十六 分数排名  （难度：中等）
依然是昨天的分数表，实现排名功能，但是排名需要是非连续的，如下：
+-------+------+
| Score | Rank |
+-------+------+
| 4.00  | 1    |
| 4.00  | 1    |
| 3.85  | 3    |
| 3.65  | 4    |
| 3.65  | 4    |
| 3.50  | 6    |
+-------+------



    mysql> select score,
    -> (select count(score)+1 from scores s1 where s1.score>s2.score)as rank
    -> from scores s2
    -> order by score desc;
    +-------+------+
    | score | rank |
    +-------+------+
    |  4.00 |1 |
    |  4.00 |1 |
    |  3.85 |3 |
    |  3.65 |4 |
    |  3.65 |4 |
    |  3.50 |6 |
    +-------+------+
    6 rows in set (0.00 sec)



项目十七：查询回答率最高的问题 （难度：中等）
求出**survey_log**表中回答率最高的问题，表格的字段有：**uid, action, question_id, answer_id, q_num, timestamp**。
uid是用户id；action的值为：“show”， “answer”， “skip”；当action是"answer"时，answer_id不为空，相反，当action是"show"和"skip"时为空（null）；q_num是问题的数字序号。
写一条sql语句找出回答率最高的问题。
**举例：**
**输入**
| 12345678 | +------+-----------+--------------+------------+-----------+------------+\| uid  \| action    \| question_id  \| answer_id  \| q_num     \| timestamp  \|+------+-----------+--------------+------------+-----------+------------+\| 5    \| show      \| 285          \| null       \| 1         \| 123        \|\| 5    \| answer    \| 285          \| 124124     \| 1         \| 124        \|\| 5    \| show      \| 369          \| null       \| 2         \| 125        \|\| 5    \| skip      \| 369          \| null       \| 2         \| 126        \|+------+-----------+--------------+------------+-----------+------------+ | 
|----|----|
**输出**
| 12345 | +-------------+\| survey_log  \|+-------------+\|    285      \|+-------------+ | 
|----|----|
**说明**
问题285的回答率为1/1，然而问题369的回答率是0/1，所以输出是285。
**注意：** 最高回答率的意思是：同一个问题出现的次数中回答的比例。



    select question_id as survey_log
    from(
    select (sum(case when `action` like 'answer' then 1 else 0 end) / sum(case when `action` like 'show' then 1 else 0 end)) as rate,question_id
    from survey_log
    group by question_id
    order by rate desc
    limit 1) x
    


### 项目十八：各部门前3高工资的员工（难度：中等）
将项目7中的employee表清空，重新插入以下数据（其实是多插入5,6两行）：
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
编写一个 SQL 查询，找出每个部门工资前三高的员工。例如，根据上述给定的表格，查询结果应返回：
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+

此外，请考虑实现各部门前N高工资的员工功能。

    
    select * from (
    select b.Department,a.name,a.salary
    row_number() over(partition by b.Department order by a.salary desc) rn
     from Employee a 
    left join Department b on a.DepartmentId  = b.Id
    ) temp
    where temp.rn<4 
    order by temp.rn 


### 项目十九：平面上最近距离
**point_2d** 表包含一个平面内一些点（超过两个）的坐标值（x，y）。
写一条查询语句求出这些点中的最短距离并保留2位小数。
| x   | y   | 
|:----|:----|
| -1   | -1   | 
| 0   | 0   | 
| -1   | -2   | 

最短距离是1，从点（-1，-1）到点（-1，2）。所以输出结果为：
| shortest   | 
|:----|
| 1.00   | 

**注意：** 所有点的最大距离小于10000。
    
    selectround(SQRT(min(power((p1.x-p2.x),2)+power((p1.y-p2.y),2))),2)"shortest"
    
    frompoint_2d p1joinpoint_2d p2
    
    onp1.x!=p2.xorp1.y!=p2.y;



### 项目二十：行程和用户（难度：困难）
Trips 表中存所有出租车的行程信息。每段行程有唯一键 Id，Client_Id 和 Driver_Id 是 Users 表中 Users_Id 的外键。Status 是枚举类型，枚举成员为 (‘completed’, ‘cancelled_by_driver’, ‘cancelled_by_client’)。
+----+-----------+-----------+---------+--------------------+----------+
| Id | Client_Id | Driver_Id | City_Id |        Status      |Request_at|
+----+-----------+-----------+---------+--------------------+----------+
| 1  |     1     |    10     |    1    |     completed      |2013-10-01|
| 2  |     2     |    11     |    1    | cancelled_by_driver|2013-10-01|
| 3  |     3     |    12     |    6    |     completed      |2013-10-01|
| 4  |     4     |    13     |    6    | cancelled_by_client|2013-10-01|
| 5  |     1     |    10     |    1    |     completed      |2013-10-02|
| 6  |     2     |    11     |    6    |     completed      |2013-10-02|
| 7  |     3     |    12     |    6    |     completed      |2013-10-02|
| 8  |     2     |    12     |    12   |     completed      |2013-10-03|
| 9  |     3     |    10     |    12   |     completed      |2013-10-03| 
| 10 |     4     |    13     |    12   | cancelled_by_driver|2013-10-03|
+----+-----------+-----------+---------+--------------------+----------+
Users 表存所有用户。每个用户有唯一键 Users_Id。Banned 表示这个用户是否被禁止，Role 则是一个表示（‘client’, ‘driver’, ‘partner’）的枚举类型。
+----------+--------+--------+
| Users_Id | Banned |  Role  |
+----------+--------+--------+
|    1     |   No   | client |
|    2     |   Yes  | client |
|    3     |   No   | client |
|    4     |   No   | client |
|    10    |   No   | driver |
|    11    |   No   | driver |
|    12    |   No   | driver |
|    13    |   No   | driver |
+----------+--------+--------+
写一段 SQL 语句查出 **2013年10月1日 **至 **2013年10月3日 **期间非禁止用户的取消率。基于上表，你的 SQL 语句应返回如下结果，取消率（Cancellation Rate）保留两位小数。
+------------+-------------------+
|     Day    | Cancellation Rate |
+------------+-------------------+
| 2013-10-01 |       0.33        |
| 2013-10-02 |       0.00        |
| 2013-10-03 |       0.50        |
+------------+-------------------+


    
    select
    t.request_at as day
    ,round(count(distinct if(u1.banned = 'no' and u2.banned ='no' and t.status!='completed',t.id,null))/count(distinct if(u1.banned = 'no' and u2.banned ='no',t.id,null)),2) as "cancellation rate"
    from trips t
    left join users u1
    on t.client_id=u1.users_id
    left join users u2
    on t.driver_id=u2.users_id
    where t.request_at between '2013-10-01' and '2013-10-03'
    group by 
    t.request_at