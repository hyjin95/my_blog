---
description: 2020.08.27 - 21일차
---

# 21 Days - JComboBox, DB연동, 회원가입화면

### 학습목표

* API에서 생성자 선택하기
* 파라미터 타입을 이용한 호출하기

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## JAVA

### API JComboBox의 생성자 종류

| [`JComboBox`](../../javax/swing/JComboBox.html#JComboBox-E:A-)`(`[`E`](../../javax/swing/JComboBox.html)`[] items)` |
| :--- |
| [`JComboBox`](../../javax/swing/JComboBox.html#JComboBox-java.util.Vector-)`(`[`Vector`](../../java/util/Vector.html)`<`[`E`](../../javax/swing/JComboBox.html)`> items)` |

* 주소 table의 row는 Vector, colum은 String 타입
* 1번 생성자 : E타입 배열을 파라미터로 갖는다.  -  E = element, 타입 - E \[ \] = ex\) String \[ \] - 타입이 정해져있는 고정형 파라미터를 갖는다. 
* 2번 생성자 : Vector&lt;E&gt; - &lt;E&gt; : 제네릭 타입 E를 갖는다. - 타입이 구체적으로 정해지지않은 자료구조형, 유동적 파라미터를 갖는다. - ZipcodeSearch.java에서는 String타입으로 구체화된다.

### 제네릭

* 클래스를 설계할 때 구체적인 타입을 명시하지 않고, 타입 파라미터 ex\)T, E, ...로 대체 했다가 실제 클래스가 사용될 때 구체적인 타입을 지정함으로서 형전환을 최소화한다.
* Object타입에 대한 번거로움과 오류를 줄여준다.
* 제네릭 타입 : 클래스이름\|\|인터페이스이름 + &lt;타입\(element\)&gt;

### Object 타입

* Object타입은 모든 종류의 자바 객체를 저장할 수 있다.
* 하지만 반환시 다른 타입의 객체들이 모두 Object타입으로 반환되므로, 읽어올때 원래 타입으로 형전환을 해주어야한다는 번거로움이 있다.
* 이를 보완하기위한 것이 제네릭이다.

{% page-ref page="zipcodesearch-jcombobox.md" %}

## Toad for Oracle

### UNION ALL

* row를 추가할때 사용한다.
* 열거형 연산자로 나열하면 1차배열로 인식하여 colum이 늘어난다.

### distinct\( \)

* 중복 제거할 때 사용한다.
* 조회하려는 테이블에 a모자를 가진 사람이 10명, b모자를 가진 사람이 10명, ..이런식일때 모자의 종류만을 조회하고 싶을때 사용할 수 있다.

### ALIAS

* 알리아스명, 별칭을 의미한다. - 필드명을 다른이름으로 바꿔주는 역할
* 테이블 : test, 필드 : field\_i  
  - select field\_id from test일때  
  - select field\_id as f\_id from test \|\| select field\_id f\_id from test  
  - 둘다 field\_id인 컬럼명을 f\_id로 별칭을 지어주게된다.

### 인라인뷰

* 인라인 뷰는 FROM절 안에서 사용되는 서브 쿼리이다.
* FROM\(SELECT 컬럼명 FROM 집합\)
* rownum을 묶어주거나, 원하는 row만을 뽑기위해 rownum을 초기화할때 사용한다.
*  ex\)최근 입사한 5명의 사원을 검색하려면 아래와 같은 쿼리문을 실행하면 된다. - SELECT ROWNUM, EMPNO, ENAME, HIREDATE  - FROM \(SELECT \* FROM EMP ORDER BY HIREDATE DESC\)  - WHERE ROWNUM &lt;= 5;

{% page-ref page="toad.md" %}

## 카카오톡 구현

* 메인화면 구현
* id입력창, pw입력창 구현
* 로그인, 회원가입 버튼 구현
* 회원가입 memberShip 구현
* 우편번호 조회 구현

## 한줄일기

세미 프로젝트와 병행하느라 시간이 눈코뜰새 없이 부족하지만 재미있다. 하루가 30시간이였으면 좋겠다는 생각을 한다.

