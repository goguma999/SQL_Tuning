# 순위 매기기: RANK, DENSE_RANK, ROW_NUMBER
```sql
SELECT name, attack,
  RANK() OVER (ORDER BY attack DESC) AS rank_rank,             #RANK: 공동 순위가 있으면 다음 순서로 건너뜀 
  DENSE_RANK() OVER (ORDER BY attack DESC) AS rank_dense_rank, #DENSE_RANK: 공동 순서가 있어도 다음 순서를 건너뛰지 않음 
  ROW_NUMBER() OVER (ORDER BY attack DESC) AS rank_row_number
FROM mypokemon;
```

