# sql练习



![image-20200816210542707](C:/Users/mao/AppData/Roaming/Typora/typora-user-images/image-20200816210542707.png)![image-20200816210626069](C:/Users/mao/AppData/Roaming/Typora/typora-user-images/image-20200816210626069.png)



查询每个科目成绩最高的学生名字和成绩





查询每个学生的总成绩和名字

```sql
SELECT stu.name, SUM(sc.grade) AS scores FROM student stu, score sc
WHERE stu.id = sc.stu_id GROUP BY sc.stu_id
```



查询计算机成绩前三名额学生名字

```sql
SELECT stu.name FROM student stu
WHERE stu.id IN(select sid.stu_id FROM (SELECT stu_id FROM score WHERE c_name='计算机'
ORDER BY grade desc LIMIT 3) AS sid)
```



查询计算机成绩低于95的学生名字

```sql
SELECT stu.name FROM student stu
WHERE stu.id IN(SELECT stu_id FROM score WHERE c_name = '计算机' AND grade<95)

```

