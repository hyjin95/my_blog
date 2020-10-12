---
description: 2020.10.10 - 39일차
---

# 39 Days - toad복습, try-catch,

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## Toad - SQL

### Oracle - 소계, 총계

* 실제 테이블에 없는 소계, 총계를 추출해보자.
* 실제 테이블에 없는 row데이터이므로 주어진 테이블 자료에서 추출, 수집, 가공을 통해 출력한다. - 추출 : SELECT - 수집, 가공 : 조인, WHERE\(조건, 원하는 정보\) - 실제 없는 row를 테이블로 보이게 하는것을 돕는 것 : 카타시안의 곱\(정보수집\) - 실제 없는 컬럼을 테이블에 보이게하는것을 돕는 것 : rownum
* rno1에는 날짜를, rno2에는 총계를 보여준다. - rno가 2가지 경우의 수 - &gt; true or false -&gt; if문 -&gt; DECODE - decode\(비교값, 조건, true값, false값\)
* 결과를 말하기\(출력하기\)위해서는 먼저 듣기가 이루어 져야한다. - 소통하기 위해 UI가 필요하다. - UI를 통해 듣기가 일어난다. = 파라미터 -&gt; WHERE절 조건 - 결과를 말하기 한다. = 리턴값 -&gt; SELECT절 컬럼
* 조인은 집합 간의 관계를 연결해주는 것이다. - 1 : 1 - 1 : n - n: m = 카타시안 곱, 테이블 복제

### 자료의 정렬, 순서

* Oracle : ORDER BY
* List : 순서가 있다.
* Map : 순서가 없다.

### 컬럼, VO, key 이름

* 컬럼명, VO이름, Map key값이 동일한것을 가리키고있다면 같은 이름을 지어 혼잡함을 방지하자.
* key값은 대문자를 사용한다.

### 예외처리 try-catch

* try{ }catch\(SQLException se\) {               1번 그물 }catch\(Exception e\){             2번 그물 }
* 1번그물이 2번 그물보다 촘촘한 그물이여야한다.
* 2번그물은 1번 그물보다는 넓어진 그물이다.

### Cursor

* 커서는 모든 DB제품에 존재한다. - oracle, DB2, my-sql, ms-sql, ....
* SELECT = 조회 -&gt; 결과 = Cursor
*  테이블에서 개발자가 필요로하는 정보를 SELECT문으로 커서를 조작해 조회한다.
* 같은 '검색'버튼이더라도 case, 경우에 따라 기능이 달라질 수 있다. - case1 지식인,  case2 영화, case3 음악, ....
* '동작'이라는 것을 같은 이름의 메서드로 통일해 사용하자 - ActionListener, MouseListener,..... - 누구나 재정의, 오버라이드하여 사용할 수 있다. - 인터페이스의 생성
* EX\) 어느 상품의 제조사 홈페이지에서 날짜입력창&검색버튼, 제조명입력창&검색버튼 으로 검색이 가능하다면, 여기서 SELECT문은 WHERE절의 날짜입력값 AND 제조명입력값 이 될 것이다. - 날짜, 제조명 = 컬럼 - 날짜입력값, 제조명입력값 = 파라미터

```sql
SELECT ename FROM emp
GROUP BY ename --14가지

SELECT deptno FROM emp
GROUP BY deptno --3가지
```

![ORA-00979](../../.gitbook/assets/.png%20%289%29.png)

```sql
--ename은 GROUP BY 표현식이 아닙니다.=JAVA SQLException
--ora-000979 에러는 무조건 오라클 쿼리문 에러인 것이다.
SELECT deptno, ename FROM emp
GROUP BY deptno

SELECT ename, deptno FROM emp
GROUP BY ename --사용불가 둘다 같이 묶일 수 없으므로
```

![&#xD574;&#xACB0;&#xBC29;&#xBC95; 1](../../.gitbook/assets/1%20%2822%29.png)

```sql
--해결방법1 : deptno에 그룹함수(sum,count,...)를 사용한다, 문법적 문제는 해결되지만 의미있는 정보가 아님
SELECT ename, MAX(deptno), MIN(deptno) FROM emp
GROUP BY ename--ename의 부서번호가 아닌 그저 최대, 최소값 부서번호가 나온것이다.

```

![&#xD574;&#xACB0;&#xBC29;&#xBC95; 2](../../.gitbook/assets/2%20%2814%29.png)

```sql
--해결방법2 : deptno도 GROUP BY절에 포함시킨다. ename이 다 다른값이므로 효과가 없다는것이 문제
SELECT ename, deptno FROM emp
GROUP BY ename, deptno
```

```sql
--단, 예외인 경우가 존재한다. 어떤경우일까?
-- 힌트 : 중복을 제거하는 데이터를 조회해보자
SELECT distinct(deptno) FROM emp

SELECT distinct(ename) FROM emp

SELECT distinct(empno) FROM emp

```

![HAVING](../../.gitbook/assets/having%20%281%29.png)

```sql
--그룹화 한 뒤에 그 결과에 대한 조건검색을 할때 WHERE절은 불가능하다. HAVING절 사용
SELECT deptno, sum(sal) FROM emp
GROUP BY deptno
HAVING sum(sal)>10000
--부서별 연봉중 1000이상이것이 나오므로 의미있는 값이다.
```

![](../../.gitbook/assets/having2.png)

```sql
--decode는 from절을 제외한 어디든 사용될 수 있다.
--SELECT문 컬럼자리에, 
--WHERE절에, WHRER절의 and, or다음에도 올 수 있다.

--HAVING절에는  알리아스명은 사용할수 있을까?
--인라인뷰에서 사용되면 사용할 수 있다.
--인라인뷰 없이 FROM절 뒤에는 알리아스명을 사용할 수 없다.
SELECT s_sal
  FROM (SELECT deptno, sum(sal) s_sal FROM emp
        GROUP BY deptno)
WHERE s_sal>10000
```

## Explain plan & index

### 1단계

![1](../../.gitbook/assets/1%20%2818%29.png)

```sql
--explain plan
--옵티마이저에게는 두가지 동작 모드가 있다.
--1.룰베이스 옵티마이저 모드 : 15가지 기준에따라 순서를 정해 진행(ex index가 있으면 index 검색이 최우선), 기준을 조정할 수 있다. 수동카메라
--2.비용기반 옵티마이저 모드 : 비용을계산한다. 통계정보가 많을수록 정확도가 높다. 자동카메라

--pk제약조건 : not null, unique 
--pk는 unique index를 지원받는다.
SELECT empno FROM emp--1
```

### 2단계

![2](../../.gitbook/assets/2%20%2815%29.png)

```sql
SELECT empno, ename FROM emp--2
```

### 3단계

![3](../../.gitbook/assets/3%20%2814%29.png)

```sql
SELECT empno, ename FROM emp--3
WHERE empno=7566
--1.테이블을 먼저 드라이브 하는 것이 아니라 index를 드라이브한다.
--2.index가 존재하더라도 저건절에서 묻지않으면 사용하지 않는다.
--index를 사용하려면 해당 pk컬럼을 조건절에서 사용해야한다.
```

### 4단계

![4](../../.gitbook/assets/4%20%2813%29.png)

```sql
--index를 무력화시킬 수 있다.옵티마이저를 조작할 수 있다.
--좌변 pk를 가공하면 index로서 사용될수 없다.
SELECT empno, ename FROM emp--4
WHERE empno!=7566

SELECT empno, ename FROM emp
WHERE empno<>7566
```

### IN

![IN](../../.gitbook/assets/in-.png)

```sql
--in구문
--테이블을 한번만 읽어서 처리
SELECT dname, loc FROM dept WHERE deptno IN(10,30)--집합 한개

--테이블을 두번 읽어서 처리
SELECT dname, loc FROM dept WHERE deptno = 10--집한 한개
UNION ALL 
SELECT dname, loc FROM dept WHERE deptno = 30--집합 두개
```

{% page-ref page="t\_orderbasket.md" %}

후기 : 

