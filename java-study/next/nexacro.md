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
* dataset을 공유하려면 Application을 활용한다.

### Application

* project Explorer에서 Application Information의 Application Variables의 Datasets을 클릭하면 멤버\(Global\) dataset을 생성할 수 있다.
* 수업 자료중에는 com\_code테이블을 이렇게 사용하는 경우에 해당한다. 굳이 DB를 통할 필요없이 UI에서 dataset을 처리한다.

### Component

* Edit = input
* MaskEdit = 정해진 규격값만 입력되는 input - number형과 Strng형으로 구분된다. - F1에서 MaskEdit &gt; Property &gt; format 을 살펴본다.
* 위 두 edit는 생김새가 같다. 구분하려면 Static을 사용한다.
* Static = label - value를 설정할 수 없다. text속성만 갖는다.

### 정렬

* View &gt; Toolbar &gt; align 메뉴바가 추가된다. 정렬을 적용할 컴포넌트들을 드래그하면 활성화 된다.
* n개를 선택하고 Height를 적용하면 모두 크기가 같아진다. shift를 누르고 1개 컴포넌트를 선택하면 기준이 되어 해당 컴포넌트 크기에 맞춰진다.

### MaskEdit : format

* \#\#.\#\# - 0 입력시 값이 보이지 않음 - 0.1 입력시 .1
* 99.99 - 0 입력시 0 - 0.1 입력시 .1
* 00.00 - 사용자가 지정한 모든 자릿수가 비어있어도 0으로 표기됨   1 입력시 01.00 - 입력값이 없거나 입력값이 0 이면 00.00 으로 표시 - 입력칸에 마우스 포커스되면 표시는 사라짐
* 9,999 - 원 단위 표기 - 네 자리수가 넘어가도 ,가 단위마다 표기된다.   value값에는 적용되지 않고 data는 number로 들어간다. 

