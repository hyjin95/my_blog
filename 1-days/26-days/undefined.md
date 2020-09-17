# 도서관리 -

## BookManagerEventHandler.java

### 입력 SQL 메서드

```java
//새로운 도서 정보 등록하기 구현
	public int bookInsert(BookVO pbVO) {
		int result = 0;
		StringBuilder sql = new StringBuilder();
		try {
			sql.append(" INSERT INTO book2020 SELECT seq_book_no.nextval, ?, ?, ?, ?, ? FROM dual");
			con = dbMgr.getConnection();
			pstmt = con.prepareStatement(sql.toString());
			//?가 있으므로 먼저 쿼리문을 스캔한다.
			int i = 1;
			pstmt.setString(i++, pbVO.getname());
			pstmt.setString(i++, pbVO.getAuthor());
			pstmt.setString(i++, pbVO.getPublish());
			pstmt.setString(i++, pbVO.getinfo());
			pstmt.setString(i++, pbVO.getimg());		
			result = pstmt.executeUpdate();
			
		} catch (SQLException se) {
			System.out.println(se.toString());
		} catch (Exception e) {
			System.out.println(e.toString());
		}
		return result;
	}
```

### 입력 & 수정 actionPerformed 메서드

```java
@Override
	public void actionPerformed(ActionEvent e) {
		
		String label = e.getActionCommand();//입력 or수정 or상세보기가 들어옴
		System.out.println("lable = "+label);//단위테스트, 잘 가져오는지
		
		if("입력".equals(label)) {//글자타입 비교는 .equals
			System.out.println("입력하시겠습니까?");//단위테스트
			bm.bookCRUD.set("입력", null, bm, true);
			bm.bookCRUD.setVisible(true);
		}
		else if("수정".equals(label)) {
			System.out.println("수정하실건가요?");
			BookVO rbVO = new BookVO();
			rbVO.setAuthor("남궁성");
			rbVO.setPublish("글레이즈드 도넛");			
			bm.bookCRUD.set("수정", rbVO, bm, true);
			bm.bookCRUD.setVisible(true);
		}					
	}//end of actionPerformed
```

## BookCRUD.java

### 선택 메뉴에 따른 값 받아오는 메서드

```java
public void set(String title, BookVO bVO, BookManager bm, boolean enable) {
		  //JOptionPane.showMessageDialog(this, "title : "+title);//단위테스트
		  this.bVO = bVO;//상세보기와 수정의 경우 조회가 먼저 일어나야 하므로 필요하다.
		  setValue(bVO);
		  this.bm = bm;//수정과 입력이 성공하면 팝업창은 닫야주고 부모창을 새로고침처리 해야하므로 부모창의 주소번지가 필요하다.
		  this.setTitle(title);
		  this.setEditable(enable);
	  }
```

### 선택 메뉴에 따른 화면 셋팅

```java
//수정하기와 상세보기 처리시 오라클 서버를 먼저 경유해야하므로 rbVO는 null이 아닐 것이고,
	//set메서드의 파라미터로 넘어온 값을 이용해 화면을 초기화해야한다.
	public void setValue(BookVO rbVO) {
		//사용자가 입력을 선택한 경우를 위한 화면 설정 : 모든 값을 빈 문자열로 셋팅한다.
		if(bVO==null) {//입력 선택
			setName("");
			setAuthor("");
			setPublish("");
			setInfo("");
		}
		//상세 조회 혹은 수정 시에는 파라미터로 받은 값을 셋팅한다.
		else {
			setName(rbVO.getname());
			setAuthor(rbVO.getAuthor());
			setPublish(rbVO.getPublish());
			setInfo(rbVO.getinfo());			
		}
	}//end of setValue
```

### 파일찾기 버튼 actionPerformed

```java
//입력, 수정이 끝난 후에는 전체 조회를 통해 부모창을 새로고침 해야한다.
	@Override
	public void actionPerformed(ActionEvent e) {
		Object obj = e.getSource();
		  if(obj == jbtn_file) {
			  //열기 대화창의 기본 경로 설정
			  jfc.setCurrentDirectory(new File("C:\\workspace_java\\dev_java\\src\\design\\image"));
			  //열기 대화창을 오픈
			  int inRet = jfc.showOpenDialog(BookCRUD.this);
			  //yes 혹은 ok 버튼이 눌린 거야
			  if(inRet == JFileChooser.APPROVE_OPTION) {
				  //파일 여는 처리를 수행한다. - 항상 예외상황이 발생할 수 있으므로 try-catch해주어야한다.
				  try {
					System.out.println("선택했을 경우...");
					//읽어들이기 용 String 객체 선언
					String strLine = null;
					//FileChooser로 선택된 파일을 File객체에 대입
					File myFile = jfc.getSelectedFile();
					//선택된 파일의 절대경로를 지정하여 BufferReader 객체를 작성
					BufferedReader br = new BufferedReader(new FileReader(myFile.getAbsolutePath()));
					System.out.println(myFile.getAbsolutePath());
					
					//파일찾기에서 가져온 주소로 JLable에 도서 이미지 출력하기
					jtf_file.setText(myFile.getAbsolutePath());
					//이미지 미리보기 시작 처리
					ImageIcon originIcon = new ImageIcon(myFile.getAbsolutePath());
					//ImageIcon에서 Image 추출 하기
					Image originImg = originIcon.getImage();
					//추출된 이미지의 크기를 조절하여 새로운 Image 객체 생성
					Image changeImg = originImg.getScaledInstance(300, 380, Image.SCALE_SMOOTH);
					//새로운 Image로 ImageIcon객체 생성
					ImageIcon icon = new ImageIcon(changeImg);
					jlb_img.setIcon(icon);
					//revalidate는 새 구성 요소가 추가되거나 이전 요소가 제거되면 컨테이너에서 호출된다.
					//이 호출은 레이아웃 관리자에게 새 구성 요소 목록을 기반으로 재설정 하도록 지시하는 명령이다.
					cont.revalidate();
				} catch (Exception e2) {
					// TODO: handle exception
				}
			  }//end of if
		  }//end of if		  
```

### 닫기 버튼 actionPerformed

```java
		  else if(obj == jbtn_close) {
			  this.dispose();
		  }		 
```

### 입력 메뉴 actionPerformed

```java
		  else if(obj == jbtn_save) {
			  //입력시 : 새로 입력은 DB에서 값을 조회할 필요없이 바로 화면에서 입력한다.
			  if(bVO == null) {
				  BookVO pbVO = new BookVO();
				  pbVO.setname(getName());
				  pbVO.setAuthor(getAuthor());
				  pbVO.setPublish(getPublish());
				  pbVO.setinfo(getInfo());
				  //pbVO.setimg(get);
				  int result = 0;
				  result = bm.handler.bookInsert(pbVO);//insert성공여부
				  System.out.println("result = "+result);
				  this.dispose();
				  if(result ==1) {//insert성공
					  bm.refreshData();
				  }
			  }	
```

### 수정 메뉴 actionPerformed

```java
			  //수정시 : 수정은 DB에서 값을 조회해 와서 화면에 표시해주어야한다.
			  else if(bVO != null) {
			  }
		  }
	}//end of acrionPerformed	 
```

