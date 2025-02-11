### 1. CASE WHEN을 활용한 정적 피벗
```sql
SELECT 
    year,
    SUM(CASE WHEN month = 'Jan' THEN sales ELSE 0 END) AS Jan,
    SUM(CASE WHEN month = 'Feb' THEN sales ELSE 0 END) AS Feb,
    SUM(CASE WHEN month = 'Mar' THEN sales ELSE 0 END) AS Mar,
    SUM(CASE WHEN month = 'Apr' THEN sales ELSE 0 END) AS Apr
FROM sales
GROUP BY year;
```

### 2. GROUP_CONCAT() + 동적 SQL을 활용한 동적 피벗
```sql
-- 1) GROUP_CONCAT()을 사용하여 `CASE WHEN` 구문 자동 생성
SET SESSION group_concat_max_len = 1000000;

SET @sql = NULL;     # @sql: MySQL에서 문자열을 저장하는 사용자 정의 변수

SELECT GROUP_CONCAT(DISTINCT 
    CONCAT('SUM(CASE WHEN month = ''', month, ''' THEN sales ELSE 0 END) AS `', month, '`')
) INTO @sql
FROM sales;

-- 2) 동적 SQL 생성
SET @sql = CONCAT('SELECT year, ', @sql, ' FROM sales GROUP BY year;');

-- 3) 동적 SQL 실행
PREPARE stmt FROM @sql;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```
