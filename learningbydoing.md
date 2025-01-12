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
	group by 1,2 
    ) foo 
GROUP BY 1
ORDER BY 2 desc;
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


