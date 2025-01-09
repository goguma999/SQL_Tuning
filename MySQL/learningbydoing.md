## [패스트캠퍼스] K-Digital Credit SQL로 시작하는 데이터 분석 첫걸음
- Day13 서비스 이해 기본
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
```




