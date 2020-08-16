---
description: 2020.08.12 - 11 일차
---

# 11 Days - SELECT문과 Vector, StringBuilder

### 복습

1. 클래스 안에 main함수가 없어도 되는가? - main함수가 없어도 된다. - main함수를 가진 다른 클래스에서 인스턴스화를 통해 호출하여 실하면 된다.
2. 인스턴스화는 두가지 문장으로 정의한다. - 선언 : Tivoli mycar = null;   - 생성 :  mycar = new Tivoli\( \);  : \(RAM에 저장, 생성된다.\) - Tivoli mycar = new Tivoli\(생성자\);
3. 생성자 : 타입 클래스이름과같은 메서드이다. - 구성 : 타입 변수이름 - overloading 으로 중복정의 할 수있다. 파라미터로 차이를 둔다. - 생성자는 인스턴스 생성할때 호출된다. 파라미터로 원하는 생성자를 호출한다. - 디폴트 생성자 : 파라미터가 없는 생성자
4. API - java에서 제공받는 이미 정의된 클래수들은 수정 불가하다. - 개발자가 정의하는 클래스는 API 규정을 준수하는 아래 수정할 수 있다.
5. toString - 클래스가 아닌 Object 꾸러에 내장된 클래스이다. - public toString\( \):String-Object - 접근제한자 클래스이름\( \):타입-소유주 - 인스턴스화를 하지않고 생성자 호출로 불러올 수 있다. - print\(mycar\); : 일반적인 경우 주소번지 출력, toString이 구현되어있으면 toString 클래스 호출된다.
6.  메서드 오버로딩 - 이름만 같고 파라미터의 갯수나 타입이 다른 메소드를 말한다. 호출시 맞는 파라미터, 타입을 가진 메서드를 호출한다. 인스턴스화 만으로는 호출되지 않고, 이름을 불러주어야 한다.
7. 메서드 오버라이딩 - 상속관계에서 부모클래스의 메서드의 파라미터, 타입은 그대로 두고 구현만 새로 정의해서 구현하는 메서드 이다.

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

### 예외처리

* 정보를 검색하는 메서드와 같이 무언가 오류가 날 수 있는 요소를 가진 메서드에 오류발생시 정지가 아닌 처리를 하는 것이다.
* 메서드를 이용해 인스턴스화를 하면 참조형 리턴타입을 갖게되는데 이때에는 API에 따라 예외처리를 꼭 해주어야 한다. 
* try { a라는 오류가 발생한다. b라는 오류가 발생한다. }catch\(a\){ 실행문} catch\(b\){실행문}
* 이런 식으로 a오류 발생시 실행오류되는것이 아니라 catch\(a\)를 실행시키도록 한다.
* 참고 :  DBConnectionMgr.java 17

### 추상 클래스, 인터페이스

* 타입 \( 클래스 \( 메서드, 변수\)\) - Car \( Sonata, Tivoli \(speed, wheelnum\)\)
* 타입 : 추상클래스, 인터페이스가 올 수 있다.
* 상속의 개념을 갖는다.
* 추상 클래스의 사용 : extends - class 타입\(추상클래스\)이름 extends 클래스이름 { ~ }
* 인터페이스의 사용 : implements - class 클래스이름 implements 타입\(인터페이스\)이름 { ~ }

### String, StringBuilder

* String은 문자열을 하나의 덩어리로 인식한다. - 수정하려면 = 대입연산자가 있어야 된다. - 수정이라기 보다는 통째로 갈아치워진다. - 원본들이 수정할때마다 생겨나므로 공간낭비가 생긴다.
* StringBuilder은  문자열을 각각 인식한다.  - 문자들이 나뉘어 있기때문에 수정이 가능하다.
* N%    : N~인 값, N이 앞에 붙어있는 값 : 인덱스를 통한 검색이 가능하다.
* %N% : 위치 상관없이 N이 붙어있는 값 : 인덱스를 통한 검색이 불가하기 때문에 검색속도가 느리다.
* 참고 : StringTest.java, DeptApp.java

### StringTest.java

```java
package book.ch5;

public class StringTest {

	public static void main(String[] args) {
		String msg = "hello";
		msg.replace("e", "o");//replace메소드가 e를 o로바꾼다.
		System.out.println(msg);//=연산자가 없어서 String은 원본을 유지한다.
		msg = msg.replace("e", "o");//replace 메소드를 msg에 대입시킨다.
		System.out.println(msg);
		
		StringBuilder sb = new StringBuilder("hello");//StringBuilder 타입은 뒤어 이어붙일 수 있다.
		sb.append(" world!!!");//append메소드는 붙여주는 메소드
		System.out.println(sb.toString());
		
		String str = "hello";//String은 덩어리개념이다.원본1
		str = str+" world";//덩어리째 hello world로 바뀐다.원본2
		str +=" java";//덩어리째 hello world java로 바뀐다.원본3 = 공간낭비
		System.out.println(str);
	}
}
```

* 출력결과 8번   hello 9번   hollo 14번 hello world!!! 19번 hello world java

### java와 oracle 연결

* Connection : 연결통로를 확보하는 인터페이스
* PreparedStatment : 자바에서 찾을 내용을 가져다주는 클래스 \(무엇을 조회할 것인지\) - PreparedStatment클래스가 executeQuery메서드를 제공한다.
* pstmt.excuteQuery : oracle에게 처리를 요청하는 메서드 - 반환타입 : Resultset인터페이스 :  \(true,1 \|\| false,0\)을 반환 - pstmt : excuteQuery를 사용하기 위한 인스턴스 주소 인터페이스로 new를 사용하지 않는다.
* Resultset인터페이스가 Curser를 조작, 이동시키는 Absolute 메서드를 제공한다.
* Cuser는 boolean타입의 s/w이다.
* re\(Resulteset\) 인터페이스가 제공하는 메서드 - next\( \) - previrous\( \) - 제공하는 메서드를 사용하려면 인스턴스화를 해서 찾아와야한다.
* 참고 : DBConnectionMgr.java, DeptApp.java

### DBConnectionMgr.java

```java
package oracle.db;

import java.sql.Connection;
import java.sql.DriverManager;

public class DBConnectionMgr {
	//공유 static(원본) 못바꾸게final "클래스이름"
	public static final String _DRIVER = "oracle.jdbc.driver.OracleDriver";
	//ClassNotFoundException오류가 날 수 있다.
	//물리적인 주소가 있어야한다. 
	public static final String _URL = "jdbc:oracle:thin:@192.168.0.187:1521:orcl11";
	//thin:ip주소:대문:식별자
	
	public static String _USER = "scott";//final이 없다 보안때문에 주기적으로 변경해야한다.
	public static String _PW   = "tiger";
	//클래스 두개를 사용하려는 것으로 다른 클래스에서도 사용해야 하므로 전역변수로 선언해야 한다.
  public Connection con = null;
	//물리적으로 떨어져 있는 오라클 서버와 연결 통로 만들기
	public Connection getConnection() {//Connection import, 참조형이다, 연결
		try {//예외처리를 했다. 실행시에 에러발생할 가능성이있는 코드는 try-catch로 예외처리한다.
			Class.forName(_DRIVER);///////////////////////////////드라이버 클래스 이름을 찾는 호출
			con = DriverManager.getConnection(_URL, _USER, _PW);//연결 호출 == 인스턴스화
		} catch (ClassNotFoundException ce) {/////////////////////클래스 이름을 찾을수 없는 오류 예외처리
			System.out.println("드라이버 클래스를 찾을 수 없습니다.");
		} catch (Exception e) {///////////////////////////////////con, 연결 오류에 대한 예외처리
			System.out.println("연결 실패 "+e.toString());
		}
		//return null;//NullPointerException 오류
		return con;//return 반환타입이 인터페이스이다.
	}
	public static void main(String args[]) {
		DBConnectionMgr dbMgr = new DBConnectionMgr();
		dbMgr.con = dbMgr.getConnection();
		System.out.println("con ===> "+dbMgr.con);
	}//end of main
}//end of class

```

* 8번 : 접근제한자 static\(원본이다\) final\(고정값\) ~
* 11번 thin은 드라이버 방식을 말한다. - 드라이버방식은 티어로 갈리는데 2티어는 클라이언트와 서버만있는 방식, 3티어는 클라이언트와 미드서버, 서버가 있는 방식으로 티어가 늘어날수록 보안이 좋아진다. - 이렇게 n개의 여러 티어가 있는 방식을 멀티티어 드라이버 방식이라고 한다. - 멀티 티어 드라이버 방식 = thin 드라이버 - 드라이버 방식 뒤에는 ip, oracle대문번호, 식별자 값이 들어온다.
* 14, 15번은 final이 붙지않는 변하는 값으로 보안상 주기적으로 바꿔주는것이 좋다.
* 17번 **Connection** : java와 oracle간의 연결통로를 확보하는 인터페이스로 사용시 인스턴스화 해야한다. -  선언 : public Connection con = null;  -  생성 : con = DriverManager.getConnection\(\_URL, \__USER, \_PW\); -_ 파라미터 안에 세 메서드가 들어가므로 **메소드를 이용한 인스턴스화**이다. - **리턴타입이 참조형**이여야한다. = Reference data Type, 부르면 값이아닌 **주소번지**가 호출된다. - URL : 드라이버방식,IP, port번호,식별자 / USER : 계정이름 / PW : 계
* 29번 에서 return null;이 아니라 con인터페이스가 와야한다.  - null로 반환하면 다른 클래스에서 Connection클래스 호출시, 수행된 con 값이 반환되어야지 비어있으면 오류가 나기때문이다.

### 객체 배열과 Vector

| 0,0 | 0,1 | 0,2 |
| :---: | :---: | :---: |
| 1,0 | 1,1 | 1,2 |
| 2,0 | 2,1 | 2,2 |

* DefaultTableModel클래스 위처럼 좌표로 표시된다.
* 인스턴스 변수 : dtm
* dtm.SetValueAt\(object, int,int\); 
* 메서드는 세개의 파라미터를 갖는다. 읽어온 값object, 좌표 int, 좌표 int

| deptno부서번호 | dname 부서이 | loc 지 |
| :---: | :---: | :---: |
| 10 | ACCOUNTTING | NEW YORK |
| 20 | RESEARCH | DALLAS |
| 30 | O | O |
| 40 | X | X |

| DeptVO\[0\] | DeptVO\[1\] | DeptVO\[2\] | DeptVO\[3\] |
| :---: | :---: | :---: | :---: |
| dvo | dvo | dvo | dvo |

* DeptVO는 deptno, dname, loc 이 세 변수를 갖는 클래스이다.
* 4개의 값을 모두 기억하기위해 객배열을 사용해서 주소를 기억한다.
* 객체배열은 주소를 따라가기때문에 인스턴스화가 반드시 필요하다.
* DeptVO\[ \] dvos = new DeptVO\[4\] - DeptVO\[0\]의 주소번지 dvo는 \(10, ACCOUNTTING, NEW YORK\)를 담는다. - DeptVO\[1\]의 주소번지 dvo는\(20, RESEARCH, DALLAS\)를 담는다. ...
* 위와 같이 **배열은 row의 갯수를 정해주어야한다. \(=방의 갯수\)** - data의 row수가 많거나 수정되어 늘어나고 줄어들고 하면 오류가 발생하거나 표시하기 어렵다. -&gt; **Vector**를 이용하여 보완한다.
* Vector : 데이터를 담는 꾸러미 - 담을수 있는 row의 갯수가 늘었다 줄었다 한다. data의 row수를 반영한다.
* 참고 : DeptApp.java

### SELECT

* 순서 1. **SELECT** deptno, dname, loc  - 컬럼명=변수, 변수, 변수 = 열거형 연산자 2. **FROM** dept  - dept=집합이름 3. **WHERE** deptno &gt;= 값  -  컬럼명, 비교연산자, UI에서 입력되는 값 \(WHERE=조건\)  - 값은 파라미터로 사용된다.
* 업무처리라는 것은 UI -&gt; 등록, 조 -&gt; java -&gt; insert -&gt; 목록 업데이트 , -&gt; SELECT -&gt; 응 이다. - insert : 등록되었습니다=true=1, 등록실패했습니다=false=0 - insert가 성공해야 목록으로 간다.
* 등록, 수정, 삭제와 같은 요청은 모두 조회를 거쳐야한다.
* SELECT문은 재사용을 위해 메서드로 꺼내야한다.
* java에서 SELECT문 코드를 작성하기 전에 Toad에서 단위테스트를 거치도록 한다. - 오류발생시 'orc-오류코드'로 검색하여 해결한다.

후기 : 좋은 개발자가 되기 위해서는 코드 작성 전에 계획을 그려보고, 테스트 하는것을 습관들여야한다. 익숙해지자!

