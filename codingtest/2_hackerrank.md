## HackerRank
### Basic
21. Weather Observation Station 5 🍄
Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). <br>
If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
```
```

22. Weather Observation Station 6
Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates. <br>
```
select distinct(CITY)
from STATION
where substring(CITY,1,1) in ('a','e','i','o','u') ;
```

23. Weather Observation Station 7
Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.
```
select distinct(CITY)
from STATION
where lower(CITY) like '%a'
    or lower(CITY) like '%e'
    or lower(CITY) like '%i'
    or lower(CITY) like '%o'
    or lower(CITY) like '%u' ;
```

24. Weather Observation Station 8
```
select distinct(CITY)
from STATION
where lower(substring(CITY,1,1)) in ('a','e','i','o','u')
  and lower(right(CITY,1)) in ('a','e','i','o','u') ;
```

25. Weather Observation Station 9
```
select distinct(CITY)
from STATION
where lower(substring(CITY,1,1)) not in ('a','e','i','o','u') ;
```

26. Weather Observation Station 10
```
select distinct(CITY) 
from STATION
where lower(right(CITY,1)) not in ('a','e','i','o','u') ;
```

27. Weather Observation Station 11
```
select distinct(CITY)
from STATION
where lower(left(CITY,1)) not in ('a','e','i','o','u') 
   or lower(right(CITY,1)) not in ('a','e','i','o','u') ;
```

28. Weather Observation Station 12
```
select distinct(CITY)
from STATION
where lower(left(CITY,1)) not in ('a','e','i','o','u')
  and lower(right(CITY,1)) not in ('a','e','i','o','u') ;
```

29. Higher Than 75 Marks
```
select Name
from STUDENTS
where MARKS > 75    
order by right(Name,3), ID ;
```

30. Average Population of Each Continent
```
select B.CONTINENT, floor(avg(A.population))
from CITY A 
left join COUNTRY B
on A.COUNTRYCODE = B.CODE
where B.Continent is not null
group by B.Continent ;
```

31. Population Census
```
select sum(A.population)
from CITY A
left join COUNTRY B 
on A.COUNTRYCODE = B.CODE
where B.continent = 'Asia' ;
```

32. Weather Observation Station 15
```
select round(long_w,4)
from station 
where lat_n < 137.2345
order by lat_n desc 
limit 1 ;
```

33. Type of Triangle
```
Select case when A=B and B=C and C=A then 'Equilateral'    
    when A+B <= C or A+C <= B or B+C <= A then 'Not A Triangle' 
    when A<>B and A<>C and B<>C then 'Scalene'
    else 'Isosceles' 
    end 
from TRIANGLES ;
```

34. Weather Observation Station 19
```
select round(sqrt( power(max(lat_n)-min(lat_n),2) 
            + power(max(long_w)-min(long_w),2) 
           ),4)
from station ;
```

35. Top Earners
```
select salary*months, count(*) 
from Employee
group by salary*months
order by 1 desc 
limit 1 ;
```

### Intermediate ~ Advanced 
36. Ollivander's Inventory 🍄 <br>
-- Power, Age 고려하면서 coins_needed를 최소화한 것 고르기 
```
Select A.id, B.age, A.coins_needed, A.power 
from Wands A
left join Wands_Property B 
on A.code = B.code
where B.is_evil = 0
and A.coins_needed = (select min(C.coins_needed)
                     from Wands C
                     left join Wands_Property D 
                     on C.code = D.code
                     where D.is_evil = 0
                     and C.power = A.power
                     and D.age = B.age)
order by A.power desc, B.age desc ;
```

37. Contest Leaderboard
-- Hackers들의 challenge_id 별 score 중 최고점들 합해서 정렬
```
select hacker_id, name, sum(max_score)
from(
	select S.hacker_id as hacker_id, H.name as name, S.challenge_id as challenge_id, max(S.score) as max_score
	from Submissions S
	left join Hackers H
	on S.hacker_id = H.hacker_id
	group by 1,2,3
	) foo
group by 1,2
having sum(max_score) != 0 
order by 3 desc, 1 asc ;
```

38. Placements
-- 3개 테이블 조인하기
```
select S.name
from Students S 
left join Packages P1 on S.ID = P1.ID
left join Friends F on S.ID = F.ID 
left join Packages P2 on F.Friend_ID = P2.ID
where P1.Salary < P2.Salary 
order by P2.Salary ;
```

39. The Report
```
select Case when 
    G.Grade < 8 then Null
    else S.Name
    end as Name
    , G.Grade
    , S.Marks
from Students S 
left join Grades G
on S.Marks between G.Min_Mark and G.Max_Mark
order by 2 desc, 1 asc ;
```

40. Weather Observation Station 5 🍄
-- 이름이 가장 짧은 도시, 가장 긴 도시 한 개씩만 출력하기 알파벳 순으로
```
select CITY, length(CITY)
from (
select CITY, length(CITY), rank() over (partition by length(CITY) order by CITY asc) as ord
from STATION
where length(CITY) = (select min(length(CITY))
                     from STATION) 
    or length(CITY) = (select max(length(CITY))
                      from STATION)
    ) foo
where ord = 1 ;

(select CITY, length(CITY)   
from STATION
where length(CITY) = (select max(length(CITY))
                     from STATION)
order by 1 limit 1)
union
(select CITY, length(CITY)
from STATION
where length(CITY) = (select min(length(CITY))
                     from STATION)
order by 1 limit 1) ;
# 괄호로 묶어서 UNION
```

41. The PADS
```
select concat(name,"(",left(Occupation,1),")")
from Occupations
order by Name asc ; 
select concat("There are a total of ", occupation_cnt, " ",lower(occupation),"s.") 
from (select count(*) as occupation_cnt, occupation
      from OCCUPATIONS
      group by 2 
      order by occupation_cnt, occupation) foo ;
```

42. Occupations 🍄
-- pivot. 직업별 컬럼 만들고 이름 정렬
```
```
