# java 파라미터 활용

### 파라미터(String타입, VO타입, 클래스타입, 참조형타입)

```java
//가져온 값 bVO, 파라미터로 넘기는 pbVO, 리턴된 값을 담을 rbVO
//삭제 : BookManager 수정,입력 : BookCURD sql메서드에 클래스,List를 넘겨야한다.
//title이 변경되어야한다. 상세보기,수정은 SELECT가 진행되어 값을 담아야한다. 부모창의 주소번지. 입력,수정은 true 상세보기는 false.
public void set(String title, BookVO bVO, BookManager bm, boolean enable) {
	//JOptionPane.showMessageDialog(this, "title : "+title);//단위테스트
	this.bVO = bVO;//상세보기와 수정의 경우 1건을 먼저 조회해야한다면 필요하다.
	this.bm = bm;
	this.setTitle(title);
	this.setEditable(enable);
} 
```

* set메서드는 부모 창에서 결정된 내용을 팝업창에서 사용하기 위한 메서드이다.
* 생성자가 아니지만 파라미터에 부모의 주소번지를 받는다.
* String title : 사용자가 선택하는 메뉴에 따라 팝업창의 title이 달라져야한다.
* BookVO bVO : sql문의 반환값을 담는다.
* BookManager bm : 수정, 입력이 성공하면 팝업창은 닫아주고 부모창을 새로고침 처리 하기 위해 부모의 주소번지를 담는다.
* boolean enable : 상세보기 창은 입력, 수정이 불가능 해야하므로, textField를 비활성화해야한다.

### 참조형 타입을 파라미터로 담아 실행되는 메서드

```java
//입력, 수정시에는 컬럼값을 수정 가능하도록 하고 상세조회시에는 불가능하게 셋팅하는 메서드 구현
public void setEditable(boolean e) {
	jtf_name.setEditable(e);//제목 입력창
	jtf_author.setEditable(e);//작가 
	jtf_publish.setEditable(e);//출판사 입력창
	jta_info.setEditable(e);//정보 입력창
	jtf_file.setEditable(e);//파일 
}
```

* 위 set메서드에서 textField를 비활성화 하기 위한 메서드이다.
* 파라미터로 boolean타입 e를 갖는다.\
  \- 입력, 수정의 경우에는 true : 활성화\
  \- 상세보기의 경우에는 false : 비활성화
