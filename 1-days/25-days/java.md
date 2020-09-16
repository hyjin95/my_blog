# java 파라미터 활용

```java
//부모 창에서 결정된 내용을 팝업창에서 사용하기 위한 메서드 선언하기
//생성자가 아니라 메서드로 주소번지를 받는다.
//가져온 값 bVO, 파라미터로 넘기는 pbVO, 리턴된 값을 담을 rbVO
//삭제 : BookManager 수정,입력 : BookCURD sql메서드에 클래스,List를 넘겨야한다.
//title이 변경되어야한다. 상세보기,수정은 SELECT가 진행되어 값을 담아야한다. 부모창의 주소번지. 입력,수정은 true 상세보기는 false.
public void set(String title, BookVO bVO, BookManager bm, boolean enable) {
	//JOptionPane.showMessageDialog(this, "title : "+title);//단위테스트
	this.bVO = bVO;//상세보기와 수정의 경우 1건을 먼저 조회해야한다면 필요하다.
	this.bm = bm;//수정과 입력이 성공하면 팝업창은 닫야주고 부모창을 새로고침처리 해야하므로 부모창의 주소번지가 필요하다.
	this.setTitle(title);
	this.setEditable(enable);
} 
```

```java
//입력, 수정시에는 컬럼값을 수정 가능하도록 하고 상세조회시에는 불가능하게 셋팅하는 메서드 구현
public void setEditable(boolean e) {
	jtf_name.setEditable(e);
	jtf_author.setEditable(e);
	jtf_publish.setEditable(e);
	jta_info.setEditable(e);
	jtf_file.setEditable(e);
}
```

