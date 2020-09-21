# java - Update, Delete

## Update

### 수정 - actionPerformed

```java
public void actionPerformed(ActionEvent e) {		
	String label = e.getActionCommand();//입력 or수정 or상세보기가 들어옴
		
		else if("수정".equals(label)) {
		System.out.println("수정하실건가요?");
		BookVO rbVO = null;
		int pb_no = 0;
		int indexs[] = bm.jtb.getSelectedRows();
		//하나도 선택하지 않는다면
		if(indexs.length == 0) {
			JOptionPane.showMessageDialog(bm, "수정할 목록을 선택하세요.");
			return;//0이면 if문 종료
		}
		//하나도 선택하지 않는다면
		else if(indexs.length > 1) {
			JOptionPane.showMessageDialog(bm, "목록을 한개만 선택하세요.");
			return;//1이면 if문 종료
		}
		else {
			//초기값 0을 선택한 로우의 b_no값으로 변경 처리해야한다.
			//배열이므로 for문 사용
			for(int i=0;i<indexs.length;i++) {
				pb_no = indexs[0]; // 0번째줄, 1번째줄 이것만 알려준다.=로우의 순번
				//하지만 우리가 필요한 WHERE 조건은 b_no=도서번호 이므로 밑의 조건이 추가되어야한다.
				pb_no = Integer.parseInt(bm.dtm.getValueAt(pb_no, 0).toString());
			}
		}
		rbVO = getBookDetail(pb_no);//초기값은 0이지만 1개 선택을 했다면 값을 갖는다.
		rbVO.setno(pb_no);
		bm.bookCRUD.set("수정", rbVO, bm, true);
		bm.bookCRUD.setVisible(true);
	}
```

### Update SQL

```java
public int bookUpdate(BookVO pbVO) {
			int result = 0;
			StringBuilder sql = new StringBuilder();
			try {
				sql.append("UPDATE book2020 SET b_name=?, author=?, publish=?, b_info=? WHERE b_no=?");
				con = dbMgr.getConnection();
				pstmt = con.prepareStatement(sql.toString());
				//?가 있으므로 먼저 쿼리문을 스캔한다.
				int i = 1;
				pstmt.setString(i++, pbVO.getname());
				pstmt.setString(i++, pbVO.getAuthor());
				pstmt.setString(i++, pbVO.getPublish());
				pstmt.setString(i++, pbVO.getinfo());
				pstmt.setInt(i++, pbVO.getno());
				result = pstmt.executeUpdate();
				
			} catch (SQLException se) {
				System.out.println(se.toString());
			} catch (Exception e) {
				System.out.println(e.toString());
			}
			return result;
		}
```

### **수정화면 - jbtn\_save actionperFormed**

```java
else if(obj == jbtn_save) {
			  //수정시 : 수정은 DB에서 값을 조회해 와서 화면에 표시해주어야한다.
			  else if(bVO != null) {
				  BookVO pbVO = new BookVO();
				  pbVO.setno(bVO.getno());
				  pbVO.setname(getName());
				  pbVO.setAuthor(getAuthor());
				  pbVO.setPublish(getPublish());
				  pbVO.setinfo(getInfo());
				  //pbVO.setimg(get);
				  int result = 0;
				  System.out.println(pbVO.getno()+","+pbVO.getname()+","+pbVO.getAuthor()+","+pbVO.getPublish());
				  result = bm.handler.bookUpdate(pbVO);//insert성공여부
				  System.out.println("result = "+result);
				  this.dispose();
				  if(result ==1) {//insert성공
					  bm.refreshData();
				  }
			  }
```

## Delete

### 삭제 - actionPerformed

```java
public void actionPerformed(ActionEvent e) {		
		String label = e.getActionCommand();//입력 or수정 or상세보기가 들어옴
		
		else if("삭제".equals(label)) {
			JOptionPane.showMessageDialog(bm, "삭제하시겠습니까?");
			int status = 0;//삭제 유무를 담을 값
			int indexs[] = bm.jtb.getSelectedRows();//선택 값을 int배열에 담는다.
			//하나도 선택하지 않는다면
			if(indexs.length == 0) {
				JOptionPane.showMessageDialog(bm, "수정할 목록을 선택하세요.");
				return;//0이면 if문 종료
			}				
			else {
				//테이블에서 선택된 row의 b_no에 대해 삭제 요청을 한다.
				//배열이므로 for문 사용
				for(int i=0;i<bm.dtm.getRowCount();i++) {//row의 갯수만큼
					if(bm.jtb.isRowSelected(i)) {//Jtable에서 선택된 row
						//선택된 row의 번호 가져오기
						Integer b_no = (Integer)bm.dtm.getValueAt(i, 0);
						status = bookDelete(b_no);//초기값은 0이지만 1개 선택을 했다면 값을 갖는다.
						bm.refreshData();
					}
				}
			}//end of else
		}//end of else
```

### Delete SQL

```java
public int bookDelete(int b_no) {
		int result = 0;
		StringBuilder sql = new StringBuilder();
		sql.append("DELETE FROM book2020 WHERE b_no=?");
		BookVO bVO = null;//NullPointerException이 발생할 수 있다.
		try {
			con = dbMgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			pstmt.setInt(1,  b_no);
			result = pstmt.executeUpdate();
			
		} catch (SQLException se) {//이 그물에 걸리는 에외는 모두 toad에서 잡히는 에러이다.
			System.out.println("[[query]]"+sql.toString());
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return result;
	}
```

