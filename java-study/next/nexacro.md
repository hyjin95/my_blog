# Nexacro

### Button : Properties

* id : button의 key값 - 중복설정 불가능
* text : button에 설정할 text값

### Button : Event

* Propertied상단의 메뉴중 네번째 이벤트 탭
* 만든 버튼을 더블클릭하면 script에 자동으로 클릭이벤트가 생긴다. 또는 event탭에 원하는 이벤트의 입력칸을 더블 클릭하면 자동 이벤트 생성된다.

### API : F1, Help&gt;Help

* 메뉴 중 화면을 그리는 도구들 : Components
* 속성 = property

### 화면테스트

* 메뉴 가장 오른쪽의 QuickView

### Script : this

* this : 현재 Form 주소 - this.Button\_onClick : 이 form에 있는 함수

### combobox

* html : select 
* innerdata를 넣고 해당 combobox를 복사하면 해당 데이터도 똑같이 가져온다. 복사한 이후의 데이터값의 변경은 적용되지 않는다.
* 데이터값의 변경도 같이 적용하려면? - component의 dataset을 활용한다.

### dataset

* 컴포넌트 선택후 화면을 클릭하면 invisible Object에 추가된 것을 확인할 수 있다.
* 만들어진 dataset을 더블클릭하면 데이터를 편집할 수 있다. 상단 창은 컬럼, 하단 창은 row
* invisible Object의 dataset을 드래그해 combobox로 가져가면 innerdata에서 만든 dataset을 선택할 수 있다.
* dataset을 갖는 객체를 복사하고 dataset을 편집하면 같이 변경 적용된다.
* 다른 From으로 combobox를 복사하려면 dataset도 같이 복사해 form에 옮겨야한다. dataset이 분리되었으므로 앞의 dataset의 변경은 적용되지 않는다.

