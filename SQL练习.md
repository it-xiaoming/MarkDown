

# 							SQL练习

- 1.学生表

  ````sql
  create table Student(SID varchar(10),Sname varchar(10),Sage datetime,
                       Ssex varchar(10));
   insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
   insert into Student values('02' , '钱电' , '1990-12-21' , '男');
   insert into Student values('03' , '孙风' , '1990-05-20' , '男');
   insert into Student values('04' , '李云' , '1990-08-06' , '男');
   insert into Student values('05' , '周梅' , '1991-12-01' , '女');
   insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
   insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
   insert into Student values('08' , '王菊' , '1990-01-20' , '女');
  ````

- 2.课程表

  ```sql
  create table Course(CID varchar(10),Cname nvarchar(10),TID varchar(10));
  insert into Course values('01' , '语文' , '02');
  insert into Course values('02' , '数学' , '01');
  insert into Course values('03' , '英语' , '03');	
  ```

- 3.教师表

  ```sql
  create table Teacher(TID varchar(10),Tname nvarchar(10));
  insert into Teacher values('01' , '张三');
  insert into Teacher values('02' , '李四');
  insert into Teacher values('03' , '王五');	
  ```

- 4.成绩表

  ```
  create table Grade(SID varchar(10),CID varchar(10),score decimal(18,1));
  insert into Grade values('01' , '01' , 80);
  insert into Grade values('01' , '02' , 90);
  insert into Grade values('01' , '03' , 99);
  insert into Grade values('02' , '01' , 70);
  insert into Grade values('02' , '02' , 60);
  insert into Grade values('02' , '03' , 80);
  insert into Grade values('03' , '01' , 80);
  insert into Grade values('03' , '02' , 80);
  insert into Grade values('03' , '03' , 80);
  insert into Grade values('04' , '01' , 50);
  insert into Grade values('04' , '02' , 30);
  insert into Grade values('04' , '03' , 20);
  insert into Grade values('05' , '01' , 76);
  insert into Grade values('05' , '02' , 87);
  insert into Grade values('06' , '01' , 31);
  insert into Grade values('06' , '03' , 34);
  insert into Grade values('07' , '02' , 89);
  insert into Grade values('07' , '03' , 98);	
  ```

## Day 01

**1、查询"01"课程比"02"课程成绩高的学生的信息及课程分数**

​	**1.1、查询同时存在"01"课程和"02"课程的情况**

```sql
select student.*,grade1.score 课程01,grade2.score 课程02
from student,grade grade1,grade grade2
where student.sid = grade1.sid and student.sid = grade2.sid and grade1.cid = '01' and 			grade2.cid = '02' and grade1.score > grade2.score
```

​	**1.2、查询同时存在"01"课程和"02"课程的情况和存在"01"课程但可能不存在"02"课程的情况**

```sql
select student.*,grade1.score 课程01,grade2.score 课程02
from student
left join grade grade1 on student.sid = grade1.sid and grade1.cid = '01'
left join grade grade2 on student.sid = grade2.sid and grade2.cid = '02'
where grade1.score > ifNull(grade2.score,0)
```

**2、查询"01"课程比"02"课程成绩低的学生的信息及课程分数**

​	**2.1、查询同时存在"01"课程和"02"课程的情况**

```sql
select student.*,grade1.score,grade2.score
from student,grade grade1,grade grade2
where student,sid = grade1.sid and student.sid = grade2.sid and grade1.cid = '01' and grade2.cid = '02' and grade1.score < grade2.score
```

​	**2.2、查询同时存在"01"课程和"02"课程的情况和不存在"01"课程但存在"02"课程的情况**

```sql
select student.*,grade1.score,grade2.score
from student
left join grade grade1 on student.sid = grade1.sid and grade1.cid = '01'
left join grade grade2 on student.sid = grade2.sid and grade2.cid = '02'
where ifNull(grade1.score,0) <grade2.score
```

**3、查询平均成绩大于等于60分的同学的学生编号和学生姓名和平均成绩**

```sql
select student.sid,student.sname,avg(grade.score)
from student,grade
where student.sid = grade.sid 
group by grade.sid having avg(grade.score) >= 60
```

**4、查询平均成绩小于60分的同学的学生编号和学生姓名和平均成绩**

```sql
select student.sid,student.sname,avg(grade.score)
from student,grade
where student.sid = grade.sid
group by grade.sid having avg(grade.score) < 60
```

**5、查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩**
	**5.1、查询所有有成绩的SQL**

```sql
select student.sid,student.sname,count(grade.cid),sum(grade.score)
from student,grade
where student.sid = grade.sid
group by grade.sid
```

​	**5.2、查询所有(包括有成绩和无成绩)的SQL。**

```sql
select student.sid,student.sname,count(grade.cid),sum(grade.score)
from student
left join grade on student,sid = grade,sid
group by student.sid
```

## Day 02

**6、查询"李"姓老师的数量**

```sql
select count(tname) from teacher where tname like '李%' 
```

```sql
select count(tname) from teacher where left(tname,1) = '李'
```

**7、查询学过"张三"老师授课的同学的信息**

```sql
子查询	--- 效率低
select student.*
from student
where sid in (select sid from grade where cid = (select cid from course where tid = (select tid from teacher where tname = '张三')))
```

```sql
连表查询
Select student.* from student,grade,course,teacher
where student.sid = grade.sid and grade.cid = course.cid and course.tid = teacher.tid and teacher.tname='张三'
```

**8、查询没学过"张三"老师授课的同学的信息**

```sql
子查询 --- 效率低
select student.* from 
student where sid not in (select sid fron grade where cid = (select cid from course where tid = (select tid from teacher where tname = '张三')))
```



## 知识点

- [子查询和连表查询的效率对比](https://www.cnblogs.com/cdf-opensource-007/p/6540521.html)
- in和exists的使用

> **在子查询中使用IN的性能高还是是用EXITS的性能高，有一种普遍的说法是：**
>
> **1，在外表大，内表小，外表中有索引的情况下，使用IN。**
>
> **2，在外表小，内表大，内表中有索引的情况下，使用EXITS。**



**9、查询学过编号为"01"并且也学过编号为"02"的课程的同学的信息**

- 方式一：连表查询

```sql
select student.* 
from student,grade grade1,grade grade2
where student.sid = grade1.sid and grade1.cid = '01' and student.sid = grade2.sid and 			grade2.cid = '02'
```

- 方式二：in

```sql
select student.* 
from student,grade grade1
where student.sid = grade1.sid and grade1.cid = '01' and grade1.sid in (select sid from grade grade2 where grade2.cid = '02')
```

- 方式三：exists

```sql
select student.* 
from student,grade grade1
where student.sid = grade1.sid and grade1.cid = '01' and  exists(select 1 from grade 			grade2 where grade2.cid = '02' and grade1.sid = grade2.sid)
```

- 方法四：union all

```sql
select student.* from student 
where SID in (select SID from( select  SID from Grade where CID = '01'
    union all
    select SID from Grade where CID = '02'
  ) t group by SID having count(1) = 2
)

```

**10、查询学过编号为"01"但是没有学过编号为"02"的课程的同学的信息**

- 方法一：not  in

```sql
select student.* 
from student,(select sid from grade where cid = '01') t1
where student.sid = t1.sid and t1.sid not in (select sid from grade where cid = '02')
```

- 方法二：not exists

```
select Student.* 
from Student,Grade 
where Student.SID = Grade.SID and Grade.CID = '01' 
		and not exists (Select 1 from Grade Grade_2 where Grade_2.SID = Grade.SID and 				Grade_2.CID = '02')
```



## Day 03

**11、查询没有学全所有课程的同学的信息**

​	**11.1、排除一门课程都没有选修的学生**

```sql
select Student.*
from Student,Grade
where Student.SID = Grade.SID
group by Student.SID having count(CID) < (select count(CID) from Course)
```

​	**11.2、包含一门课程都没有选修的学生**

```sql
select student.*
from student
left join grade on student.sid = grade.sid
group by student.sid having count(cid) < (select count(cid) from course)
```

<!--重-->**12、查询至少有一门课与学号为"01"的同学所学相同的同学的信息**

```sql
select distinct student.* 
from student,grade
where student.sid = grade.sid and grade.cid in (select cid from grade where sid = '01') 	and	studet.sid <> '01'
```

**<!--重-->13、查询和"01"号的同学学习的课程完全相同的其他同学的信息**

```sql
select Student.* 
from Student 
where SID in(select distinct Grade.SID from Grade where SID <> '01' and Grade.CID in (select distinct CID from Grade where SID = '01')
group by Grade.SID having count(1) = (select count(1) from Grade where SID='01'))
```

**14、查询没学过"张三"老师讲授的任一门课程的学生姓名**

```sql
select student.* 
from student 
where student.SID not in
(select distinct sc.SID from sc , course , teacher where sc.CID = course.CID and course.TID = teacher.TID and teacher.tname = '张三')
```

**15、查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩**

```sql
select student.sid,student.sname,avg(grade.score)
from student,grade,(select sid from grade where score < 60 group by sid having count(sid) >= 2) t where student.sid = grade.sid and student.sid = t.sid group by grade.sid 
```

## Day 04

**16、检索"01"课程分数小于60，按分数降序排列的学生信息**

```sql
Select student.*
from student
where student.sid in 
	(select sid from grade where cid = '01' and score < 60 order by score desc)
```

<!--重-->**17、按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩**

```sql
select student.SID 学生编号 , student.Sname 学生姓名 ,
       max(case course.Cname when '语文' then grade.score else null end)  语文 ,
       max(case course.Cname when '数学' then grade.score else null end)  数学 ,
       max(case course.Cname when '英语' then grade.score else null end)  英语 ,
       cast(avg(grade.score) as decimal(18,2)) 平均分
from Student student
left join Grade grade on student.SID = grade.SID
left join Course course on grade.CID = course.CID
group by student.SID
order by 平均分 desc
```

<!--重-->**18、查询各科成绩最高分、最低分和平均分：以如下形式显示：课程ID，课程name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率**
**及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90**

```sql
select course.cid,course.cname,max(grade.score),min(grade.score),
	cast((select sum(grade.score) from grade where  grade.cid=course.cid)/(select 				count(1) from grade where grade.cid = course.cid) as decimal(18,2)) as 平均分,
	
	cast((select count(1) from grade where grade.cid=course.cid and grade.score >=60)
		/(select count(1) from grade where grade.cid = course.cid) as decimal(18,2)) as 及格率,
		
	cast((select count(1) from grade where grade.cid=course.cid and grade.score >=70 and 		grade.score < 80)/(select count(1) from grade where grade.cid = course.cid) as 			decimal(18,2)) as 中等率,
	
	cast((select count(1) from grade where grade.cid=course.cid and grade.score >=80 and 		grade.score < 90)/(select count(1) from grade where grade.cid = course.cid) as 			decimal(18,2)) as 优良率,
	
	cast((select count(1) from grade where grade.cid=course.cid and grade.score >=90)
		/(select count(1) from grade where grade.cid = course.cid)as decimal(18,2)) as 优秀率
from grade,course
where grade.cid = course.cid
group by course.cid
```

<!--重重重-->**19、按各科成绩进行排序，并显示排名**
	**19.1 、Score重复时保留名次空缺**

```sql
select grade1.cid, grade1.sid, grade1.score, count(grade2.score)+1 as rank
from grade grade1
left join grade grade2
on grade1.score<grade2.score and grade1.cid = grade2.cid
group by grade1.cid, grade1.sid
order by grade1.cid, rank ASC;
```

​	**19.2 、Score重复时合并名次**

```sql

```

 **20、查询学生的总成绩并进行排名**
	 **20.1 、查询学生的总成绩**

```sql
select student.sid,student.sname,ifnull(sum(grade.score),0) 总成绩     
from Student student 
left join Grade grade on student.SID = grade.SID 
group by student.SID
order by  总成绩  desc
```

 	**20.2 、查询学生的总成绩并进行排名**

​		**分总分重复时保留名次空缺**

```sql

```

​		**不保留名次空缺**

```sql

```

## Day 05

 **21、查询不同老师所教不同课程平均分从高到低显示** 

```sql

```

 **22、查询所有课程的成绩第2名到第3名的学生信息及该课程成绩**

 ```sql

 ```

 **23、统计各科成绩各分数段人数：课程编号,课程名称, 100-85 , 85-70 , 70-60 , 0-60 ** 
 	**23.1、横向显示**

```sql
select course.cid,course.cname,
	sum(case when score >= 85 then 1 else 0 end)  '85-100',
	sum(case when score >= 70 and score < 85 then 1 else 0 end) '70-85' ,
	sum(case when score >= 60 and score < 70 then 1 else 0 end)  '60-70' ,
	sum(case when score < 60 then 1 else 0 end)  '0-60' 
from grade,course
where grade.cid = course.CID
group by course.CID
```

​	 **23.2、纵向显示**

```sql
select course.cid,course.cname,
	(case when score >= 85 then '85-100'
    	when score >= 70 and score < 85 then '70-85'
    	when score >= 60 and score < 70 then '60-70'
    	else '0-60'
    end) 分数段,count(1) 人数
from grade,course
where grade.cid = course.cid
group by 分数段,course.cid
order by course.cid,分数段 asc
```

 **24、统计各科成绩各分数段人数：课程编号,课程名称, 100-85 , 85-70 , 70-60 , <60 及所占百分比** 
	 **24.1、横向显示**

```sql

```

 	**24.2、纵向显示**

```sql

```

**25、查询学生平均成绩及其名次**
	**25.1 、保留名次空缺**

```sql

```

​	**25.2、不保留名次空缺**

```sql

```

## Day 06

**26、查询各科成绩前三名的记录**

​	**26.1、分数重复时保留名次空缺**

```sql

```

​	**26.2、 分数重复时不保留名次空缺，合并名次**

```sql

```

**27、查询每门课程被选修的学生数**

 ```sql

 ```

**28、查询男生、女生人数**

```sql

```

**29、查询名字中含有"风"字的学生信息**

```sql

```

**30、查询同名同性学生名单，并统计同名人数**

```sql

```

## Day 07

**31、查询1990年出生的学生名单(注：Student表中Sage列的类型是datetime)**

```sql

```

**32、查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列**

```sql

```

**33、查询平均成绩大于等于85的所有学生的学号、姓名和平均成绩**

```sql

```

**34、查询课程名称为"数学"，且分数低于60的学生姓名和分数**

```sql

```

**35、查询所有学生的课程及分数情况**

```sql

```



## Day 08

**36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数**

 ```sql

 ```

**37、查询不及格的课程**

```sql

```

**38、查询课程编号为01且课程成绩在80分以上的学生的学号和姓名**

 ```sql

 ```

**39、求每门课程的学生人数**

```sql

```

**40、查询选修"张三"老师所授课程的学生中，成绩最高的学生信息及其成绩**
	**40.1 当最高分只有一个时**

```sql

```

​	**40.2 当最高分出现多个时**

```sql

```

## Day 09

41、查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩

```sql

```

42、查询每门功成绩最好的前两名

```sql

```

43、统计每门课程的学生选修人数（超过5人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列 
 select Course.CID , Course.Cname , count(*)  学生人数 
 from Course , Grade
 where Course.CID = Grade.CID
 group by Course.CID , Course.Cname
 having count(*) >= 5
 order by  学生人数  desc , Course.CID

--44、检索至少选修两门课程的学生学号
 select student.SID , student.Sname
 from student , Grade
 where student.SID = Grade.SID
 group by student.SID , student.Sname
 having count(1) >= 2
 order by student.SID

--45、查询选修了全部课程的学生信息
 --方法1 根据数量来完成
 select student.* from student where SID in
 (select SID from sc group by SID having count(1) = (select count(1) from course))
 --方法2 使用双重否定来完成
 select t.* from student t where t.SID not in
 (
  select distinct m.SID from
  (
   select SID , CID from student , course
  ) m where not exists (select 1 from sc n where n.SID = m.SID and n.CID = m.CID)
 )
 --方法3 使用双重否定来完成
 select t.* from student t where not exists(select 1 from
 (
  select distinct m.SID from
  (
   select SID , CID from student , course
  ) m where not exists (select 1 from sc n where n.SID = m.SID and n.CID = m.CID)
 ) k where k.SID = t.SID
 )

--46、查询各学生的年龄
 --46.1 只按照年份来算
 select * , datediff(yy , sage , getdate())  年龄  from student
 --46.2 按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一
 select * , case when right(convert(varchar(10),getdate(),120),5) < right(convert(varchar(10),sage,120),5) then datediff(yy , sage , getdate()) - 1 else datediff(yy , sage , getdate()) end  年龄  from student

--47、查询本周过生日的学生
 select * from student where datediff(week,datename(yy,getdate()) + right(convert(varchar(10),sage,120),6),getdate()) = 0

--48、查询下周过生日的学生
 select * from student where datediff(week,datename(yy,getdate()) + right(convert(varchar(10),sage,120),6),getdate()) = -1

--49、查询本月过生日的学生
 select * from student where datediff(mm,datename(yy,getdate()) + right(convert(varchar(10),sage,120),6),getdate()) = 0

--50、查询下月过生日的学生

