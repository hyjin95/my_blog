# java - 상세보기SQL

```java
public BookVO getBookDetail(int b_no) {
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT b_no, b_name, author, b_info, publish FROM book2020");
		sql.append(" WHERE b_no=?");
		BookVO bVO = null;//NullPointerException이 발생할 수 있다.
		try {
			con = dbMgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			pstmt.setInt(1,  b_no);
			rs = pstmt.executeQuery();
			if(rs.next()) {
				bVO = new BookVO();
				bVO.setno(rs.getInt("b_no"));
				bVO.setname(rs.getString("b_name"));
				bVO.setAuthor(rs.getString("author"));
				bVO.setinfo(rs.getString("b_info"));
				bVO.setPublish(rs.getString("publish"));
			}
			else {
				bVO = new BookVO();
			}
		} catch (SQLException se) {//이 그물에 걸리는 에외는 모두 toad에서 잡히는 에러이다.
			System.out.println("[[query]]"+sql.toString());
		} catch (Exception e) {//그 외 나머지가 잡힌다.
			e.printStackTrace();//stack영역에 쌓여있는 에러 메세지의 이력을 라인번호와 함께 출력
		}
		return bVO;
	}
```

```java
else if("상세보기".equals(label)) {
			System.out.println("상세정보를 보여드릴까요?");
			BookVO rbVO = null;
			int indexs[] = bm.jtb.getSelectedRows();//선택 값을 int배열에 담는다.
			int pb_no = 0;//파라미터로 넘길 값
			//하나도 선택하지 않는다면
			if(indexs.length == 0) {
				JOptionPane.showMessageDialog(bm, "상세보기할 목록을 선택하세요.");
				return;//0이면 if문 종료
			}
			//하나도 선택하지 않는다면
			else if(indexs.length > 1) {
				JOptionPane.showMessageDialog(bm, "목록을 한개만 선택하세요.");
				return;//1이면 if문 종료
			}
			else {
				//선택을 했다면 b_no값을 pb_no에 초기화한다.
				//배열이므로 for문 사용
				for(int i=0;i<indexs.length;i++) {
					pb_no = indexs[0]; // 0번째줄, 1번째줄 이것만 알려준다.=로우의 순번
					//하지만 우리가 필요한 WHERE 조건은 b_no=도서번호 이므로 밑의 조건이 추가되어야한다.
					pb_no = Integer.parseInt(bm.dtm.getValueAt(pb_no, 0).toString());
				}
			}
			rbVO = getBookDetail(pb_no);//초기값은 0이지만 1개 선택을 했다면 값을 갖는다.
			JOptionPane.showMessageDialog(bm, "조회된 결과 : "+rbVO);//단위테스트
			bm.bookCRUD.set("상세보기", rbVO, bm, false);
			bm.bookCRUD.setVisible(true);

		}
```

