---
description: 2020.10.10 - 39일차
---

# 39 Days - toad복습,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## Toad 복습

```sql
SELECT ename FROM emp
GROUP BY ename --14가지

SELECT deptno FROM emp
GROUP BY deptno --3가지

--ename은 GROUP BY 표현식이 아닙니다.=JAVA SQLException
--ora-000979 에러는 무조건 오라클 쿼리문 에러인 것이다.
SELECT deptno, ename FROM emp
GROUP BY deptno

SELECT ename, deptno FROM emp
GROUP BY ename --사용불가 둘다 같이 묶일 수 없으므로

--해결방법1 : deptno에 그룹함수(sum,count,...)를 사용한다, 문법적 문제는 해결되지만 의미있는 정보가 아님
SELECT ename, MAX(deptno), MIN(deptno) FROM emp
GROUP BY ename--ename의 부서번호가 아닌 그저 최대, 최소값 부서번호가 나온것이다.

--해결방법2 : deptno도 GROUP BY절에 포함시킨다. ename이 다 다른값이므로 효과가 없다는것이 문제
SELECT ename, deptno FROM emp
GROUP BY ename, deptno

--단, 예외인 경우가 존재한다. 어떤경우일까?
--그룹화 한 뒤에 그 결과에 대한 조건검색을 할때 WHERE절은 불가능하다. HAVING절 사용
SELECT deptno, sum(sal) FROM emp
GROUP BY deptno
HAVING sum(sal)>10000
--부서별 연봉중 1000이상이것이 나오므로 의미있는 값이다.

-- 힌트 : 중복을 제거하는 데이터를 조회해보자
SELECT distinct(deptno) FROM emp

SELECT distinct(ename) FROM emp

SELECT distinct(empno) FROM emp

--decode는 from절을 제외한 어디든 사용될 수 있다.
--SELECT문 컬럼자리에, 
--WHERE절에, WHRER절의 and, or다음에도 올 수 있다.
```

