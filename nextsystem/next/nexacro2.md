# Nexacro2

### Grid

* dataset을 드래그해 보여줄 수 있다.
* 더블 클릭시 Gird Content Editor

### component와 dataset연동

![1](<../../.gitbook/assets/1 (120).png>)

* component 의 property의 Binable의 Value속성에서 dataset의 컬럼과 연동 할 수 있다.
* 이렇게 지정한 값이 compnent에 보이게 되고 component에서 수정한 값은 바로 Gird에서 바뀌는 것을 확인할 수 있다.
* 하지만 Edit Component와 다르게 MaskEdit나 다른 컴포넌트들은 기본값이 보이지 않는다.

### Component - type, format 변환 : Properties

![2](<../../.gitbook/assets/2 (94).png>)

* 컴포넌트의 Properties에서 format과 type을 확인한다.
* empl_id\
  \- format : a###\
  \- type : stirng
* married\
  \- falsevalue : 0\
  \- truevalue : 1
* salary\
  \- format : 9,999
* gender\
  \- Binding\
    innerdataset : innerdataset\
    code 컬럼 : W, M\
    data 컬럼 : 남자, 여자\
  \- direction : vertical\
  \- shift를 누르면 간격 조절이 가능하다.
* dept_id_\
  _- Binding_\
  __  innerdata : ds_dept

### Grid Content Editor : Component와 맞추기

![3](<../../.gitbook/assets/3 (71).png>)

* Date\
  \- Date형식으로 표시할 셀 클릭\
  \- displaytype : date
* Married\
  \- text > Set Expression - MARRIED==1?"기혼":"미혼"
* Salary\
  \- displaytype : number
* Gender\
  \- text > Set Expression - GENDER=="M"?"남자":"여자"
* Dept_id\
  \- displaytype : combotext\
  \- CellCombo\
    combodataset : ds_dept

### transaction

* strURL \
  \- Spring의 @RequestMapping value값
* strinDatasets\
  \- Dataset 여러개를 보낼때 공백으로 구분하기 때문에 \
    "input1=DataSet00 input2=DataSet01:\
    이처럼 작성해야지 =사이에 공백이 들어가면 안되니 주의\

* strArgument\
  \- key-value형식
* strCallbackFunc\
  \- 트랜잭션 결과를 받을 떄 호출될 콜백함수명
* bAsync\
  \- 트랜잭션을 비동기 처리할지 여부\
  \- true(default) : 비동기, 동시처리\
    false : 동기, 동시처리 불가능\


### transaction : 서버의 data가져오기

```javascript
this.Button00_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	
	var strSvcID = "select";
	var strURL = "http://demo.nexacro.com/edu/nexacro/employees_select.jsp";
	var strInDatasets = "";//조회 이므로 필요없음
	var strOutDatasets = "ds_employee=ds_employees";
	var strArgument = "";
	var strCallbackFunc = "tr_c";

	this.transaction(
		strSvcID
		,strURL
		,strInDatasets
		,strOutDatasets
		,strArgument
		,strCallbackFunc)
};

this.tr_c = function (strSvcID, nErrorCode, strErrorMag){
	
	if(nErrorCode < 0) {
		this.alert(strErrormag)
	}else{
		this.alert("조회성공")
	}
};
```

### data CRUD 전송하기

```javascript
this.tr_c = function (strSvcID, nErrorCode, strErrorMag){
	
	if(nErrorCode < 0) {
		this.alert(strErrormag)
	}else{
		if(strSvcID == "select"){
			this.alert("조회성공")
		}else {
			this.alert("저장성공")
		}
	}
};


this.Button01_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	var strSvcID = "save";//insert만 말하는 것이 아니라 변경사항 모두를 저장하는 것
	var strURL = "http://demo.nexacro.com/edu/nexacro/employees_save.jsp";
	var strInDatasets = "in_ds=ds_employee:A";
	var strOutDatasets = "";//저장 이므로 필요없음
	var strArgument = "";
	var strCallbackFunc = "tr_c";

	this.transaction(
		strSvcID
		,strURL
		,strInDatasets
		,strOutDatasets
		,strArgument
		,strCallbackFunc)
};
```

* strInDatasets에 서버에 보낼 Id뒤에 :A를 작성하면\
  모든 data를 전송한다.
* 위처럼 코드를 작성하면 data변경 후 > 저장 > 조회 시 변경된 데이터가 들어있음을 볼 수 있다.

### data 삭제

```javascript
this.Button02_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	var nRow = this.ds_employee.rowposition
	
	this.ds_employee.deleteRow(nRow)
};//화면에서만 삭제된 것이지 트랜잭션되어 서버에 반영된 것은 아니다.
  //바로 처리시 조회하면 안보이고 저장하고 처리하면 복구가 가능하다. 기능의 차이
```

* 삭제 후 저장버튼으로 data를 서버에 전송해야 서버에 반영된다.

### data 추가

```javascript
this.Button03_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	var nRow = this.ds_employee.addRow()
};
```

* 추가 후 저장버튼으로 data를 서버에 전송해야 서버에 반영된다.

### 추가시 기본값 넣기

```javascript
this.Button03_onclick = function(obj:nexacro.Button,e:nexacro.ClickEventInfo)
{
	var nRow = this.ds_employee.addRow()
	this.ds_employee.setColumn(nRow, "GENDER","M")
	this.ds_employee.setColumn(nRow, "FULL_NAME","이름입력")
	this.ds_employee.setColumn(nRow, "DEPT_ID","01")
};
```

### Grid Content Editor 

* 컬럼로우(헤더)를 여러줄로 만들 수 있다.\
  \- 왼쪽 head의 row0 우클릭 : add, insert
* 셀 병합\
  \- shift + 셀 선택 : merge
* 동적 row 적용\
  \- expr = expression\
  \- 순번 컬럼의 text에 currow+1하면 자동으로 1부터 작성된다.\
  \- expr에서는 this가 Form이 아닌 선택된 셀을 가리킨다.   \
    dataset.getSum("SALARY")
* 해당 컬럼 위치 고정\
  \- 컬럼 우클릭 > add column > left\
  \- 화면의 왼쪽에 고정된다.
* editType : 입력방식

### row에 삼항연산자가 아닌 함수 지정하기

```javascript
this.gender = function(obj) {//obj="M"or"W"
	if(obj == "M"){
		return "남자"
	}else{
		return "여자"
	}
};
```

* 함수를 만들고 Grid Content Editor에서 

![](<../../.gitbook/assets/1 (119).png>)

* text > expr에서 함수를 지정할 수 있다.
* comp : 지금 바라보고 있는 Grid\
  parent : Grid의 부모 = Form(this를 사용할 수 없으므로)\
  gender : 함수이름\
  (GENDER) : 적용할 컬럼명

### Nexacro : Base

* spring에서의 패키지와 같은 역할로 보통 form을 업무에 따라 분리한다.
* 패키지 추가하기\
  \- TypeDefinition > Services 더블클릭

