## （一） 数据库表的初始化
### 1. 数据库表的个数
已知有如下 4 张表：
1. 学生表：students (id, name, birth, gender)      --> (学号, 学生姓名, 出生年月, 性别)
2. 教师表：teachers (id, name)                     --> (教师号, 教师姓名)
3. 课程表：courses  (id, name, teacher_id)         --> (课程号, 课程名称, 教师号)
4. 成绩表：scores   (student_id, course_id, score) --> (学号, 课程号, 成绩)

### 2. 创建数据库表
创建这 4 张表并且插入相关的测试数据

#### （1） 学生表 创建的 SQL 语句
```sql
CREATE TABLE students (
    id     VARCHAR(20)  NOT NULL,
    name   VARCHAR(20)  NOT NULL,
    birth  DATE         NOT NULL,
    gender VARCHAR(20)  NOT NULL,
    PRIMARY KEY(id)
);
```

#### （2） 教师表 创建的 SQL 语句
```sql
CREATE TABLE teachers (
    id   VARCHAR(20)  NOT NULL,
    name VARCHAR(20)  NOT NULL,
    PRIMARY KEY(id)
);
```

#### （3） 课程表 创建的 SQL 语句
```sql
CREATE TABLE courses (
    id         VARCHAR(20) NOT NULL,
    name       VARCHAR(20) NOT NULL,
    teacher_id VARCHAR(20) NOT NULL,
    FOREIGN KEY(teacher_id) REFERENCES teachers(id),
    PRIMARY KEY(id)
);
```

#### （4） 成绩表 创建的 SQL 语句
```sql
CREATE TABLE scores (
    student_id VARCHAR(20) NOT NULL,
    course_id  VARCHAR(20) NOT NULL,
    score      FLOAT(3)    NOT NULL,
    FOREIGN KEY(student_id) REFERENCES students(id),
    FOREIGN KEY(course_id)  REFERENCES courses(id)
);
```


### 3. 插入测试数据
#### （1） 学生表测试数据 创建的 SQL 语句
```sql
insert into students values('01', '赵雷', '1990-01-01', '男');
insert into students values('02', '钱电', '1990-12-21', '男');
insert into students values('03', '孙风', '1990-05-20', '男');
insert into students values('04', '李云', '1990-08-06', '男');
insert into students values('05', '周梅', '1991-12-01', '女');
insert into students values('06', '吴兰', '1992-03-01', '女');
insert into students values('07', '郑竹', '1989-07-01', '女');
insert into students values('08', '王菊', '1990-01-20', '女');
```

#### （2） 教师表测试数据 创建的 SQL 语句
```sql
insert into teachers values('01', '张三');
insert into teachers values('02', '李四');
insert into teachers values('03', '王五');
```

#### （3） 课程表测试数据 创建的 SQL 语句
```sql
insert into courses values('01', '语文', '02');
insert into courses values('02', '数学', '01');
insert into courses values('03', '英语', '03');
```

#### （4） 成绩表测试数据 创建的 SQL 语句
```sql
insert into scores values('01', '01', 80);
insert into scores values('01', '02', 90);
insert into scores values('01', '03', 99);
insert into scores values('02', '01', 70);
insert into scores values('02', '02', 60);
insert into scores values('02', '03', 80);
insert into scores values('03', '01', 80);
insert into scores values('03', '02', 80);
insert into scores values('03', '03', 80);
insert into scores values('04', '01', 50);
insert into scores values('04', '02', 30);
insert into scores values('04', '03', 20);
insert into scores values('05', '01', 76);
insert into scores values('05', '02', 87);
insert into scores values('06', '01', 31);
insert into scores values('06', '03', 34);
insert into scores values('07', '02', 89);
insert into scores values('07', '03', 98);
```

## （二） 50 道练习题
### 1. 练习题的题目
[1. 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数](#id-1)

[1.1 查询同时存在" 01 "课程和" 02 "课程的情况](#id-1.1)

[1.2 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )](#id-1.2)

[1.3 查询不存在" 01 "课程但存在" 02 "课程的情况](#id-1.3)

[2. 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩](#id-2)

[3. 查询在 scores 表存在成绩的学生信息](#id-3)

[4. 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )](#id-4)

[4.1 查有成绩的学生信息](#id-4.1)

[5. 查询「李」姓老师的数量](#id-5)

[6. 查询学过「张三」老师授课的同学的信息](#id-6)

[7. 查询没有学全所有课程的同学的信息](#id-7)

[8. 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息](#id-8)

[9. 查询和" 01 "号的同学学习的课程完全相同的其他同学的信息](#id-9)

[10. 查询没学过"张三"老师讲授的任一门课程的学生姓名](#id-10)

[11. 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩](#id-11)

[12. 检索" 01 "课程分数小于 60，按分数降序排列的学生信息](#id-12)

[13. 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩](#id-13)

[14. 查询各科成绩最高分、最低分和平均分：以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90](#id-14)

[15. 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺](#id-15)

[15.1 按各科成绩进行排序，并显示排名， Score 重复时合并名次](#id-15.1)

[16.  查询学生的总成绩，并进行排名，总分重复时保留名次空缺](#id-16)

[16.1 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺](#id-16.1)

[17. 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比](#id-17)

[18. 查询各科成绩前三名的记录](#id-18)

[19. 查询每门课程被选修的学生数](#id-19)

[20. 查询出只选修两门课程的学生学号和姓名](#id-20)

[21. 查询男生、女生人数](#id-21)

[22. 查询名字中含有「风」字的学生信息](#id-22)

[23. 查询同名同性学生名单，并统计同名人数](#id-23)

[24. 查询 1990 年出生的学生名单](#id-24)

[25. 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列](#id-25)

[26. 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩](#id-26)

[27. 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数](#id-27)

[28. 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）](#id-28)

[29. 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数](#id-29)

[30. 查询不及格的课程](#id-30)

[31. 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名](#id-31)

[32. 求每门课程的学生人数](#id-32)

[33. 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩](#id-33)

[34. 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩](#id-34)

[35. 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩](#id-35)

[36. 查询每门课成绩最好的前两名](#id-36)

[37. 统计每门课程的学生选修人数（超过 5 人的课程才统计）](#id-37)

[38. 检索至少选修两门课程的学生学号](#id-38)

[39. 查询选修了全部课程的学生信息](#id-39)

[40. 查询各学生的年龄，只按年份来算](#id-40)

[41. 查询各学生的年龄，按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一](#id-41)

[42. 查询本周过生日的学生](#id-42)

[43. 查询下周过生日的学生](#id-43)

[44. 查询本月过生日的学生](#id-44)

[45. 查询下月过生日的学生](#id-45)


### 2. 练习题的答案
#### <span id="id-1">（1）</span> 查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数
```sql
select st.*, a.score AS a_score, b.score AS b_score
from students AS st, scores AS a, scores AS b
where st.id=a.student_id
and st.id=b.student_id
and a.course_id='01'
and b.course_id='02'
and a.score>b.score;
```

#### <span id="id-1.1">（1.1）</span> 查询同时存在" 01 "课程和" 02 "课程的情况
```sql
select st.*, a.score AS a_score, a.course_id, b.score AS b_score, b.course_id
from students AS st, scores AS a, scores AS b
where st.id=a.student_id
and st.id=b.student_id
and a.course_id='01'
and b.course_id='02';
```
或者：
```sql
select st.*, a.score AS a_score, a.course_id, b.score AS b_score, b.course_id
from students AS st
JOIN scores as a ON st.id=a.student_id and a.course_id='01'
JOIN scores as b ON st.id=b.student_id and b.course_id='02';
```

#### <span id="id-1.2">（1.2）</span> 查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null )
```sql
select st.*, a.score AS a_score, a.course_id, b.score AS b_score, b.course_id
from students AS st
JOIN scores as a ON st.id=a.student_id and a.course_id='01'
LEFT JOIN scores as b ON st.id=b.student_id and b.course_id='02';
```

#### <span id="id-1.3">（1.3）</span> 查询不存在" 01 "课程但存在" 02 "课程的情况
```sql
select st.*, a.score, a.course_id
from students AS st, scores AS a
where st.id=a.student_id
and st.id not in (select student_id from scores where scores.course_id='01')
and a.course_id='02';
```

#### <span id="id-2">（2）</span> 查询平均成绩大于等于 60 分的同学的学生编号和学生姓名和平均成绩
```sql
select st.*, a.avg_score
from students AS st, (SELECT scores.student_id, ROUND(AVG(score)) as avg_score FROM scores GROUP BY student_id) AS a
where st.id=a.student_id and a.avg_score>=60;
```

#### <span id="id-3">（3）</span> 查询在 scores 表存在成绩的学生信息
```sql
select st.*
from students AS st
where st.id in (select distinct student_id from scores);
```

#### <span id="id-4">（4）</span> 查询所有同学的学生编号、学生姓名、选课总数、所有课程的总成绩(没成绩的显示为 null )
```sql
select st.*, b.coursecount, b.sumscore
from students AS st
LEFT JOIN (select a.student_id, COUNT(a.course_id) as coursecount, SUM(a.score) as sumscore
            from scores AS a group by a.student_id) AS b ON st.id=b.student_id
order by st.id;
```

#### <span id="id-4.1">（4.1）</span> 查有成绩的学生信息
好像跟上面的第三题是一样的
```sql
select st.*
from students AS st
where EXISTS (select a.student_id from scores AS a where a.student_id=st.id);
```

#### <span id="id-5">（5）</span> 查询「李」姓老师的数量
```sql
select COUNT(*) AS li_count from teachers where name LIKE '李%';
```

#### <span id="id-6">（6）</span> 查询学过「张三」老师授课的同学的信息
```sql
select st.* from students AS st, scores AS a
where st.id=a.student_id
and a.course_id in (select id from courses where teacher_id in (select id from teachers where name='张三'));
```

#### <span id="id-7">（7）</span> 查询没有学全所有课程的同学的信息
```sql
select st.* from students as st
where st.id not in (select student_id from scores group by scores.student_id
having count(scores.course_id)=(select count(id) from courses));
```

#### <span id="id-8">（8）</span> 查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息
```sql
select * from Students
where id in (select distinct student_id from scores where student_id<>'01'
and course_id in (select course_id from scores where student_id='01'));
```

#### <span id="id-9">（9）</span> 查询和" 01 "号的同学学习的课程完全相同的其他同学的信息
以下两个写法是网上看别人写的，自己水平还是不够啊
（思路：
    计算被查询者的课程总数，找出相同总数课程的人；
    筛选出被查询者没学过的课程，过滤掉学过该课程的人）
```sql
select * from students as st
where st.id in (select student_id from scores       -- 获取与被查询者具有相同课程数的同学
                    group by student_id having count(course_id) = (
                        select count(course_id) from scores
                            where student_id='01') )
    and st.id not in (select student_id from scores where course_id in  -- 获取被查询者未曾学过的课程，并过滤掉学过该课程的人
                        (select id from courses where id not in
                            (select course_id from scores where student_id='01')
                        )
                        )
    and st.id != '01';
```

第二种写法（思路：将每个人的课程使用 group_concat 函数串成字符串然后再比较）
```sql
SELECT t3.* FROM (select student_id, group_concat(course_id ORDER BY course_id) as group1
                    FROM scores WHERE student_id <> '01' GROUP BY student_id) as t1
INNER JOIN (SELECT group_concat(course_id ORDER BY course_id) as group2
                FROM scores WHERE student_id = '01' GROUP BY student_id) as t2 ON t1.group1 = t2.group2
INNER JOIN students t3 ON t1.student_id = t3.id;
```

#### <span id="id-10">（10）</span> 查询没学过"张三"老师讲授的任一门课程的学生姓名
```sql
select st.name from students AS st
where st.id not in (select student_id from scores
            where course_id=(select id from courses where teacher_id in (select id from teachers where name='张三')));
```

#### <span id="id-11">（11）</span> 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩
```sql
select st.*, AVG(a.score) from students as st
left join scores as a on a.student_id=st.id
where st.id in
    (select b.student_id from (select * from scores where score<60) AS b group by b.student_id having count(b.student_id)>=2)
group by st.id;
```

#### <span id="id-12">（12）</span> 检索" 01 "课程分数小于 60，按分数降序排列的学生信息
```sql
select st.*, a.score from students AS st, (select * from scores where course_id='01') AS a
where st.id=a.student_id and a.score<60
order by a.score desc;
```

#### <span id="id-13">（13）</span> 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩
(以下写法都不严谨，因为默认给定了 01、02、03，而不是从 courses 表中查出来的)
```sql
select st.*,
sum(case when a.course_id='01' then a.score else null end) as 语文,
sum(case when a.course_id='02' then a.score else null end) as 数学,
sum(case when a.course_id='03' then a.score else null end) as 英语,
ROUND(AVG(a.score), 2) as avg_score
from students as st
left join scores as a on st.id=a.student_id
group by st.id
order by avg_score desc;
```

网上看到别人的一个写法
```sql
select a.student_id,
        (select score from scores where student_id=a.student_id and course_id='01') as 语文,
        (select score from scores where student_id=a.student_id and course_id='02') as 数学,
        (select score from scores where student_id=a.student_id and course_id='03') as 英语,
        round(avg(score), 2) as 平均分
from scores as a
GROUP BY a.student_id
ORDER BY 平均分 DESC;
```

#### <span id="id-14">（14）</span> 查询各科成绩最高分、最低分和平均分：
以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率
及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90
```sql
select a.course_id, co.name, max(a.score) as 最大值, min(a.score) as 最小值, count(a.student_id) as 人数, round(avg(a.score), 2) as 平均分,
CONCAT(ROUND(100*(sum(case when a.score >= 60 then 1 else 0 end) / count(a.student_id)), 2), '%') as 及格率,
CONCAT(ROUND(100*(sum(case when a.score >= 70 and a.score < 80 then 1 else 0 end) / count(a.student_id)), 2), '%') as 中等率,
CONCAT(ROUND(100*(sum(case when a.score >= 80 and a.score < 90 then 1 else 0 end) / count(a.student_id)), 2), '%') as 优良率,
CONCAT(ROUND(100*(sum(case when a.score >= 90 then 1 else 0 end) / count(a.student_id)), 2), '%') as 优秀率
from scores as a
left join courses as co on co.id=a.course_id
group by course_id;
```

#### <span id="id-15">（15）</span> 按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺
#### <span id="id-15.1">（15.1）</span> 按各科成绩进行排序，并显示排名， Score 重复时合并名次
```sql
select a.student_id,
    @i:=(case when @course_id=a.course_id then @i+1 else 1 end) as i保留排名,
    @k:=(case when @score=a.score then @k else @i end) as rank不保留排名,
    @course_id:=a.course_id as course_id,
    @score:=a.score as 分数
from (select b.student_id, b.course_id, b.score from scores as b order by b.course_id, b.score desc) as a,
(select @i:=0,@k:=0,@score:=0,@course_id:=0) as s;
```

#### <span id="id-16">（16）</span> 查询学生的总成绩，并进行排名，总分重复时保留名次空缺
#### <span id="id-16.1">（16.1）</span> 查询学生的总成绩，并进行排名，总分重复时不保留名次空缺
```sql
select a.student_id,
@i:=@i+1 as 保留排名,
@k:=(case when @sumscore=a.sum_score then @k else @i end) as 不保留排名,
@sumscore:=a.sum_score as 总分数
from (select b.student_id, sum(b.score) as sum_score from scores as b group by b.student_id order by sum_score desc) as a,
(select @i:=0,@k:=0,@sumscore:=0) as varia;
```

以下展示一种错误的写法，执行顺序：先提取出列数据，然后再进行排名
```sql
select a.student_id,
@sumscore:=sum(a.score) as 总分数,
@i:=@i+1 as 保留排名,
@k:=(case when sum(a.score)=@sumscore then @k else @i end) as 不保留排名
from scores as a, (select @i:=0,@k:=0,@sumscore:=0) as b
group by a.student_id
order by sum(a.score) desc;
```

#### <span id="id-17">（17）</span> 统计各科成绩各分数段人数：课程编号，课程名称，[100-85]，[85-70]，[70-60]，[60-0] 及所占百分比
```sql
select a.course_id, co.name,
sum(case when a.score >= 85 then 1 else 0 end) as '[100-85]人数',
CONCAT(round(100*(sum(case when a.score >= 85 then 1 else 0 end)/count(a.student_id)), 2), '%') as '[100-85]百分比',
sum(case when a.score >= 70 and a.score < 85 then 1 else 0 end) as '[85-70]人数',
CONCAT(round(100*(sum(case when a.score >= 70 and a.score < 85 then 1 else 0 end)/count(a.student_id)), 2), '%') as '[85-70]百分比',
sum(case when a.score >= 60 and a.score < 70 then 1 else 0 end) as '[70-60]人数',
CONCAT(round(100*(sum(case when a.score >= 60 and a.score < 70 then 1 else 0 end)/count(a.student_id)), 2), '%') as '[70-60]百分比',
sum(case when a.score < 60 then 1 else 0 end) as '[60-0]人数',
CONCAT(round(100*(sum(case when a.score < 60 then 1 else 0 end)/count(a.student_id)), 2), '%') as '[60-0]百分比'
from scores as a
left join courses as co on a.course_id=co.id
group by a.course_id;
```

#### <span id="id-18">（18）</span> 查询各科成绩前三名的记录
```sql
select a.* from scores as a
where (select count(distinct b.score) from scores as b where b.course_id=a.course_id and b.score > a.score) < 3
order by course_id, score desc;
```

网上的一个案例
```sql
select a.*
from scores as a
left join scores as b on a.course_id=b.course_id and a.score < b.score
group by a.student_id, a.course_id, a.score
HAVING COUNT(distinct b.student_id)<3
ORDER BY a.course_id, a.score DESC;
```

第三种解法：（不推荐   如果第一名为多个人并列的分数，会存在没有取完的情况）
```sql
(
    select * from scores
    where course_id='01'
    order by score desc
    LIMIT 3
)
UNION ALL
(
    select * from scores
    where course_id='02'
    order by score desc
    LIMIT 3
)
UNION ALL
(
    select * from scores
    where course_id='03'
    order by score desc
    LIMIT 3
);
```

#### <span id="id-19">（19）</span> 查询每门课程被选修的学生数
```sql
select b.course_id, a.name, count(b.student_id) as 人数
from courses as a, scores as b
where a.id=b.course_id
group by b.course_id;
```

#### <span id="id-20">（20）</span> 查询出只选修两门课程的学生学号和姓名
```sql
select id, name from students
where id in (select student_id from scores as a group by a.student_id having count(a.course_id)=2);
```

#### <span id="id-21">（21）</span> 查询男生、女生人数
```sql
select gender, count(gender) as gender_count from students group by gender;
```

#### <span id="id-22">（22）</span> 查询名字中含有「风」字的学生信息
```sql
select * from students where name like '%风%';
```

#### <span id="id-23">（23）</span> 查询同名同性学生名单，并统计同名人数
思路（同名同性的人数合计肯定大于等于 2，根据他们的学号是唯一的来判断）
```sql
select name, gender, count(id) as num
from students
group by name, gender
having count(id)>=2;
```

#### <span id="id-24">（24）</span> 查询 1990 年出生的学生名单
```sql
select name from students where birth like '%1990%';
```

#### <span id="id-25">（25）</span> 查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
```sql
select course_id, AVG(score) as avg_score
from scores
group by course_id
order by avg_score desc, course_id;
```

#### <span id="id-26">（26）</span> 查询平均成绩大于等于 85 的所有学生的学号、姓名和平均成绩
```sql
select st.id, st.name, ROUND(avg(a.score), 2) as avg_score
from students as st
left join scores as a on st.id=a.student_id
group by st.id
having avg_score>=85;
```

#### <span id="id-27">（27）</span> 查询课程名称为「数学」，且分数低于 60 的学生姓名和分数
```sql
select st.name, a.score from students as st
join scores as a on a.student_id=st.id
where a.course_id=(select id from courses where name='数学') and a.score<60;
```

#### <span id="id-28">（28）</span> 查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）
```sql
select st.*,
sum(case when a.course_id='01' then a.score else null end) as 语文,
sum(case when a.course_id='02' then a.score else null end) as 数学,
sum(case when a.course_id='03' then a.score else null end) as 英语
from students as st
left join scores as a on a.student_id=st.id
group by st.id;
```

#### <span id="id-29">（29）</span> 查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数
```sql
select co.name, st.name, a.score
from students as st, courses as co
join scores as a on a.course_id=co.id
where st.id=a.student_id and a.score>=70;
```

解法二:
```sql
select co.name as 科目, st.name, a.score
from courses as co
left join scores as a on a.course_id=co.id
left join students as st on st.id=a.student_id
where a.score>=70;
```

#### <span id="id-30">（30）</span> 查询不及格的课程
```sql
select a.student_id, co.name as 科目, a.course_id, a.score as 不及格分数
from scores as a
left join courses as co on a.course_id=co.id
where a.score<60;
```

#### <span id="id-31">（31）</span> 查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名
```sql
select st.id, st.name, a.course_id, a.score
from students as st
left join scores as a on a.student_id=st.id and a.course_id='01'
where a.score>=80;
```

#### <span id="id-32">（32）</span> 求每门课程的学生人数
```sql
select a.course_id, co.name, count(a.student_id) as 人数
from scores as a
left join courses as co on co.id=a.course_id
group by a.course_id;
```

#### <span id="id-33">（33）</span> 成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
```sql
select st.*, a.score from students as st
left join scores as a on a.student_id=st.id
where a.course_id=
    (select co.id from courses as co where co.teacher_id=
        (select id from teachers where name='张三')
    )
order by a.score desc
limit 1;
```

#### <span id="id-34">（34）</span> 成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩
思路（存在多名并列第一的情况，因此提取出最高分的分数）
```sql
select st.*, a.score
from students as st
left join scores as a on a.student_id=st.id
where a.score=
    (select MAX(score) from scores as a where a.course_id=
        (select co.id from courses as co where co.teacher_id=
            (select id from teachers where name='张三')
        )
    );
```

#### <span id="id-35">（35）</span> 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
```sql
select distinct b.* from scores as a, scores as b
where a.course_id!=b.course_id and a.score=b.score;
```

审错题，然后写了个相同课程且成绩相同的学生的学生信息
```sql
    select st.*, b.course_id, b.score from students as st, (select score, course_id, count(student_id) from scores
            group by course_id, score
            having count(student_id)>=2) as a
    left join scores as b on b.course_id=a.course_id and b.score=a.score
    where st.id=b.student_id
    order by b.course_id;
```

#### <span id="id-36">（36）</span> 查询每门课成绩最好的前两名
```sql
select *
from scores as a
where (select count(distinct b.score) from scores as b where b.course_id=a.course_id and b.score>a.score) < 2
order by a.course_id, a.score desc;
```

#### <span id="id-37">（37）</span> 统计每门课程的学生选修人数（超过 5 人的课程才统计）
```sql
select course_id, count(course_id) as 选修人数
from scores
group by course_id
having count(course_id)>5;
```

#### <span id="id-38">（38）</span> 检索至少选修两门课程的学生学号
```sql
select student_id from scores
group by student_id
having count(course_id)>=2;
```

#### <span id="id-39">（39）</span> 查询选修了全部课程的学生信息
```sql
select st.* from students as st
left join scores as a on a.student_id=st.id
group by st.id
having count(a.course_id)=(select count(id) from courses);
```

#### <span id="id-40">（40）</span> 查询各学生的年龄，只按年份来算
```sql
select *, year(now()) - year(birth) as 年龄
from students;
```

#### <span id="id-41">（41）</span> 查询各学生的年龄，按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一
```sql
select st.*, YEAR(now()) - YEAR(birth) - (case when date_format(birth, '%m%d') < date_format(now(), '%m%d') then 0 else 1 end) as 年龄
from students as st;
```

#### <span id="id-42">（42）</span> 查询本周过生日的学生
```sql
select * from students
having WEEK(birth)=WEEK(now());
```

#### <span id="id-43">（43）</span> 查询下周过生日的学生
(可能是跨越年份的下周)
```sql
select * from students as st
where week(st.birth)=week(date_add(now(), interval 7 day));
```

#### <span id="id-44">（44）</span> 查询本月过生日的学生
```sql
select * from students
having MONTH(birth)=MONTH(now());
```

#### <span id="id-45">（45）</span> 查询下月过生日的学生
(可能是跨越年份的下月)
```sql
select * from students as st
where month(st.birth)=month(date_add(now(), interval 1 month));
```