## [패스트캠퍼스] K-Digital Credit SQL로 시작하는 데이터 분석 첫걸음
- Day 13. 서비스 이해 기본
```
-- 1. 2020년 7월의 총 Revenue
select sum(price)
from tbl_purchase
where substring(purchased_at, 1,7) = '2020-07';   # 21060206300

select sum(price)
from tbl_purchase
where purchased_at >= '2020-07-01'
  and purchased_at < '2020-08-01' ;   # 21060206300
```

```
-- 2. 2020년 7월의 총 MAU 
-- Monthly Active Users: 30일동안 앱을 사용하는 순수 유저 수 
select count(distinct customer_id) 
from tbl_visit 
where substring(visited_at,1,7) = '2020-07';  # 16414
```

```
-- 3. 7월에 우리 Active 유저의 구매율(Paying Rate)은 ? 
-- 7월 구매 유저 수 / 전체 활성 유저
select count(distinct customer_id) from tbl_purchase 
where substring(purchased_at,1,7) = '2020-07';   # 11174

select count(distinct customer_id) from tbl_visit 
where substring(visited_at,1,7) = '2020-07';   # 16414 

select round(11174/16414*100,2);   # 68.08    # 그냥 숫자 긁어서 나누기 함 
```

```sql 
-- 4. 7월에 구매 유저의 월 평균 구매금액은 ? 
-- ARPPU = Average Revenue per Paying User
# 1.유져별 7월에 한 번 구매할 때 평균 금액 
select customer_id, avg(price)
from tbl_purchase
where substring(purchased_at,1,7)='2020-07'
group by customer_id ;    

# 2.유져별 7월에 구매한 총 금액
select customer_id, sum(price)
from tbl_purchase
where substring(purchased_at,1,7)='2020-07'
group by customer_id ; 

# 3.유져별 7월에 구매한 총 금액의 평균 = 2번의 평균 
select avg(revenue)
from ( select customer_id, sum(price) as revenue
	from tbl_purchase
	where substring(purchased_at,1,7)='2020-07'
	group by customer_id ) A ;   # 1884750.8770
```

```sql
-- 5. 7월에 가장 많이 구매한 고객 Top3와 Top10~15 고객 ?
# rank+서브쿼리 사용
select customer_id, sum_price, rnk
from (
	select customer_id, sum(price) as sum_price , rank() over (order by sum(price) desc) as rnk
	from tbl_purchase 
	where substring(purchased_at,1,7)='2020-07'
	group by customer_id 
    ) A
where rnk between 1 and 3 ;

# offset 사용
select customer_id, sum(price) as revenue
from tbl_purchase
where purchased_at >= '2020-07-01'
  and purchased_at < '2020-08-01'
group by 1 
order by 2 desc
limit 5
offset 10;  # 앞에 10개를 건너뛰고 11번부터 보여줌 
```

- Day 14. 날짜·시간별 분석
```sql
-- 6. 2020년 7월의 평균 DAU를 구해주세요. Active User 수가 증가하는 추세인가요 ? 
# date_format 변경 확인
SELECT *, date_format(visited_at, '%Y-%m-%d %T') as date_at
from tbl_visit 
WHERE visited_at >= '2020-07-01'
  and visited_at < '2020-08-01' ;  
# 확인해보니까 date_at 에 9시간 추가됨 
# visited_at 컬럼의 값이 저장된 시간대와 쿼리를 실행하는 MySQL 서버나 클라이언트의 시간대 설정이 달라서. 

# 세션 시간대 설정
SET time_zone = '+00:00';
    
# 평균 DAU 
SELECT avg(DAU) 
FROM (
	SELECT date_format(visited_at, '%Y-%m-%d') as date_at, count(distinct customer_id) as DAU
	from tbl_visit 
	WHERE visited_at >= '2020-07-01'
	  and visited_at < '2020-08-01' 
	GROUP BY 1
	ORDER BY 1 
    ) A ;
```
```
-- 7. 2020년 7월의 평균 WAU를 구해주세요
SELECT avg(users) as 평균WAU
FROM (
	SELECT date_format(visited_at, '%Y-%m-%U') as week, count(distinct customer_id) as users
	FROM tbl_visit
	WHERE visited_at >= '2020-07-05'     # 그냥 달력에서 보고 온전한 주 선택
	  and visited_at < '2020-07-26'
	GROUP BY 1 
    ) foo ;
```
```
-- 8-1. 2020년 7월의 Daily Revenue는 증가하는 추세인가요 ? 평균 Daily Revenue도 구해주세요
SELECT avg(sum_price) as avg_DR
FROM (
	SELECT date_format(purchased_at, '%Y-%m-%d') as date_at, sum(price) as sum_price
	FROM tbl_purchase
	WHERE purchased_at >= '2020-07-01' 
	  and purchased_at < '2020-08-01'
	GROUP BY 1
	ORDER BY 1
    ) foo ;

-- 8-2. 2020년 7월의 평균 Weekly Revenue를 구해주세요
SELECT avg(sum_price) as avg_WR
FROM (
	SELECT date_format(purchased_at, '%Y-%m-%U') as date_at, sum(price) as sum_price
	FROM tbl_purchase
	WHERE purchased_at >= '2020-07-05'
	  and purchased_at < '2020-07-26'
	GROUP BY 1
	ORDER BY 1
    ) foo ;
```

```sql
-- 9. 2020년 7월 요일별 Daily Revenue를 구해주세요. 어느 요일이 Revenue가 가장 높고 낮나요 ?
# 각 날짜별 daily revenue 구함 -> 날짜의 요일로 그룹바이-평균 
SELECT date_format(date_at, '%a') as day_name, avg(revenue)
FROM ( 
	SELECT date_format(purchased_at, '%Y-%m-%d-%w') as date_at, sum(price) as revenue
	FROM tbl_purchase
	WHERE purchased_at >= '2020-07-01'
	  and purchased_at < '2020-08-01'
	GROUP BY 1 
	ORDER BY 1
    ) foo
GROUP BY 1 
ORDER BY 2 desc;
```

```sql
-- 10. 2020년 7월 시간대별 시간당 총 Revenue를 구해주세요. 어느 시간대가 Revenue가 가장 높고 낮나요 ? 
-- 평균을 구하려면 총합하는 과정이 필요함
# 시간대별로 개별 거래의 price의 평균
SELECT date_format(purchased_at, '%H') as date_time, avg(price) as revenue 
FROM tbl_purchase
WHERE purchased_at >= '2020-07-01'
  and purchased_at < '2020-08-01'
GROUP BY 1 ; 

# 날짜별 시간대 매출 합계의 평균 
SELECT hour_at, avg(revenue)   
FROM (
	SELECT date_format(purchased_at, '%Y-%m-%d') as date_at,  # 날짜별 합계
			date_format(purchased_at, '%H') as hour_at,  # 시간대별 합계도 뽑아야 하니까 시간 컬럼
			sum(price) as revenue
	FROM tbl_purchase
	WHERE purchased_at >= '2020-07-01'
	  and purchased_at < '2020-08-01'
	GROUP BY 1,2 
    ) foo 
GROUP BY 1
ORDER BY 2 desc;
```

```sql
-- 11. 2020년 7월의 요일 및 시간대별 Revenue를 구해주세요
SELECT week_at, hour_at, avg(revenue)
FROM (
	SELECT date_format(purchased_at, '%a') as week_at,
		date_format(purchased_at, '%H') as hour_at,
		sum(price) as revenue 
	FROM tbl_purchase
	WHERE purchased_at >= '2020-07-01'
	  and purchased_at < '2020-08-01' 
	GROUP BY 1, 2 
	) foo 
GROUP BY 1, 2 ;
```

- Day 15. 유저 세그먼트별 분석
```sql
-- 12. 전체 유저의 Demographic을 알고 싶어요. 성별 연령별로 유저 숫자를 알려주세요.
-- 어느 세그먼트가 가장 숫자가 많나요 ? 참고로 기타 성별은 하나로, 연령은 5세 단위로 적당히 묶어주시고 유저 수가 높은 순서대로 정렬
select case 
	when length(gender) < 1 then 'Others'       # gender is null이 안 먹힐 때 length 함수로 돌파 
  	else gender end as '성별'
    , case
	when age between 11 and 15 then '0_11-15'
	when age between 16 and 20 then '1_16-20'
	when age between 21 and 25 then '2_21-25'
	when age between 26 and 30 then '3_26-30'
	when age between 31 and 35 then '4_31-35'
	when age between 36 and 40 then '5_36-40'
	when age between 41 and 45 then '6_41-45'
	when age between 46 and 50 then '7_46-50'
	when age between 51 and 55 then '8_51-55'
	else '9_56-' end as "연령대"
    , count(*) as 유저수
from tbl_customer
group by 1, 2 
order by 3 desc ; 
```

```sql
-- 13. Q12 결과의 성,연령을 "남성(25~29세)"와 같이 통합해주시고, 각 성,연령이 전체 고객에서 얼마나 차지하는지 분포를 확인하기 
select concat(case 
		when length(gender) < 1 then '기타'
		when gender = 'F' then '여성'
		when gender = 'M' then '남성'
	    	when gender = 'Others' then '기타'
		end
        ,'('
	, case  
		when age between 11 and 15 then '11-15세'
		when age between 16 and 20 then '16-20세'
		when age between 21 and 25 then '21-25세'
		when age between 26 and 30 then '26-30세'
		when age between 31 and 35 then '31-35세'
		when age between 36 and 40 then '36-40세'
		when age between 41 and 45 then '41-45세'
		when age between 46 and 50 then '46-50세'
		when age between 51 and 55 then '51-55세'
		else '56세 이상' end 
        ,')') as 세그먼트
	, round(count(*)/ (select count(*) from tbl_customer)*100,2) as 비율
from tbl_customer
group by 1 ; 
```

```
-- 14. 2020년 7월, 성별에 따라 총 구매 건수와, 총 Revenue를 구해주세요. 이전처럼 남녀 이외의 성별은 하나로 묶기
select case 
	when length(C.gender) < 1 then '기타'
	when C.gender = 'F' then '여성'
	when C.gender = 'M' then '남성'
	else '기타' end as '성별', count(P.id) as 구매건수, sum(price) as revenue
from tbl_purchase P 
left join tbl_customer C 
on P.customer_id = C.customer_id
where P.purchased_at >= '2020-07-01'
  and P.purchased_at < '2020-08-01'
group by 1 ;
```

```
-- 15. 2020년 7월의 성별 연령대에 따라 구매 건수와, 총 Revenue를 구해주세요
select concat(case 
	when length(gender) < 1 then '기타'
	when gender = 'F' then '여성'
	when gender = 'M' then '남성'
	when gender = 'Others' then '기타'
	end
        ,'('
	, case  
	when age between 11 and 15 then '11-15세'
	when age between 16 and 20 then '16-20세'
	when age between 21 and 25 then '21-25세'
	when age between 26 and 30 then '26-30세'
	when age between 31 and 35 then '31-35세'
	when age between 36 and 40 then '36-40세'
	when age between 41 and 45 then '41-45세'
	when age between 46 and 50 then '46-50세'
	when age between 51 and 55 then '51-55세'
	else '56세 이상' end 
    	,')') as 세그먼트
    , count(P.id) as 구매건수
    , sum(P.price) as revenue
from tbl_purchase P
left join tbl_customer C
on P.customer_id = C.customer_id
where P.purchased_at >= '2020-07-01'
  and P.purchased_at < '2020-08-01'
group by 1 ;
```

```sql
-- 16. 2020년 7월 일별 매출의 전일 대비 증감폭, 증감률 구하기             # with, lag 
with tbl_revenue as (
select date_format(purchased_at, '%Y-%m-%d') as date_at
	, sum(price) as revenue
from tbl_purchase
where purchased_at >= '2020-07-01'
  and purchased_at < '2020-08-01'
group by 1
)

select *, 
    revenue - lag(revenue) over (order by date_at asc) as 증감폭,
    round((revenue - lag(revenue) over (order by date_at asc)) / lag(revenue) over (order by date_at asc) * 100, 2) as 증감률
from tbl_revenue ;
```

```sql
-- 17. 7월에 일별로 구매 금액 기준으로 가장 많이 지출한 고객 Top3           # partition 
select *             
from (
	select date_format(purchased_at, '%Y-%m-%d') as date_at, customer_id, sum(price),
		dense_rank() over (partition by date_format(purchased_at, '%Y-%m-%d') order by sum(price) desc) as rnk
	from tbl_purchase
	where purchased_at >= '2020-07-01'
	  and purchased_at < '2020-08-01'     
	group by 1, 2
    ) foo
where rnk <= 3; 
```

- Day 16. 프로덕트 분석 심화
```sql
-- 18. 2020년 7월에 우리 신규유저가 하루 안에 결제로 넘어가는 비율이 어떻게 되나요 ? 
-- 그 비율이 어떤지 알고 싶고, 결제까지 보통 몇 분 정도가 소요되는지 알고 싶어요      # timediff, interval
with rt_tbl as (
		select C.customer_id
			, C.created_at
			, P.customer_id as paying_user
			, P.purchased_at
			, time_to_sec(timediff(P.purchased_at, C.created_at))/3600 as diff_hour
		from tbl_customer C
		left join ( select customer_id, min(purchased_at) as purchased_at 
			    from tbl_purchase
			    group by 1 ) P 
		on C.customer_id = P.customer_id 
		and P.purchased_at < C.created_at + interval 1 day   # 계정 생성 후 하루 이내 구매한 사람
		where C.created_at >= '2020-07-01'
		  and C.created_at < '2020-08-01' 
		)

select round(count(paying_user)/count(customer_id)*100,2)
from rt_tbl ;

select avg(diff_hour)
from rt_tbl;
```

```sql
-- 19. 우리 서비스는 유저의 재방문율이 높은 서비스인가요? 이를 파악하기 위해 7월 기준 Day1 Retention이 어떤지 구하고 추세를 보기 위해 Daily로 추출해주세요.
-- Retention: 시간이 지날수록 얼마나 많은 유저가 제품이나 서비스로 다시 돌아오는지
-- N-day Retention: n=1,2,3,4...30..
select date_format(A.visited_at - interval 9 hour, '%Y-%m-%d') as d_date
	, count(distinct A.customer_id) as active_user
    , count(distinct B.customer_id) as retained_user
    , count(distinct B.customer_id) / count(distinct A.customer_id) as retention 
from tbl_visit A
left join tbl_visit B
on A.customer_id = B.customer_id
and date_format(A.visited_at - interval 9 hour, '%Y-%m-%d') = date_format(B.visited_at - interval 9 hour - interval 1 day, '%Y-%m-%d')
where A.visited_at >= '2020-07-01'
  and A.visited_at < '2020-08-01'
group by 1 ;
```

```sql
-- 20. 2020년 7월 우리 서비스는 신규유저가 많나요 ? 기존 유저가 많나요 ? 
-- 가입기간별로 고객 분포가 어떤지 알려주세요. DAU 기준으로 부탁합니다.
with tbl_visit_by_joined as ( 
	select date_format(A.visited_at - interval 9 hour, '%Y-%m-%d') as d_date
		, A.customer_id
		, B.created_at as d_joined
		, max(A.visited_at) as last_visit
		, datediff(max(A.visited_at), B.created_at) as date_diff
	from tbl_visit A
	left join tbl_customer B
	on A.customer_id = B.customer_id
	where A.visited_at >= '2020-07-01'
	  and A.visited_at < '2020-08-01'
	group by 1,2,3
)

select A. d_date
	, case when date_diff >= 730 then '2년 이상'
	   when date_diff >= 365 then '1년 이상'
           when date_diff >= 183 then '6개월 이상'
           when date_diff >= 91 then '3개월 이상'
           when date_diff >= 30 then '1개월 이상'
           else '1개월 미만'
           end as segment
	, B.all_users
	, count(A.customer_id) as users
        , round(count(A.customer_id)/B.all_users*100,2) as ratio
from tbl_visit_by_joined A
left join (select d_date
		, count(customer_id) all_users
	    from tbl_visit_by_joined
            group by 1 ) B 
on A.d_date = B.d_date
group by 1,2,3 
order by 1, 2 ;
```

## HackerRank
- Basic
```
-- 21. Weather Observation Station 5 🍄
-- Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

-- 22. Weather Observation Station 6
-- Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.
select distinct(CITY)
from STATION
where substring(CITY,1,1) in ('a','e','i','o','u') ;

-- 23. Weather Observation Station 7
-- Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.
select distinct(CITY)
from STATION
where lower(CITY) like '%a'
    or lower(CITY) like '%e'
    or lower(CITY) like '%i'
    or lower(CITY) like '%o'
    or lower(CITY) like '%u' ;

-- 24. Weather Observation Station 8
-- Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.
select distinct(CITY)
from STATION
where lower(substring(CITY,1,1)) in ('a','e','i','o','u')
  and lower(right(CITY,1)) in ('a','e','i','o','u') ;

-- 25. Weather Observation Station 9
-- Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.
select distinct(CITY)
from STATION
where lower(substring(CITY,1,1)) not in ('a','e','i','o','u') ;
```



