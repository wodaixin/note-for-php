### MySQL 的SQL语句编写



#### MySQL 的关联 UPDATE 语句

关联更新：

``` sql
UPDATE A，B SET A.c1 = B.c1，A.c2 = B.c2 WHERE A.id = B.id

UPDATE A inner join B on A.id = B.id SET A.c1 = B.c1, A.c2 = B.c2 where ...
```



六种关联查询：

交叉连接(CROSS JOIN)，内连接(INNER JOIN)，外连接(LEFT / RIGHT JOIN)，联合查询(UNION 与 UNION ALL)，全连接(FULL JOIN)



交叉连接：

```sql
select * from a,b,c

select * from a cross join b (cross join c)
```

没有任何关联条件，结果是笛卡尔积，结果集合会很大，没有意义，很少使用



内连接：

```sql
select * from a,b where a.id = b.id /*等值  不等值  自连接三类*/

select * from a inner join b on a.id = b.id
```

多表中同时符合某种条件的数据记录的集合



外连接：

左外连接：`left outer join`，以左表为主，先查询出左表，按照 `on` 后的关联条件匹配右图表，没有匹配到的用 `null` 填充，可以简写成 `left join`

右外连接：`right outer join`，以右表为主，先查询出右表，按照 `on` 后的关联条件匹配左图表，没有匹配到的用 `null` 填充，可以简写成 `right join`



联合查询：

``` sql
select * from a union select * from b union ...
```

就是把多个结果集集中在一起，`UNION` 前的结果为基准，需要注意的是联合查询的列数要相等，相同记录行会合并

如果使用 `UNION ALL` 不会合并重复的记录行



全连接：

MySQL 本身不支持全连接

可以使用 `left join` 和 `union` 和 `right jooin` 联合使用



嵌套查询：

用一条 SQL 语句的结果作为另外一条 SQL 语句的条件

``` sql
select * from a where id in (select id from b)
```





