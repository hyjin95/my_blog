---
description: 2020.09.15 - 24일차
---

# 24 Days - 프로시저, 스칼라변수,카테시안 곱,refCursor, Vector, ArrayList

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261 : Oracle.com
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5

## 전체 데이터 갱신

### DafaultTableModel method

| method | 파라미터 |
| :---: | :---: |
| addRow | Object\[ \] rowData |
| addRow | Vector rowData |

* List는 파라미터에 없다.

### Vector, & addRow

```java
	public void refreshData() {
		//조회나 혹은 입력 후에 해당 메서드가 연속해서 호출될 수 있으므로 기존에 처리된 결과 화면을 초기화 해야한다.
		//이미 테이블에 보여지는 테이터가 있는 경우 모두 삭제한다.
		while(dtm.getRowCount()>0) {//초기화
			//이런 부분들이 html, 안드로이드 web, app에서 활용된다.
			dtm.removeRow(0);
		}
		//연결
		List<BookVO> bList = handler.getBookList();
		if(bList!=null && bList.size() >0) {
			for(int i=0;i<bList.size();i++) {//dtm에 row순서대로 add하기
				Vector oneRow = new Vector();//벡터는 타입을 가리지 않기때문에 바로 값을 받을 수 있다.
				BookVO bVO = bList.get(i);
				oneRow.add(0, bVO.getno());
				oneRow.add(1, bVO.getname());
				oneRow.add(2, bVO.getAuthor());
				oneRow.add(3, bVO.getPublish());
				dtm.addRow(oneRow);
			}
		}else {
			JOptionPane.showMessageDialog(this, "데이터가 없습니다.","Warning", JOptionPane.ERROR_MESSAGE);
			return;
		}
	}
```

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
			
		} catch (SQLException se) {//이 그물에 걸리는 에외는 모두 toad에서 잡히는 에러이다.
			System.out.println("[[query]]"+sql.toString());
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return bList;
	}
```

### Object\[ \] & addRow

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

