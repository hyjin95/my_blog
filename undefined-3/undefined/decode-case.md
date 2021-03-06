# 조건문 : DECODE 와 CASE

### DECODE

```sql
SELECT A
    DECODE(A, '0', '무료, '50', '저렴') 변환값
FROM table
```

* table 테이블의 데이터를 변환값 컬럼을 만들어 변환값을 보여준다.

```sql
//값이 조건에 만족하면 true값 아니면 null
DECODE(값, 조건, true값)

//값이 조건에 만족하면 true값 아니면 false값
DECODE(값, 조건, true값, false값)

//값이 1번조건을 만족하면 true1, 2번 조건을 만족하면 truethen2 ...
//모든 조건에 만족하지 않으면 false값
DECODE(값, 조건1, true값, 조건2, true값2 ..., false값)

//값이 조건을 만족하고 값2가 조건2를 만족하면 true값 아니면 null
DECODE(값, 조건, DECODE(값2, 조건2, true값))
```

* 경우의 수가 2개 이상이라면 if문이 필요하지만 DML에서는 if문을 사용할 수 없다. DML에서는 DECODE 함수를 사용한다.
* 데이터를 조회할때 특정 데이터 값을 변환해 출력할 수 있다.
* FROM 절을 제외한 어디든 사용될 수 있다. - SELECT문, WHERE절
* false값을 명시 하지 않으면 null을 출력한다.
* SQL에서만 사용가능

### CASE

```sql
CASE 값(컬럼)
    WHEN IF조건1 true값1
    WHEN IF조건2 true값2
    ...
ELSE false
END
```

* 약어나 코드를 읽기 쉬운 값으로 바꿔 줄때 데이터를 범주화 할때 사용한다.
* 예를들어 성적이 90점 이상이면 우등생 으로 출력하는 경우 50보다 작은 값들은 모두 under $50 으로 출력하는 경우
* 조건에는 =나 &gt;같은 비교연산자가 올 수 있다. 단 case 값 when이 아닌 case 컬럼 when의 경우에는 비교연산자를 작성할 수 없다.
* DECODE보다 확장적이다. - PL/SQL, SQL모두 사용가능

### 차이점

* 조건에서의 비교연산자 사용 여부 - DECODE함수는 &lt;, &gt;, = 와같은 비교연산자를 작성할 수 없다. - CASE 함수는 &lt;, &gt;, = 와같은 비교연산자를 작성할 수 있다. - 단, CASE 값에 컬럼이 위치하는 경우에는 비교연산자를 사용할 수 없다.
* PL/SQL지원 여부 - DECODE함수는 SQL에서만 작동된다. - CASE함수는 SQL과 PL/SQL모두 작동된다.
* null = null 비교시 응답의 차이 - DECODE 함수는 null과 null 비교시 true를 반환한다. - CASE 함수는 null과 null 비교시 false를 반환한다.

## 참고

{% page-ref page="../../java-study/1-days/25-days/" %}

