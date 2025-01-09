# TCL - COMMIT, ROLLBACK
- MySQL은 기본적으로 AUTOCOMMIT 모드가 활성화 되어 있음
```sql
SET AUTOCOMMIT = 0;  # AUTOCOMMIT 비활성화 
``` 
- 트랜잭션 시작 및 확정
```sql
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
COMMIT; 
```
- 트랜잭션 이전으로 돌아가기
```sql
ROLLBACK;
```
- SAVEPOINT
```sql
# 세이브포인트 만들기
SAVEPOINT [세이브포인트 이름];

# 세이브포인트로 돌아가기
ROLLBACK TO [세이브포인트 이름];

# 세이브포인트 제거하기
RELEASE SAVEPOINT savepoint_name;
``` 
