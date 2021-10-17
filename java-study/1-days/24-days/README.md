---
description: 2020.09.15 - 24일차
---

# 24 Days - PROCEDURE, 스칼라변수,카테시안 곱,refCursor, Vector, ArrayList

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261 : Oracle.com
* 사용Tool \
  \- Eclipse : Eclipse.org\
  \- Toad DBA Suite for Oracle 11.5

## 자바 - 전체 데이터 갱신

### DafaultTableModel method

| method |        파라미터        |
| :----: | :----------------: |
| addRow | Object\[ ] rowData |
| addRow |   Vector rowData   |

* List는 파라미터에 없다.

### Vector, & addRow

```java
	public void refreshData() {
		//조회나 혹은 입력 후에 해당 메서드가 연속해서 호출될 수 있으므로 
		//기존에 처리된 결과 화면을 초기화 해야한다.
		while(dtm.getRowCount()>0) {//초기화
			//이런 부분들이 html, 안드로이드 web, app에서 활용된다.
			dtm.removeRow(0);
		}
		//연결
		List<BookVO> bList = handler.getBookList();
		if(bList!=null && bList.size() >0) {
			for(int i=0;i<bList.size();i++) {//dtm에 row순서대로 add하기
				Vector oneRow = new Vector();
				//벡터는 타입을 가리지 않기때문에 바로 값을 받을 수 있다.
				BookVO bVO = bList.get(i);
				oneRow.add(0, bVO.getno());
				oneRow.add(1, bVO.getname());
				oneRow.add(2, bVO.getAuthor());
				oneRow.add(3, bVO.getPublish());
				dtm.addRow(oneRow);
			}
		}else {
			JOptionPane.showMessageDialog(this, "데이터가 없습니다."
			                 ,"Warning", JOptionPane.ERROR_MESSAGE);
			return;
		}
	}
```

* vector와 addrow를 이용한 페이지 새로고침 메서드\
  \- 상세보기는 필요없지만 수정, 입력 후에는 메인 페이지가 새로고침 되어야한다.\
  \- 테이블에 띄워져있던 데이터는 모두 삭제한다.
* 4-7번 : dtm에 로우가 있는 동안 삭제
* 9번 : BookVO타입 List에 eventHandler에서 DB에서 데이터를 가져오는  메서드를 호출한다.
* 10-20번 : 리스트에 값이 담겨있으면 Vector에 값을 순서대로 add해서 dtm에 addRow한다.

### ArrayList

```java
	public List<BookVO> getBookList() {
		StringBuilder sql = new StringBuilder();
		List<BookVO> bList = new ArrayList<>();
		sql.append("SELECT b_no, b_name, author, b_info, publish FROM book2020");
		try {
			con = dbMgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			rs = pstmt.executeQuery();
			BookVO bVO = null;
			while(rs.next()) {
				bVO = new BookVO();
				bVO.setno(rs.getInt("b_no"));
				bVO.setname(rs.getString("b_name"));
				bVO.setAuthor(rs.getString("author"));
				bVO.setinfo(rs.getString("b_info"));
				bVO.setPublish(rs.getString("publish"));
				bList.add(bVO);
			}			
		} catch (SQLException se) {//이 그물에 걸리는 예외는 모두 toad에서 잡히는 에러이다.
			System.out.println("[[query]]"+sql.toString());
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return bList;
	}
```

* ArrayList를 이용해 DB데이터를 받아오는 메서드

### Object\[ ] & addRow

```java
public void reData() {
      while(dtm_book.getRowCount()>0) {
         dtm_book.removeRow(0);
      }
      List<BookVO> bList = beh.getBookList();
      if(bList!=null && bList.size()>0) {
         for(int i=0;i<bList.size();i++) {
            //Vector oneRow = new Vector();
            int x= 0;
            Object[] oneRow = new Object[bList.size()];
            BookVO bVO = bList.get(i);
            oneRow[x]  = bVO.getB_no();
            oneRow[x+1]= bVO.getName();
            oneRow[x+2]= bVO.getAuthor();
            oneRow[x+3]= bVO.getPublish();
            oneRow[x+4]= bVO.getInfo();
            dtm_book.addRow(oneRow);
         }
      }
   }
```

* Object타입 배열을 이용해 dtm에 addRow하는 메서드

## Toad & 자바

### PL/SQL

* Procedural Language
* SQL을 확장한 절차적 언어로, 관계형 DB에서 PROCEDURE를 사용하는 언어이다.\
  \- SQL에서 사용할 수 없는 절차적 프로그래밍 기능을 갖는다.
* **기본구조**\
  **DECLARE(선언 예약어)** : 변수, 상수, 커서 선언\
  **BEGIN(실행부)** : SQL을 절차적 형식으로 실행할 수 있는 절차적 언어인 제어문, 반복문, 함수 등의 로직\
  **EXCEPTION(예외처리부) **\
  **END;(실행문 종료)**\
  **- **DECLARE절에서 변수 선언이 가능하고 BEGIN 실행문 안에서 초기화 할 수 있다.
* 프로그램 종류\
  \- **Procedure** : 리턴값을 n개 갖는 프로그램\
  \- **Function **   : 리턴 값을 반드시 반환하는 프로그램\
  \- **Trigger**       : 지정 이벤트 발생시 자동으로 실행되는 블록
* 블록 단위로 실행된다, :  BEGIN 실행문 END;
* 인자(변수)를 가질 수 있고, IF, LOOP제어문을 사용할 수 있으며, 예외처리가 가능하다.
* 문장의 종료부분에 ;세미콜론을 사용해서 문장의 종료를 알려주어야한다.
* 쿼리문 수행을 위해 / 입력이 있어야하며 블록 종결을 의미하기도한다.
* SQL : DML, Query,  TCL 사용가능하다.

### PROCEDURE

* 동작을 일괄처리하는 기능을 한다.
* 파라미터를 사용할 수 있고 속도가 빠르다.
* 기본 구조\
  **CREATE OR REPLACE PROCEDURE 프로시저명(생성)**\
  **IS 변수명 변수,타입 선언(IN, OUT)**\
  **BEGIN 실행문;**\
  **END;(종료)**
* 파라미터의 IN은 사용자에게서 수집하는 값, OUT은 반환값이다.
* 출력문\
  **variable 변수이름 타입**\
  **exec 프로시저명 (:변수이름)**\
  **print 변수이름**
* 출력문의 변수는 프로시저의 OUT 변수와 타입이 같아야만한다.
* 프로시저 안에서 프로시저를 호출 할 수도 있다.

### 스칼라변수

### 카테시안 곱

![카테시안 곱 예시](<../../../.gitbook/assets/1 (12).png>)

![카타시안 곱 예시2](<../../../.gitbook/assets/.png (6).png>)

* 데이터를 2배, 3배, 4배수로 확장, 복제할때 사용된다.
* 양 쪽 테이블의 모든 행을 서로 연결한다.
* 모든 경우의 수를 보여준다. \
  \- 묻지마 조인\
  \- 조건이 없다\
  \- 의미없는 값이 사용될 수 있다.

{% content-ref url="cmd-and-toad.md" %}
[cmd-and-toad.md](cmd-and-toad.md)
{% endcontent-ref %}

{% content-ref url="myproc.java.md" %}
[myproc.java.md](myproc.java.md)
{% endcontent-ref %}



후기 :  영화예매프로젝트가 완성되었다. 만족스러워!!

{% content-ref url="../undefined-3.md" %}
[undefined-3.md](../undefined-3.md)
{% endcontent-ref %}

