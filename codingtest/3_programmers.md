# Programmers
## Lv. 1~2 
1. 자동차 대여 기록에서 장기/단기 대여 구분하기
```
```

2. 특정 형질을 가지는 대장균 찾기
```
```

3. 가격대 별 상품 개수 구하기 🍄  
```
select case 
    when price >= 0 and price < 10000 then 0
    when price >= 10000 and price < 20000 then 10000
    when price >= 20000 and price < 30000 then 20000
    when price >= 30000 and price < 40000 then 30000
    when price >= 40000 and price < 50000 then 40000
    when price >= 50000 and price < 60000 then 50000
    when price >= 60000 and price < 70000 then 60000
    when price >= 70000 and price < 80000 then 70000
    when price >= 80000 and price < 90000 then 80000
    when price >= 90000 and price < 100000 then 90000
    when price >= 100000 and price < 110000 then 100000
    end as PRICE_GROUP, count(*) as PRODUCTS
from PRODUCT
group by 1 
order by 1 ;
```
```sql
select 10000 * (PRICE DIV 10000) AS PRICE_GROUP, count(1) as PRODUCTS 
from product 
group by 1 
order by 1 ;
```
```sql
select truncate(PRICE, -4) as PRICE_GROUP
       , count(*) as PRODUCTS
from PRODUCT
group by 1
order by 1 ;

select truncate(12345.67891,1) = 12345.6
select truncate(12345.67891,4) = 12345.6789
select truncate(12345.67891,-1) = 12340
select truncate(12345.67891,-2) = 12300
```


4. 상품 별 오프라인 매출 구하기
```
select P.PRODUCT_CODE, sum(P.PRICE*S.SALES_AMOUNT) as SALES
from OFFLINE_SALE S 
left join PRODUCT P
on S.PRODUCT_ID = P.PRODUCT_ID
group by 1
order by 2 desc, 1 asc ;
```

5. 진료과별 총 예약 횟수 출력하기
```
select MCDP_CD as 진료과코드, count(1) as 5월예약건수
from APPOINTMENT
where date_format(APNT_YMD,'%Y-%m') = '2022-05'
group by 1
order by 2, 1 asc ; 
```

6. 성분으로 구분한 아이스크림 총 주문량
```
select I.INGREDIENT_TYPE, sum(F.TOTAL_ORDER) as TOTAL_ORDER
from FIRST_HALF F
left join ICECREAM_INFO I 
on F.FLAVOR = I.FLAVOR 
group by 1 
order by 2 asc ;
```

7. 조건에 맞는 도서와 저자 리스트 출력하기
```
select B.BOOK_ID, A.AUTHOR_NAME,
        date_format(B.PUBLISHED_DATE,'%Y-%m-%d') as PURBLISHED_DATE
from BOOK B
left join AUTHOR A
on B.AUTHOR_ID = A.AUTHOR_ID
where B.CATEGORY = '경제'
order by 3 asc ; 
```

8. 자동차 평균 대여 기간 구하기 🍄
```
select CAR_ID
        , round(avg(datediff(END_DATE,START_DATE)+1),1) as AVERAGE_DURATION
from CAR_RENTAL_COMPANY_RENTAL_HISTORY
group by 1
having AVERAGE_DURATION >= 7
order by 2 desc, 1 desc ;
```

9. 조건에 맞는 사원 정보 조회하기
```
select G.SCORE, E.EMP_NO, E.EMP_NAME, E.POSITION, E.EMAIL
from HR_EMPLOYEES E
right join (
            select EMP_NO, sum(SCORE) as SCORE
            from HR_GRADE 
            where YEAR = 2022
            group by 1 
            order by 2 desc 
            limit 1 
            ) G
on E.EMP_NO = G.EMP_NO ;
```


