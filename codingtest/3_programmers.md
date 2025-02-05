# Programmers
## Lv. 1~2 
1. 자동차 대여 기록에서 장기/단기 대여 구분하기
```
SELECT 
    HISTORY_ID
    , CAR_ID
    , date_format(START_DATE,'%Y-%m-%d') as START_DATE
    , date_format(END_DATE,'%Y-%m-%d') as END_DATE
    , CASE
    WHEN DATEDIFF(END_DATE, START_DATE) + 1 >= 30 THEN '장기 대여'
    ELSE '단기 대여'
    END AS RENT_TYPE 
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
WHERE START_DATE >= '2022-09-01' 
  AND START_DATE < '2022-10-01'
ORDER BY HISTORY_ID desc ; 
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


33. 연도별 대장균 크기의 편차 구하기 
```
select B.YEAR, (B.maxsize - A.SIZE_OF_COLONY) as YEAR_DEV, A.ID 
from ECOLI_DATA A
join (
        select YEAR(DIFFERENTIATION_DATE) as YEAR, 
                MAX(SIZE_OF_COLONY)as maxsize
        from ECOLI_DATA 
        group by 1 
    ) B
on YEAR(A.DIFFERENTIATION_DATE) = B.YEAR 
order by B.YEAR asc, YEAR_DEV asc ; 
```
```sql
SELECT
    YEAR(DIFFERENTIATION_DATE) AS YEAR
    , (MAX_SIZE - SIZE_OF_COLONY) AS YEAR_DEV
    , ID
FROM (
    SELECT 
        *
        , MAX(SIZE_OF_COLONY) OVER (PARTITION BY YEAR(DIFFERENTIATION_DATE)) AS MAX_SIZE
    FROM ECOLI_DATA
    ) AS M
ORDER BY YEAR, YEAR_DEV
```
42. 조건에 맞는 개발자 찾기 🍄
```sql
select ID, EMAIL, FIRST_NAME, LAST_NAME
from DEVELOPERS
where SKILL_CODE & (select sum(CODE)
                   from SKILLCODES
                   where NAME IN ('Python','C#')
                   )
order by ID ;

# 비트 연산자 & : 같은 비트 위치에서 1이 있는 부분만 남김 
```
43. 부모의 형질을 모두 가지는 대장균 찾기 🍄
```
select C.ID, 
    C.GENOTYPE as GENOTYPE, 
    P.GENOTYPE as PARENT_GENOTYPE
from ECOLI_DATA C
join ECOLI_DATA P
on C.PARENT_ID = P.ID 
where (C.GENOTYPE & P.GENOTYPE) = P.GENOTYPE 
order by C.ID asc ; 
```

## Lv. 3
17. 없어진 기록 찾기
```
select O.ANIMAL_ID, O.NAME
from ANIMAL_OUTS O
left join ANIMAL_INS I
on O.ANIMAL_ID = I.ANIMAL_ID 
where I.ANIMAL_ID is null 
order by 1 ;
```
18. 있었는데요 없었습니다
```
select I.ANIMAL_ID, I.NAME
from ANIMAL_INS I 
join ANIMAL_OUTS O 
on I.ANIMAL_ID = O.ANIMAL_ID
where I.DATETIME > O.DATETIME
order by I.DATETIME asc ; 
```
19. 오랜 기간 보호한 동물(1)
```
select I.NAME, I.DATETIME 
from ANIMAL_INS I
left join ANIMAL_OUTS O
on I.ANIMAL_ID = O.ANIMAL_ID 
where O.DATETIME is null 
order by I.DATETIME asc
limit 3 ;
```
20. 오랜 기간 보호한 동물(2)
```
select O.ANIMAL_ID, O.NAME
from ANIMAL_OUTS O
left join ANIMAL_INS I 
on O.ANIMAL_ID = I.ANIMAL_ID
order by O.DATETIME - I.DATETIME  desc 
limit 2 
```
21. 조건별로 분류하여 주문상태 출력하기
```
SELECT ORDER_ID, PRODUCT_ID, date_format(OUT_DATE,'%Y-%m-%d') as OUT_DATE, 
    CASE 
    WHEN OUT_DATE <= '2022-05-01' THEN '출고완료'
    WHEN OUT_DATE > '2022-05-01' THEN '출고대기'
    ELSE '출고미정' END as 출고여부
FROM FOOD_ORDER 
ORDER BY ORDER_ID asc ; 
```
22. 헤비 유저가 소유한 장소
```
SELECT ID, NAME, HOST_ID
FROM PLACES
WHERE HOST_ID IN (SELECT HOST_ID 
           FROM PLACES 
           GROUP BY HOST_ID
           HAVING COUNT(1) >= 2 ) 
ORDER BY ID ; 
```
23. 카테고리 별 도서 판매량 집계하기
```
SELECT B.CATEGORY, SUM(S.SALES) as TOTAL_SALES 
FROM BOOK_SALES S
JOIN BOOK B
ON S.BOOK_ID = B.BOOK_ID 
WHERE SALES_DATE like '2022-01%'
GROUP BY B.CATEGORY
ORDER BY CATEGORY asc ; 
```
24. 대여 기록이 존재하는 자동차 리스트 구하기
```
select distinct H.CAR_ID
from CAR_RENTAL_COMPANY_RENTAL_HISTORY H
join CAR_RENTAL_COMPANY_CAR C 
on H.CAR_ID = C.CAR_ID 
where C.CAR_TYPE = '세단'
  and ( H.START_DATE >= '2022-10-01' AND H.START_DATE < '2022-11-01' ) 
order by CAR_ID desc ; 
```
25. 조건에 맞는 사용자와 총 거래금액 조회하기
```
SELECT U.USER_ID, U.NICKNAME, SUM(PRICE) AS TOTAL_SALES
FROM USED_GOODS_BOARD B
JOIN USED_GOODS_USER U
ON B.WRITER_ID = U.USER_ID
WHERE B.STATUS = 'DONE'
GROUP BY 1, 2 
HAVING SUM(PRICE) >= 700000 
ORDER BY 3 asc ; 
```
26. 조건에 맞는 사용자 정보 조회하기
```
SELECT 
    USER_ID,
    NICKNAME,
    CONCAT(CITY,' ', STREET_ADDRESS1, ' ',STREET_ADDRESS2) AS 전체주소,
    CONCAT(left(TLNO,3),'-',substring(TLNO,4,4),'-',right(TLNO,4)) AS 전화번호
FROM USED_GOODS_USER
WHERE USER_ID IN ( SELECT distinct WRITER_ID
                    FROM USED_GOODS_BOARD B
                    GROUP BY WRITER_ID 
                    HAVING COUNT(*) >= 3  
                 )
ORDER BY USER_ID desc ; 
```
```sql
    CONCAT_WS(' ',CITY,STREET_ADDRESS1,STREET_ADDRESS2) AS 전체주소,
    CONCAT_WS('-',left(TLNO,3),substring(TLNO,4,4),right(TLNO,4)) AS 전화번호
```
27. 조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기 🍄
```
SELECT 
    CONCAT('/home/grep/src/',
          BOARD_ID,
          '/',
          FILE_ID,
          FILE_NAME,
          FILE_EXT) as FILE_PATH
FROM USED_GOODS_FILE 
WHERE BOARD_ID = (
                    SELECT BOARD_ID
                    FROM USED_GOODS_BOARD
                    WHERE VIEWS = (SELECT MAX(VIEWS)
                                    FROM USED_GOODS_BOARD 
                                   )
                )
ORDER BY FILE_ID desc ; 
```
28. 부서별 평균 연봉 조회하기
```
select E.DEPT_ID, D.DEPT_NAME_EN, round(avg(SAL)) as AVG_SAL
from HR_EMPLOYEES E
join HR_DEPARTMENT D
on E.DEPT_ID = D.DEPT_ID 
group by 1,2 
order by 3 desc ; 
```
29. 특정 조건을 만족하는 물고기별 수와 최대 길이 구하기
```
select count(*) as FISH_COUNT, 
        MAX(LENGTH) as MAX_LENGTH,
        FISH_TYPE
from FISH_INFO
group by FISH_TYPE 
having avg(ifnull(LENGTH,10)) >= 33
order by FISH_TYPE asc ;
```
30. 대장균들의 자식의 수 구하기 🍄 
```
select C.ID, count(P.PARENT_ID) as CHILD_COUNT  
from ECOLI_DATA C
left join ECOLI_DATA P
on C.ID = P.PARENT_ID
group by C.ID
order by C.ID asc ; 
```
31. 대장균의 크기에 따라 분류하기 1
```
select ID,
    case
    when SIZE_OF_COLONY <= 100 then 'LOW'
    when SIZE_OF_COLONY <= 1000 then 'MEDIUM'
    else 'HIGH' end as SIZE 
from ECOLI_DATA
order by ID asc ;
```
32. 대장균의 크기에 따라 분류하기 2 🍄
```sql
with qtl as (
    select ID, 
            ntile(4) over (order by SIZE_OF_COLONY) as quantile
    from ECOLI_DATA )

select ID,
    case quantile
    when 1 then 'LOW'
    when 2 then 'MEDIUM'
    when 3 then 'HIGH'
    when 4 then 'CRITICAL'
    end as COLONY_NAME
from qtl
order by ID asc ; 
```
```sql
SELECT 
    ID
    , (CASE 
        WHEN PER_RANK <= 0.25 THEN 'CRITICAL'
        WHEN PER_RANK <= 0.5 THEN 'HIGH'
        WHEN PER_RANK <= 0.75 THEN 'MEDIUM'
        ELSE 'LOW'
      END) AS COLONY_NAME
FROM (SELECT 
        ID, 
        PERCENT_RANK() OVER (ORDER BY SIZE_OF_COLONY DESC) AS PER_RANK
     FROM ECOLI_DATA
     ) AS D
ORDER BY ID
```
34. 즐겨찾기가 가장 많은 식당 정보 출력하기
```
-- 윈도우 함수 사용1 
select FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
from (
        select *, rank() over (partition by FOOD_TYPE order by FAVORITES desc ) as favrnk
        from REST_INFO 
    ) foo
where favrnk = 1 
order by FOOD_TYPE desc ;
```
```
-- 윈도우 함수 사용2 
select FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
from (
    select *, max(FAVORITES) over (partition by FOOD_TYPE) as maxfav
    from REST_INFO
    ) foo
where FAVORITES = maxfav
order by FOOD_TYPE desc ; 
```
```
-- 서브쿼리 사용 | ( 컬럼 n개 ) in 가능 ✔️
select FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
from REST_INFO
where (FOOD_TYPE, FAVORITES) in (
    select FOOD_TYPE, max(FAVORITES) 
    from REST_INFO 
    group by FOOD_TYPE 
    )
order by FOOD_TYPE desc ; 
```
35. 대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기
```sql
select MONTH(START_DATE) as MONTH, CAR_ID, count(*) as RECORDS
from CAR_RENTAL_COMPANY_RENTAL_HISTORY
where CAR_ID in (
                select CAR_ID
                from CAR_RENTAL_COMPANY_RENTAL_HISTORY
                where date_format(START_DATE,'%Y-%m-%d') >= '2022-08-01'
                  and date_format(START_DATE,'%Y-%m-%d') < '2022-11-01'
                group by CAR_ID
                having count(*) >= 5 
                )
  and date_format(START_DATE,'%Y-%m-%d') >= '2022-08-01'
  and date_format(START_DATE,'%Y-%m-%d') < '2022-11-01'
group by MONTH(START_DATE), CAR_ID
having RECORDS != 0 
order by MONTH asc, CAR_ID desc ;


BETWEEN AND 를 쓰자 . .
```
36. 업그레이드 할 수 없는 아이템 구하기
```
select I.ITEM_ID, I.ITEM_NAME, I.RARITY
from ITEM_INFO I
left join ITEM_TREE T
on I.ITEM_ID = T.PARENT_ITEM_ID
where T.PARENT_ITEM_ID is null 
order by I.ITEM_ID desc ; 
```
37. 물고기 종류 별 대어 찾기
```
select ID, FISH_NAME, LENGTH
from (
        select I.ID as ID, N.FISH_NAME as FISH_NAME, I.LENGTH as LENGTH, max(I.LENGTH) over (partition by I.FISH_TYPE) as maxlng
        from FISH_INFO I
        join FISH_NAME_INFO N 
        on I.FISH_TYPE = N.FISH_TYPE
    ) foo
where LENGTH = maxlng 
order by ID asc ;
```
## Lv. 4
38. 보호소에서 중성화한 동물
```
select I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME 
from ANIMAL_INS I
join ANIMAL_OUTS O
on I.ANIMAL_ID = O.ANIMAL_ID
where I.SEX_UPON_INTAKE like 'Intact%'
  and O.SEX_UPON_OUTCOME not like 'Intact%'
order by ANIMAL_ID ;
```
```sql
where I.SEX_UPON_INTAKE REGEXP 'INTACT'
  and O.SEX_UPON_OUTCOME REGEXP 'Neutered|Spayed'
```
39. 우유와 요거트가 담긴 장바구니 🍄
```sql
select CART_ID
from (
    select CART_ID,
        GROUP_CONCAT(NAME order by Name SEPARATOR ', ') as namelist
    from CART_PRODUCTS
    group by CART_ID 
    ) foo
where namelist REGEXP 'Milk.*Yogurt'
order by CART_ID ;
```
```sql
select CART_ID 
from CART_PRODUCTS
where NAME in ('Milk','Yogurt')
group by CART_ID
having count(distinct(NAME)) >= 2 
order by CART_ID ; 
```
40. 5월 식품들의 총매출 조회하기
```
select O.PRODUCT_ID, P.PRODUCT_NAME, sum(O.AMOUNT*P.PRICE) as TOTAL_SALES
from FOOD_ORDER O
join FOOD_PRODUCT P
on O.PRODUCT_ID = P.PRODUCT_ID
where date_format(O.PRODUCE_DATE,'%Y-%m-%d') 
        between '2022-05-01' and '2022-05-31'
group by 1, 2      
order by 3 desc, 1 asc ;

PRODUCT_ID가 PK라면 ID에 대해 NAME이 여러개 존재하지 않기 때문에 group by 1 만 해도 됨
```
41. 서울에 위치한 식당 목록 출력하기
```
select R.REST_ID, I.REST_NAME, I.FOOD_TYPE, I.FAVORITES, I.ADDRESS, round(avg(R.REVIEW_SCORE),2) as SCORE 
from REST_REVIEW R
join REST_INFO I
on R.REST_ID = I.REST_ID
where ADDRESS like '서울%'
group by 1 
order by SCORE desc, I.FAVORITES desc ; 
```
