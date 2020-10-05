# ZipCodeSearch - Map, List

```java
	//조회메서드
	public void refreshData(String user, String dong) {
		System.out.println("zdo : "+zdo+", dong : "+dong);
		DBConnectionMgr dbMgr  = DBConnectionMgr.getInstance();
		con = dbMgr.getConnection();
		StringBuilder sql = new StringBuilder();
		sql.append("SELECT zipcode, address");
		sql.append(" FROM zipcode_t");
		sql.append(" WHERE 1=1");
		if(zdo!=null && zdo.length()>0) {//비어있지 않고, 하나 이상 값이 있으면
			sql.append(" AND zdo=?");//WHERE절 다음에 붙으므로 AND절을 사용
		}
		if(dong!=null) {
			sql.append(" AND dong LIKE '%'||?||'%'");//전체검색
		}
		//물음표의 갯수가 달라지므로 변수처리한다.
		//물음표 자리는 배열이 아니므로 1부터이다.
		int i = 1;
		try {
			//파라미터로 넘어온 select문을 스캔 -> ?갯수를 파악한다.
			pstmt = con.prepareStatement(sql.toString());
			//위에서 물음표 자리에 들어갈 값을 파라미터로 받아서 설정한다.1=?갯수
			if(zdo!=null && zdo.length()>0) {//비어있지 않고, 하나 이상 값이 있으면
				sql.append(" AND zdo=?");
				pstmt.setString(i++, zdo);//콤보박스에서 가져온 값
			}
			if(dong!=null && dong.length()>0) {
				pstmt.setString(i++, dong);//텍스트 필드에서 가져온 값
			}
			rs = pstmt.executeQuery();
			//내 안에 있는 타입을 꺽쇠<>안에 직접 써주면 타입체크를 별도로 하지 않는다. = 제네릭
			//선언부에는 반드시 써야하고 생성부에서는 생략가능하다.
			//그러나 다이아몬드 연산자는 작성해준다 = 오른쪽항
			//Vector v = new Vector();
			Vector<Map<String,Object>> v = new Vector<>();//권장사항
			//List v2 = new Vector(); //같다, 선언부에 인터페이스 하지만 List는 copyinto를 쓸 수 없으므로 사용하지 않는다.
			Map<String,Object> rmap = null;
			while(rs.next()) {
				rmap = new HashMap<>();//Map은 오류발생한다.
				rmap.put("zipcode",rs.getInt("zipcode"));//숫자보다 문자열로 입력하는것이 직관적이다. 알아보기 편하다, 수정하기 좋다.
				rmap.put("address",rs.getString("address"));//숫자보다 문자열로 입력하는것이 직관적이다. 알아보기 편하다, 수정하기 좋다.
				v.add(rmap);
			}//end of while
			//질문 : 두 번 조회할 경우 앞에 조회결과가 남아있어요. 초기화하는 방법
			if(v.size() > 0) {
				while(dtm_zipcode.getRowCount()>0) {
					dtm_zipcode.removeRow(0);
				}
			}
			Iterator<Map<String,Object>> iter = v.iterator();//vector에서 값꺼내서 HashMap타입의 iter에게 담는다.
			Object keys[] = null;
			//조회결과가 한 건도 없는 경우의 화면처리
			if((v==null)||(v.size()<1)) {
				//if((v==null)||(v.size()<1))예방코드 작성
				JOptionPane.showMessageDialog(this, "조회결과가 없습니다.");
				return;
			}
			else {
				while(iter.hasNext()) {
					HashMap data = (HashMap)iter.next();
					keys = data.keySet().toArray();
					Vector<Object> oneRow = new Vector<>();
					oneRow.add(0,data.get(keys[0]));//1번 key값은 zipcode
					oneRow.add(1,data.get(keys[1]));//2번 key값은 address
					dtm_zipcode.addRow(oneRow);
				}
			}				
		}catch(SQLException se) {
			System.out.println(se.toString());
			System.out.println("[[query]] == "+sql.toString());//toad에 돌려서 오타찾기
		}catch(Exception e) {
			System.out.println(e.toString());
		}//end of try-catch		
	}
```



