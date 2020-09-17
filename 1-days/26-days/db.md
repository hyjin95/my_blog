# DB 자원반납

### Connection 메서드

```java
public Connection getConnection() {
		try {//실행시에 에러발생할 가능성이있는 코드는 try-catch로 예외처리한다.
			//제조사의 정보를 수직하는 클래스 이므로 자원반납 대상이 아니다.
			Class.forName(_DRIVER);
			//생성 역순으로 자원반납이므로 첫번째 선언된 con을 제일 마지막에 반납한다.
			con = DriverManager.getConnection(_URL, _USER, _PW);//연결 호출
		} catch (ClassNotFoundException ce) {
			System.out.println("드라이버 클래스를 찾을 수 없습니다.");
		} catch (Exception e) {
			System.out.println("연결 실패 "+e.toString());
		}
		//return null;//NullPointerException 오류
		return con;//return 반환타입이 인터페이스이다.
	}
```

* Connection 클래스를 참조해서 생성한 DB연결 메서드이다.

### 기본 자원반납

```java
//사용 자원 반납 - 자바 성능 튜닝팀의 권장사항, 명시적으로 처리하는 코드
	//서버의 입장에서는 사용한 자원은 반드시 반납해야한다.
	//자바에서는 가비지컬렉터(gc)가 살고 있어서 사용한 자원을 스레드를 활용한 쓰레기 값 청소 스케줄이 따라 청소해준다.
	//그러나 스레드 스케줄을 기다리는 시간이 있으므로 동시 접속자를 관리하는 서버제품들의 언어들은 모두 명시적으로 삭제요청해야한다.
	//가비지 값을 삭제 해주세요. -> System.gc(); //쓰레기 통을 비워주세요
	//쓰레기값이 결정되는 때 -> 정의한 객체에 대해 초기화를 하고, 객체가 다시 생성될때, 생성직전에 쓰레기 값이 된다. = Candidate상태
	//생성한 역순으로 한다.
	//순서 : Connection - PreparedStatment - ResultSet 
	public void freeConnection(Connection con
								, PreparedStatement pstmt
								, ResultSet rs) {
		if(rs!=null) {
			try {
				rs.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());
			}
		}//end of if
		if(pstmt!=null) {
			try {
				pstmt.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());
			}
		}//end of if
		if(con!=null) {
			try {
				con.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());			
			}
		}//end of if		
	}//end of freeConnection
```

### 입력, 수정, 삭제 자원반납

```java
//입력, 수정, 삭제 구현 후 자원 반납하기
	public void freeConnection(Connection con
			, PreparedStatement pstmt) {	
		if(pstmt!=null) {
			try {
				pstmt.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());
			}
		}//end of if
		if(con!=null) {
			try {
				con.close();
			}catch (Exception e) {
				System.out.println("Exception ---> "+e.toString());			
			}
		}//end of if		
	}//end of freeConnection
```

* 값을 받아올 필요가 없으므로 ResultSet클래스는 사용하지도, 반납할 필요도 없다.

