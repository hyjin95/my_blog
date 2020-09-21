# java - Update, Delete

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

