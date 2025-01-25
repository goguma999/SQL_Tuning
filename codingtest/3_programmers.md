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

10. 특정 물고기를 잡은 총 수 구하기
```
select count(*) as FISH_COUNT
from FISH_INFO I
left join FISH_NAME_INFO N
on I.FISH_TYPE = N.FISH_TYPE
where N.FISH_NAME in ('BASS','SNAPPER') ; 
```

11. 조건에 부합하는 중고거래 상태 조회하기
```
select BOARD_ID, WRITER_ID, TITLE, PRICE, 
    case STATUS
    when 'DONE' then '거래완료'
    when 'SALE' then '판매중'
    when 'RESERVED' then '예약중'
    end as STATUS
from USED_GOODS_BOARD
where date_format(CREATED_DATE,'%Y-%m-%d') = '2022-10-05'
order by 1 desc ; 
```

12. 노선별 평균 역 사이 거리 조회하기
```
select ROUTE
    , concat(round(sum(D_BETWEEN_DIST),1),'km') as TOTAL_DISTANCE
    , concat(round(avg(D_BETWEEN_DIST),2),'km') as AVERAGE_DISTANCE
from SUBWAY_DISTANCE 
group by 1
order by 1 desc ;
```

13. 업그레이드 된 아이템 구하기 🍄
```
select I.ITEM_ID, I.ITEM_NAME, I.RARITY 
from ITEM_INFO I
left join ITEM_TREE T
on I.ITEM_ID = T.ITEM_ID 
where T.PARENT_ITEM_ID in (select ITEM_ID
                          from ITEM_INFO
                          where RARITY = 'RARE') 
order by 1 desc ; 
```
```sql
SELECT ITEM_ID, ITEM_NAME, RARITY
FROM ITEM_INFO
WHERE ITEM_ID IN (SELECT DISTINCT B.ITEM_ID
                    FROM ITEM_INFO A
                    JOIN ITEM_TREE B
                    ON A.ITEM_ID = B.PARENT_ITEM_ID
                    WHERE A.RARITY = 'RARE')
ORDER BY ITEM_ID DESC
```

14. 분기별 분화된 대장균의 개체 수 구하기
```sql
select concat(QUARTER(DIFFERENTIATION_DATE),'Q') as QUARTER
       , count(1) as ECOLI_COUNT
from ECOLI_DATA
group by 1
order by 1 asc ; 
```

15. 재구매가 일어난 상품과 회원 리스트 구하기
```
select USER_ID, PRODUCT_ID
from ONLINE_SALE 
group by 1,2 
having count(1) >= 2
order by USER_ID asc, PRODUCT_ID desc ; 
```

16. 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기 🍄
'통풍시트', '열선시트', '가죽시트' 중 하나 이상의 옵션이 포함된 자동차가 자동차 종류 별로 몇 대인지 출력
```
select CAR_TYPE, count(CAR_ID) as CARS
from CAR_RENTAL_COMPANY_CAR
where OPTIONS regexp '통풍시트|열선시트|가죽시트'
group by CAR_TYPE
order by CAR_TYPE asc ; 
```

### REGEXP  
| 기호       | 설명                          | 예시                          |
|------------|-------------------------------|-------------------------------|
| .          | 임의의 한 문자와 일치         | `a.b` → acb, a_b             |
| ^          | 문자열의 시작을 의미          | `^hello` → hello world, hello hi |
| $          | 문자열의 끝을 의미            | `world$` → hello word, hi word |
| *          | 앞의 문자가 0번 이상 반복됨   | `ab*c` → ac, abc, abbc, abbbc |
| +          | 앞의 문자가 1번 이상 반복됨   | `ab+c` → abc, abbc, ac (x)   |
| ?          | 앞의 문자가 0번 또는 1번 나타남| `colou?r` → color, colour    |
| [abc]      | 'a', 'b', 'c' 중 하나와 일치  | `[abc]` → apple, banana, cherry |
| [a-z]      | 소문자 알파벳 중 하나와 일치  | `[a-z]`                      |
| [A-Z]      | 대문자 알파벳 중 하나와 일치  | `[A-Z]`                      |
| [0-9]      | 숫자 중 하나와 일치           | `[0-9]`                      |
| \|         | OR 연산자 역할                | `cat|dog` → i have a cat, dog is good |









