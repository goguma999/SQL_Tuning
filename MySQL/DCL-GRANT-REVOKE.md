# DCL - GRANT, REVOKE
- 사용자 생성하기
```sql
CREATE USER [사용자 이름]@[ip주소];
```
```sql
CREATE USER [사용자 이름]@[ip주소] IDENTIFIED BY '[사용자 비밀번호]';
```
- 사용자 삭제
```sql
DROP USER [사용자 이름];
```
## GRANT, REVOKE 
- 현재 사용자가 누구인지 확인
```sql
SELECT USER(), CURRENT_USER();
```
- 현재 사용자가 가진 시스템 권한 확인
```sql
SHOW GRANTS FOR current_user;
```
- 권한 부여하기
```sql
# GRANT [권한] ON [데이터베이스 이름].[테이블 이름] TO [사용자 이름]@[ip주소];
GRANT ALL PRIVILEGES ON ann.emp TO scott@%;  # 해당 아디리를 가지는 모든 ip에서의 접근 허용
GRANT SELECT, INSERT ON *.* TO scott@%;   # 모든 데이터베이스, 모든 테이블에 대한 권한 부여 
```
- 권한 확인하기
```sql
SHOW GRANTS FOR [사용자 이름]@[ip주소];
```
- 권한 삭제하기
```sql
REVOKE [권한] ON [데이터베이스 이름].[테이블 이름] FROM [사용자 이름]@[ip주소];
```
- 권한 적용하기
```sql
FLUSH PRIVILEGES;
```


