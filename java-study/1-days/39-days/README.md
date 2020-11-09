---
description: 2020.10.10 - 39일차
---

# 39 Days - toad복습, DECODE, GROUP BY

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## Toad - SQL

### Oracle - 소계, 총계

* 실제 테이블에 없는 소계, 총계를 추출해보자.
* 실제 테이블에 없는 row데이터이므로 주어진 테이블 자료에서 추출, 수집, 가공을 통해 출력한다. - 추출 : SELECT - 수집, 가공 : 조인, WHERE\(조건, 원하는 정보\) - 실제 없는 row를 테이블로 보이게 하는것을 돕는 것 : 카타시안의 곱\(정보수집\) - 실제 없는 컬럼을 테이블에 보이게하는것을 돕는 것 : rownum
* rno1에는 날짜를, rno2에는 총계를 보여준다. - rno가 2가지 경우의 수 - &gt; true or false -&gt; if문 -&gt; DECODE
* 결과를 말하기\(출력하기\)위해서는 먼저 듣기가 이루어 져야한다. - 소통하기 위해 UI가 필요하다. - UI를 통해 듣기가 일어난다. = 파라미터 -&gt; WHERE절 조건 - 결과를 말하기 한다. = 리턴값 -&gt; SELECT절 컬럼
* 조인은 집합 간의 관계를 연결해주는 것이다. - 1 : 1 - 1 : n - n : m = 카타시안 곱, 테이블 복제

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

### DECODE

* 경우의 수가 2개 이거나 하면 비교할때 if문이 필요하다.
* DML은 if문을 사용할 수 없고, DECODE함수를 사용한다.
* decode\(비교값, 조건, true값, false값\)
* decode는 from절을 제외한 어디든 사용될 수 있다. - SELECT문 컬럼자리에, - WHERE절에, WHRER절의 and, or다음에도 올 수 있다.

### SELECT, GROUP BY

```sql
SELECT ename FROM emp
GROUP BY ename --14가지

SELECT deptno FROM emp
GROUP BY deptno --3가지
```

* 기본적인 구문
* SELECT컬럼 = GROUP BY 집합 이므로 그룹화 한 효과는 없을 것이다.

### ORA-00979 GROUP BY error

![ORA-00979](../../../.gitbook/assets/.png%20%289%29.png)

```sql
SELECT deptno, ename FROM emp
GROUP BY deptno

SELECT ename, deptno FROM emp
GROUP BY ename --사용불가 둘다 같이 묶일 수 없으므로
```

* ora-00979 = JAVA SQLException
* 오라클 쿼리문 에러
* 조회하는 컬럼과 그룹을 적용하는 컬럼의 차이로 인해서 발생
* group by문을 사용할 때는 반드시 그룹 함수 외에 조회하는 컬럼 모두를 group by문에 표시해야 한다. 
* 위 구문은 deptno &gt; ename 이므로 성립하지만 밑 구문은 ename이 deptno을 그룹화하지 못하기 때문에 에러가 발생한다.

### 해결방법 - 그룹함수

![&#xD574;&#xACB0;&#xBC29;&#xBC95; 1](../../../.gitbook/assets/1%20%2822%29.png)

```sql
SELECT ename, MAX(deptno), MIN(deptno) FROM emp
GROUP BY ename--ename의 부서번호가 아닌 그저 최대, 최소값 부서번호가 나온다.
```

* deptno에 그룹함수를 사용한다.
* 그룹함수 : sum, max, count, ....
* 문법적 에러는 해결되겠지만 매칭되는 정보가 아닌 값이 나와 필요한 정보를 얻을 수 없다.

### 해결방법 - GROUP BY

![&#xD574;&#xACB0;&#xBC29;&#xBC95; 2](../../../.gitbook/assets/2%20%2814%29.png)

```sql
SELECT ename, deptno FROM emp
GROUP BY ename, deptno
```

* 두번째는 나머지 컬럼에도 그룹화를 한다는 것이다.
* 하지만 이렇게 모든 컬럼에 그룹화를 하게되면 그룹화를 하는 의미없이 원본의 테이블을 얻게 될 것이다.

```sql
--단, 예외인 경우가 존재한다. 어떤경우일까?
-- 힌트 : 중복을 제거하는 데이터를 조회해보자
SELECT distinct(deptno) FROM emp

SELECT distinct(ename) FROM emp

SELECT distinct(empno) FROM emp

SELECT deptno, sum(sal) FROM emp
GROUP BY deptno
```

* 물론 위와 같이 사용자가 원하는 정보를 얻는 경우도 존재하낟.
* 부서번호별 연봉합계
* 
### HAVING

![HAVING](../../../.gitbook/assets/having%20%281%29.png)

```sql
SELECT deptno, sum(sal) FROM emp
GROUP BY deptno
HAVING sum(sal)>10000
--부서별 연봉중 1000이상이것이 나오므로 의미있는 값이다.
```

* GROUP BY절 이후, 그룹화된 그 결과에 대한 조건검색을 할때, WHERE절은 사용불가능하다.
* HAVING절을 사용해야한다.

{% page-ref page="../29-days/" %}

### 인라인뷰, 알리아스명

![](../../../.gitbook/assets/having2.png)

```sql
--HAVING절에는  알리아스명은 사용할수 있을까?
--인라인뷰에서 사용되면 사용할 수 있다.
--인라인뷰 없이 FROM절 뒤에는 알리아스명을 사용할 수 없다.
SELECT s_sal
  FROM (SELECT deptno, sum(sal) s_sal FROM emp
        GROUP BY deptno)
WHERE s_sal>10000
```

* FROM절 안의 인라인뷰에서 정의된 알리아스명은 그 FROM절 밖에서도 사용이 가능하다.
* SELECT절, WHERE절, HAVING절, ....
* 인라인뷰 없이는 FROM절이아닌, 서브쿼리 등에서 정의된 알리아스명은 밖에서 사용될 수 없다.

## Explain plan & index

### 1단계 - 옵티마이저, pk, index

![index&#xAC80;&#xC0C9;&#xC744; &#xD1B5;&#xD55C; explain plan](../../../.gitbook/assets/1%20%2818%29.png)

```sql
SELECT empno FROM emp--index검색
```

* Explain plan 은 oracle의 옵티마이저의 실행계획을 보여주는 것이다.
* 옵티마이저는 두가지 동작 모드가 있다.
  * 룰 베이스 옵티마이저 모드 - 15가지 기준에 따라 순서를 정해 진행한다. - ex\) index가 존재하는 구문이면 index검색을 최우선으로 한다. - 수동카메라 : 개발가자 기준을 조정할수 있다. -&gt; 힌트문
  * 비용기반 옵티마이저 모드 - 비용을 계산해 가장 적은 cost로 진행한다. - 통계정보가 많을 수록 정확도가 높다. - 자동카메라
* pk의 제약조건 : not null, unique
* pk는 unique index를 지원받는다. - pk를 조건으로한 검색을 하면,  전체 테이블을 스캔하지않고 일처리가 가능하고, 자동정렬이 된다.

{% page-ref page="../27-days-join/" %}

### 2단계 - table full 스캔

![table access full table](../../../.gitbook/assets/2%20%2815%29.png)

```sql
SELECT empno, ename FROM emp
```

* 위 처럼 index가 없는 ename이 추가되면 전체 테이블을 스캔하여 응답한다.

### 3단계 - index, 조건절

![index unique scan ](../../../.gitbook/assets/3%20%2814%29.png)

```sql
SELECT empno, ename FROM emp
WHERE empno=7566
```

* 위 실행계획을 보면 사용자가 조건문에 PK의 index값을 지정했으므로 테이블이 아닌 index를 먼저 드라이브 하는 것을 알 수 있다.
* index가 존재하더라도 조건절에서 묻지않으면 사용하지 않는다.
* index를 사용하려면 해당 pk컬럼이 조건절에서 사용되어야한다.

### 4단계 - index, 옵티마이저 조작

![table access full table](../../../.gitbook/assets/4%20%2813%29.png)

```sql
SELECT empno, ename FROM emp--4
WHERE empno!=7566

SELECT empno, ename FROM emp
WHERE empno<>7566
```

* 위 !=, &lt;&gt; 연산자 같이 조건절의 index를 사용자가 무력화 시킬 수 있다. = 옵티마이저를 조작할 수 있다.
* 조건절에오는 좌변의 pk를 가공하면 index로서 사용될 수 없게 할 수 있다.

### IN, explain plan

![IN](../../../.gitbook/assets/in-.png)

```sql
--테이블을 한번만 읽어서 처리
SELECT dname, loc FROM dept WHERE deptno IN(10,30)--집합 한개

--테이블을 두번 읽어서 처리
SELECT dname, loc FROM dept WHERE deptno = 10--집한 한개
UNION ALL 
SELECT dname, loc FROM dept WHERE deptno = 30--집합 두개
```

* 괄호 안의 특정값이나 서브쿼리의 결과값이 포함되는지 체크해주는 역할
* 특정 컬럼의 값을 조건으로 이용할 때 사용한다.
* 위 두 구문은 같은 결과값을 보여주지만,  - 2번 : 테이블을 한번만 읽는다. - 5-7번 : 테이블을 두번 읽는다.

{% page-ref page="../29-days/" %}

{% page-ref page="t\_orderbasket.md" %}

후기 : 날씨가 추워지고 있다. 따숩게 입고다니면서 컨디션 조절 잘하자!

