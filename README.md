## SQL  
🔗[MySQL 노트 바로가기](MySQL) <br>
🔗[Oracle 노트 바로가기](https://changeable-yacht-8d2.notion.site/04df6f08611d4deeb4ef1bc8152867a4?v=940a168a07e34cb7b2218cf4c058bc20&pvs=4) <br>
교재: 초보자를 위한 SQL 200제 (PL/SQL)<br> 

&nbsp;

## Codingtest
🔗[바로가기](codingtest) <br>
Fastcampus 강의, HackerRank, Programmers 에서 제공하는 SQL 문제 풀이 기록입니다. 

&nbsp; 

## SQL 튜닝 (Oracle)

**▪︎ 001. SELECT문의 실행과정 3단계**:  📄[노트](https://www.notion.so/001-select-3-db-83e00fa0d6f246b887985901ce869a3a)
  
**▪︎  002. 옵티마이져란 무엇인가 ?**: 📄 [노트](https://www.notion.so/002-2f4cf8113f4f4d75a326f11f01cf6c01?pvs=21)
  
**▪︎  003. 실행계획의 종류 2가지 ?**: 📄[노트](https://www.notion.so/003-2-99d5f15e44af4534a30fe326d515ef8b?pvs=21)

### 📍인덱스 튜닝

**▪︎  004.  WHERE 절의 인덱스 컬럼을 가공하지 말 것 !**: 📄[노트](https://www.notion.so/004-where-75f08a94455f4983a03aacd61f131dd9?pvs=21)

**▪︎  005. HAVING 절에 일반 검색조건을 쓰지 말 것 !**: 📄[노트](https://www.notion.so/005-having-e9c7507edf4642259ad737343fb65ea0?pvs=21)

**▪︎  006. WHERE 절의 인덱스 컬럼 가공이 불가피하다면 함수 기반 인덱스를 생성할 것 !**: 📄[노트](https://www.notion.so/006-where-354c32c21e5f418399ed67f064bc5a97?pvs=21)

**▪︎  007. 암시적 형변환에 주의할 것 !**: 📄[노트](https://www.notion.so/007-8007132262fa49728ede33237b619cc5?pvs=21)

**▪︎  008. Order by 를 통한 과도한 정렬작업을 피할 것 !**: 📄[노트](https://www.notion.so/008-order-by-960405b5b79646beb25408c76f892a5f?pvs=21)

**▪︎  009. 그룹함수 대신 인덱스를 사용해서 SQL을 튜닝할 것**: 📄[노트](https://www.notion.so/009-SQL-f0cb0b5715604489b08c382b1565f072?pvs=21)

**▪︎  010. 인덱스를 엑세스 하지 못하는 검색조건을 알아둘 것**: 📄[노트](https://www.notion.so/010-0affd4201d4941c699f989be85658d35?pvs=21)

**▪︎  011. full table scan 을 할 수 밖에 없다면 full table scan 이 빠르게 되도록 튜닝할 것**: 📄[노트](https://www.notion.so/011-full-table-scan-full-table-scan-5532205fb18848f38cef43a119677002?pvs=21)

**▪︎  012. 인덱스를 탈 수 있도록 힌트를 사용할 것.**: 📄[노트](https://www.notion.so/012-d371abdb21474bc9bd253ff05c80545a?pvs=21)

**▪︎  013. index merge scan - 훌륭한 인덱스 2개를 같이 사용하여 시너지 효과를 만들자**: 📄[노트](https://www.notion.so/013-index-merge-scan-2-cec3e45d90bc41ea80eb3712260dfc73?pvs=21)

**▪︎  014. 테이블 랜덤 엑세스를 줄이기 위해 결합 컬럼 인덱스를 사용할 것**: 📄[노트](https://www.notion.so/014-621895d9c5c048dbb4fd01eb4c355374?pvs=21)

**▪︎  015. 결합 컬럼 인덱스 구성시 컬럼순서에 신경쓰자**: 📄[노트](https://www.notion.so/015-18edbafb1e3e4862861491ced2bc05f0?pvs=21)

**▪︎  016. index skip scan**: 📄[노트](https://www.notion.so/016-index-skip-scan-aa612ff031ed4433a3a89695afe3d474?pvs=21)

**▪︎  017. index full scan**: 📄[노트](https://www.notion.so/017-index-full-scan-314a8343754c43d584a9e0ba6bde5675?pvs=21)

**▪︎  018. index fast full scan**: 📄[노트](https://www.notion.so/018-index-fast-full-scan-b1fe3e4545ee4520bb51cce78aec5e7e?pvs=21)

**▪︎  019. index bitmap merge scan**: 📄[노트](https://www.notion.so/019-index-bitmap-merge-scan-2c266e98444c4e10a80438d0e74d02c5?pvs=21)

**▪︎  020. index unique scan**: 📄[노트](https://www.notion.so/020-index-unique-scan-d26afbab4c9743bd8e3289f6a9d982ab?pvs=21)

### 📍조인 튜닝

**▪︎  021. 조인문장을 튜닝할 때 조인 순서 튜닝이 중요함**: 📄[노트](https://www.notion.so/021-3f0460d5a1ff4cab92e95acb49a6350e?pvs=21)

**▪︎  022. 검색조건에 따라 조인 순서를 잘 정해줄 것**: 📄[노트](https://www.notion.so/022-5501e5b551ee474fa090f8bcdfa84607?pvs=21)

**▪︎  023. nested loop 조인_조인 되는 양이 작을 때**: 📄[노트](https://www.notion.so/023-nested-loop-_-e3113eb4ac754160b02b8b95bd675004?pvs=21)

**▪︎  024. nested loop 조인_3개 이상 조인 때 힌트 쓰기**: 📄[노트](https://www.notion.so/024-nested-loop-_3-4b0f0bfcf6f94c4999ad1cde3b60a556?pvs=21)

**▪︎  025. hash 조인_대량의 데이터를 조인할 때**: 📄[노트](https://www.notion.so/025-hash-_-0fe74801f1d344ac89bc735fdb30a7f1?pvs=21)

**▪︎  026. hash 조인_3개 해쉬 조인할 때 해쉬 테이블을 선택하기**: 📄[노트](https://www.notion.so/026-hash-_3-1de488b0ece8454892ded9b3f368d6d3?pvs=21)

**▪︎  027. hash 조인_hash 안 되는 연산자 주의**: 📄[노트](https://www.notion.so/027-hash-_hash-a8e24d88a29a4d6487b3abc5316b4194?pvs=21)

**▪︎  028. 대량의 테이블을 조인하는데 해쉬조인을 할 수 없다면 sort merge join 사용**: 📄[노트](https://www.notion.so/028-sort-merge-join-879e514deec44632be2bcfbb090df258?pvs=21)

**▪︎  029. outer join 튜닝**: 📄[노트](https://www.notion.so/029-outer-join-9390264943974b7a97a6850d051acd09?pvs=21)

**▪︎  030. 3개 이상의 테이블을 조인할 때 조인 방법을 다양하게 조절하자**: 📄[노트](https://www.notion.so/030-3-07a3be3fea304b77878f6cb4b78cba68?pvs=21)

**▪︎  031.선택적 조인을 하면서 조인의 성능을 높이자**: 📄[노트](https://www.notion.so/031-88e9e7a403a347cf949475028f21b243?pvs=21)

**▪︎  032. 인라인 뷰와 조인할 때는 인라인 뷰가 해체 되지 않도록 할 것 !**: 📄[노트](https://www.notion.so/032-bf6d670c2dd94f8f8d7aa7f05e8cad00?pvs=21)

**▪︎  033. 뷰와 조인을 할 때 조인 순서를 조정할 수 있음 !**: 📄[노트](https://www.notion.so/033-763013597129461499eea4887d27f5a6?pvs=21)

**▪︎  034. 조인의 성능을 높이고 싶다면 MVIEW 생성을 고려할 것 !**: 📄[노트](https://www.notion.so/034-MVIEW-4616f488cc9f40a4bc191e3ad8068a17?pvs=21)

### 📍서브쿼리 튜닝

**▪︎  035. 서브쿼리문에서 서브쿼리의 데이터가 작으면 서브쿼리부터 수행되게 할 것 !**: 📄[노트](https://www.notion.so/035-cf2facda4aea4ce5be8b222b7a395b71?pvs=21)

**▪︎  036. push_subq 와 no_push_subq 와의 짝꿍힌트를 알아둘 것 !**: 📄[노트](https://www.notion.so/036-push_subq-no_push_subq-31a66174b183451da69bb8d6be6713d9?pvs=21)

**▪︎  037. 서브쿼리를 세미조인으로 변경해서 수행되게 할 것!**: 📄[노트](https://www.notion.so/037-8d2a9c4834254ac1b23fc987032235d8?pvs=21)

**▪︎  038. 해쉬 세미 조인도 서브쿼리 부터 수행되게 할 수 있다 !**: 📄[노트](https://www.notion.so/038-e3ff27e8e1304c0abf2a123c6c056764?pvs=21)

**▪︎  039. not in 연산자를 사용한 서브쿼리문 튜닝**: 📄[노트](https://www.notion.so/039-not-in-4faaca6d83794664b4c3bfea9e0a607d?pvs=21)

### 📍Rewrite 

**▪︎  040. in -> exists 문**: 📄[노트](https://www.notion.so/040-in-exists-c1d86fe206ab44bd8c286ea8a546e287?pvs=21)

**▪︎  041. minus -> not exists 문**: 📄[노트](https://www.notion.so/041-minus-not-exists-51908b775fae4d18a3a35035865bd839?pvs=21)

**▪︎  042. 데이터 분석함수를 이용한 SQL 재작성 1**: 📄[노트](https://www.notion.so/042-SQL-3526199604504fd9bebbc91a59017035?pvs=21)

**▪︎  043. 데이터 분석함수를 이용한 SQL 재작성 2**: 📄[노트](https://www.notion.so/043-SQL-e892da30ac494cfa9b8eaab140fca341?pvs=21)

**▪︎  044. 데이터 분석함수를 이용한 SQL 재작성 3**: 📄[노트](https://www.notion.so/044-SQL-ebb542ad97f44c9d9031d21bd59f72ea?pvs=21)

**▪︎  045. 데이터 분석함수를 이용한 SQL 재작성 4**: 📄[노트](https://www.notion.so/045-SQL-2628127b186149208c8a9793e37e2ca6?pvs=21)

**▪︎  046. 데이터 분석함수를 이용한 SQL 재작성 5**: 📄[노트](https://www.notion.so/046-SQL-672e8d2c00234479a48fbfb467acc1c5?pvs=21)

**▪︎  047. update 문의 튜닝방법**: 📄[노트](https://www.notion.so/047-update-a789aa03af27479cb87ffef383d6f770?pvs=21)

**▪︎  048. 파티션 테이블이란 ?**: 📄[노트](https://www.notion.so/048-ffa182ecd0da4d0e96b135a8285f6ec0?pvs=21)

**▪︎  049. 리스트 파티션이란 ?**: 📄[노트](https://www.notion.so/049-6b3ea7c35b474540a8dfa17d2a6b3a44?pvs=21)

**▪︎  050. 해쉬 파티션이란 ?**: 📄[노트](https://www.notion.so/050-3e3df5897d2942f1a25e288170a587d9?pvs=21)


&nbsp;

