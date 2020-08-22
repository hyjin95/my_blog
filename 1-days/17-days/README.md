---
description: 2020.08.21 - 17일차
---

# 17 Days - toad data올리기,  java의 DB연동

### 복습

* 사용자 -요청-&gt; 자바 -요청-&gt; DB
*  데이터는 객체배열로 담고 DB에서 데이터를 꺼내주는 것은 Oracle으로, SELECT문으로 꺼낸다. - 객체배열에 담긴 데이터는 String타입이다. - 리턴타입 = 객체배열, 파라미터는 String타입
* **SELECT 컬럼명, 컬럼명, ... FROM 집합 WHERE 컬럼면 비교연산자** 
* 집합 = Table - Table구성요소 : row, colum - VO\(객체\) : 컬럼명을 변수로 갖는다. cols갯수 = 변수갯수, 변수하나에 1개값만 담는다. - 객체배열을 이용한다. - 구성요소를 담는것 = 객체배열 = 리턴타입 \( \[ \]메서드이름\( \) \)

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### DB연동하기

* 참고 : DBConnectionMgr.java

## JAVA

### DAO

* data Access object
* 클래스이름+Dao
* JAVA와 DB가 만나는 부분은 인터페이스가 필요하다.
* jdbc : DB연동 담당 클래스 꾸러미, jdbc API에는 생성자들이 담겨있다. - 인스턴스화가 필요하다. - java.sql
* DAO의 역할 - SIUD : SELECT\(조회\), INSERT\(입력\), UPDATE\(수정\), DELETE\(삭제\)

### 객체배열

* VOS\[ \] = null;
* VO vo = null;  일때
* VOS의 방에는 방마다 다른 new VO값\(null\) 이 들어온다.  -&gt; 반복해서 방마다 값을 담는다.
* while\( \) { vo = new VO\( \); }
* 방의 갯수를 우리가 모르기때문에 vo에 담긴 값을 Vectort에게 주어야한다. - copyInto함수 사용 - 소유주.copyInto\( \) - 소유주 = Vector - \( \)안에는 object가 들어가야한다. = VOS  O \( VO : X\) - 소유주와 object의 파라미터 타입을 맞춰야만한다.
* v.copyInto\(new String \[1\]\); - 1번방은 String 타입이다. - 하지만 size가 없으므로 null;

### Vector

* Vector를 쓰면 객체배열의 방의 갯수를 정하지 않아도 된다. - 유동적으로 늘었다 줄었다가 가능하다. = Flexible
* Vector배열의 방 안에는 Object타입만 담길 수 있다.
* 방을 지정하지않으면 순서대로 값이 담긴다.
* 끼워넣기가 가능하다. = 자동으로 밀려난다.

```java
import java.util.Vector;
public class VectorTest {

	public static void main(String[] args) {
		Vector v = new Vector();
		System.out.println(v.size());//0 add가 없어서 방이 생성되지 않았다.
		v.add("바나나");
		v.add("수박");
		v.add(1,"체리");
		
		for(Object obj:v) {
			String f = (String)obj;//캐스팅연산자로 타입을 맞춰준다.
			System.out.println(f);//바나나 체리 수박 : 끼워넣기
		}//end of for
	}//end of main
}//end of class
```

* 바나나는 0번 방에, 수박을 1번방에 저장된다.
* 체리가 1번방에 끼어짐으로 수박은 2번방으로 저장된다.
* 바나나 정보 꺼내기  - v.get\(0\); - v.get\("바나나"\); : 직관적이다.
* 수박 정보 지우기 - v.remove\(2\); - v.remove\("수박"\);
* 방의 갯수 - v.size\( \) \_ 지금은 3이다.

## toad for oracle

### test를 위한 다른 서버 접속

* Direct - Host에 상대 ip를 입력, port입력\(기본갑 1521\), SID체크, id입력 -&gt; 접속

### 자식레코드

* 자식 레코드가 발견되었습니다. \(계속 진행 =부모를 없애겠습니까?\) : 부모를 삭제할 수 없다.
* deptno\(부모\), empno\(자식\) 일때, - 둘다 기본키 pk를 갖는다. - 자식은 왜래키\(foreign key\)를 갖는다.
* ERD = 개체-관계 다이어그램, Entity-Relationship Diagram
* UML : 모델링언어

