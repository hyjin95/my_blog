# ZipCodeSearch - Map, List

## refreshData메서드 수정

### 선언부

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
```

### 접속 요청

```java
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
```

### Vector에 Map담기

```java
			//내 안의 타입을 꺽쇠<>안에 써주면 타입체크를 별도로 하지 않는다. = 제네릭			
			Vector<Map<String,Object>> v = new Vector<>();//권장사항
			Map<String,Object> rmap = null;
			while(rs.next()) {
				rmap = new HashMap<>();//Map은 오류발생한다.
				rmap.put("zipcode",rs.getInt("zipcode"));
				rmap.put("address",rs.getString("address")); 
				v.add(rmap);
			}//end of while
			//질문 : 두 번 조회할 경우 앞에 조회결과가 남아있어요. 초기화하는 방법
			if(v.size() > 0) {
				while(dtm_zipcode.getRowCount()>0) {
					dtm_zipcode.removeRow(0);
				}
			}
```

* 2번 : n개의 Map을 담기위해 Map타입 Vector를 선언, 생성한다.\
  \- Map의 key값은 String타입, value는 Object타입
* 3번 : key를 이용한 빠른 검색을 하기위해 Map을 선언, 생성한다.\
  \- Map의 key값은 String타입, value는 Object타입
* 5번 : Map은 인터페이스 이므로 구현체 클래스인 HashMap(싱글스레드용)으로 생성한다.\
  \- 검색마다 새로운 map으로 생성되어야 하므로 생성을 while문 안에서 한다.
* 6,7번 : 생성한 Map에 put함수를 이용해 key와 value를 담는다.\
  \- value가 Object타입이므로 어떤 타입이든 담을 수 있다.
* 8번 : Map타입으로 생성해둔 Vector에 add함수를 이용해 담는다.

### 꺼낸 값 조회, 출력

```java
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

* 자료구조에서 값을 꺼내주는 Iterator인터페이스를 사용한다.
* 1번 : Vector에서 iterator함수로 값을 꺼내 Map타입의 Iterator인터페이스 변수에 담는다.
* 4번 : 조회결과가 한 건도 없다면 알림창을 띄운다.\
  \- 반드시 return;예약어를 넣어 if문을 탈출한다.
* 10번 : iterator인터페이스가 제공하는 hasNext함수를 이용해 값을 꺼내는 동안
* 11번 : HashMap타입의 data변수에 출력되는 값을 담는다. \
  \- (HashMap)캐스팅 연산자 사용
* 12번 : 생성해둔 keys배열에 위의 data변수안의 key값을 가져와 toArray함수를 이용해 담는다.
* 13번 : dtm에 row를 순서대로 입력하기 위해 Object타입의 Vector를 선언, 생성한다.
* 14번 : 생성한 Vector의 0번방에 HashMap타입 data변수에서 \[0]번 key값으로 검색한 value를 담는다.
* 15번 : Vector의 1번방에 HashMap타입 data변수에서 keys의 \[1]번방 값으로 검색한 value를 담는다.

