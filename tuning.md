**📌실행계획 보기**
```SQL
select /*+ gather_plan_statistics */ 
select * from table(dbms_xplan.display_cursor(null, null, 'ALLSTATS LAST') );
```

&nbsp;

**📌3개 테이블 해쉬조인**
```SQL
select /*+ leading(d e b) use_hash(e) use_hash(b) swap_join_inputs(b) */ 
```

&nbsp;

**📌인라인뷰와 조인할 때 ! 해체되지 않도록.**
```SQL
select /*+ no_merge(v) */ v.ename, v.loc, s.grade
  from salgrade s, ( select /*+ no_merge */ e.ename, e.sal, d.loc  
```
&nbsp;

**📌index merge scan**
```SQL
select /*+ and_equal ( emp emp_deptno emp_job ) */ ename, job, deptno 
  from emp
  where deptno = 30 and job = 'SALESMAN' ; 
```

&nbsp;

**📌선두 컬럼이 선분조건인 결합 컬럼 인덱스만 있을 때** 
```SQL
select /*+ index ( t m_salemm_salegb ) 
		no_index_ss(t m_salemm_salegb ) */ count(*)
  from mcustsum t 
  where salegb = 'A' 
	  and salemm between '200801' and '200812'; 
		and salemm in ( '200801','200802','200803','200804','200805','200806',
				'200807','200808','200809','2008010','200811','200812' ) ;
```

&nbsp;

**📌index bitmap merge scan** 
```SQL
select /*+ and_combine( emp emp_deptno emp_job ) */ ename, job, deptno 
  from emp
  where deptno = 30 and job = 'SALESMAN' ; 
```

&nbsp;






