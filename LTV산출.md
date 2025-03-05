# [DA] LTV 산출

### 📍LTV (Lifetime Value, CLV)

- 고객 생애 가치로 유저가 서비스를 사용하는 기간 동안 창출하는 매출
- LTV 활용 사례
    - **신규 고객 획득비용(CAC) 기준 설정**
    - 효율적인 마케팅 채널을 파악하여 예산 분배
- LTV 계산법
    - **LTV = ARPU * 리텐션**
        - **ARPU = 유저당 주문금액 (매출/유저수)**
        - 그룹을 코호트로 쪼개어서 LTV 비교 분석

### 📍새벽배송 플랫폼의 핵심 고객 연령층 알아보기

1. 측정인원: 2023년 1월에 첫 주문한 고객 중 10대 ~ 50대까지 **연령대별 100명 (총 500명)**
2. 분석방법: 2023년 1월에 첫 주문한 고객의 **6개월 간의 연령대별 인당 누적 LTV 계산**
    
    ![스크린샷 2025-03-03 오전 1.23.45.png](%5BDA%5D%20LTV%20%E1%84%89%E1%85%A1%E1%86%AB%E1%84%8E%E1%85%AE%E1%86%AF%201aa9dc3843b580579ffbce2125f7ec55/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-03-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.23.45.png)
    
3. SQL 로 분석하기
    
    ```sql
    with ord as (
    	select a.mem_no
    		,b.age_range
    		,b.first_ord_dt
    		,a.ord_dt
    		,a.order_amount
    	from order_master_ltv a
    	left join first_ord_table_ltv b on a.mem_no = b.mem_no
    	)
    , cohort as (
    	select age_range
    		,case when first_ord_dt = ord_dt then 'M-0'
    			  when first_ord_dt < ord_dt and ord_dt <= DATE_ADD(first_ord_dt, INTERVAL 1 MONTH) then 'M-1'
    			  when ord_dt > DATE_ADD(first_ord_dt, INTERVAL 1 MONTH) and ord_dt <= DATE_ADD(first_ord_dt, INTERVAL 2 MONTH) then 'M-2'
    			  when ord_dt > DATE_ADD(first_ord_dt, INTERVAL 2 MONTH) and ord_dt <= DATE_ADD(first_ord_dt, INTERVAL 3 MONTH) then 'M-3'
    			  when ord_dt > DATE_ADD(first_ord_dt, INTERVAL 3 MONTH) and ord_dt <= DATE_ADD(first_ord_dt, INTERVAL 4 MONTH) then 'M-4'
    			  when ord_dt > DATE_ADD(first_ord_dt, INTERVAL 4 MONTH) and ord_dt <= DATE_ADD(first_ord_dt, INTERVAL 5 MONTH) then 'M-5'
    			  else 'over_5m' end as month_nm
    		,count(distinct mem_no) as sample_cnt
    		,round(avg(order_amount)) as ARPU
    	from ord
    	group by 1,2
    	order by 1,2 )
    	
    	
    select age_range
    	,month_nm
    	,sample_cnt
    	,ARPU
    	,sample_cnt * 1.00 / max(sample_cnt) over (partition by age_range) as retention
    from cohort
    order by 1,2 ;
    ```
    
    ![스크린샷 2025-03-03 오전 1.49.10.png](%5BDA%5D%20LTV%20%E1%84%89%E1%85%A1%E1%86%AB%E1%84%8E%E1%85%AE%E1%86%AF%201aa9dc3843b580579ffbce2125f7ec55/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-03-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_1.49.10.png)
    
    ```python
    import pandas as pd
    df = pd.read_csv('/Users/ann/ltv.csv', encoding = 'utf-8')
    df['LTV'] = df['ARPU']*df['retention']
    ```
    
    ![스크린샷 2025-03-03 오전 2.10.30.png](%5BDA%5D%20LTV%20%E1%84%89%E1%85%A1%E1%86%AB%E1%84%8E%E1%85%AE%E1%86%AF%201aa9dc3843b580579ffbce2125f7ec55/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2025-03-03_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2.10.30.png)
    
    ```python
    df2 = df[df['month_nm'] != 'over_5m']
    ltv = df2.groupby('age_range')['LTV'].sum().reset_index()
    ltv
    ```
    
    | age_range | LTV |
    | --- | --- |
    | 10대 | 55881.83 |
    | 20대 | 83229.00 |
    | **30대** | **112074.93** |
    | 40대 | 94311.77 |
    | 50대 | 96026.73 |
    
    → 1월에 첫 주문한 10대가 인당 6개월 동안 5만 5천원정도를 지출한다. 
    
4. 결론
- 23년 1월 가입자 중 **30대**가 첫주문 이후 5개월까지 가장 높은 LTV를 나타냄