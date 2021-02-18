# 교육 - Nexacro2

### Grid

* dataset을 드래그해 보여줄 수 있다.
* 더블 클릭시 Gird Content Editor

### component와 dataset연동

![1](../../.gitbook/assets/1%20%28119%29.png)

* component 의 property의 Binable의 Value속성에서 dataset의 컬럼과 연동 할 수 있다.
* 이렇게 지정한 값이 compnent에 보이게 되고 component에서 수정한 값은 바로 Gird에서 바뀌는 것을 확인할 수 있다.
* 하지만 Edit Component와 다르게 MaskEdit나 다른 컴포넌트들은 기본값이 보이지 않는다.

### Component - type, format 변환 : Properties

![2](../../.gitbook/assets/2%20%2894%29.png)

* 컴포넌트의 Properties에서 format과 type을 확인한다.
* empl\_id - format : a\#\#\# - type : stirng
* married - falsevalue : 0 - truevalue : 1
* salary - format : 9,999
* gender - Binding   innerdataset : innerdataset   code 컬럼 : W, M   data 컬럼 : 남자, 여자 - direction : vertical - shift를 누르면 간격 조절이 가능하다.
* dept_id - Binding_   innerdata : ds\_dept

### Grid Content Editor : Component와 맞추기

![3](../../.gitbook/assets/3%20%2871%29.png)

* Date - Date형식으로 표시할 셀 클릭 - displaytype : date
* Married - text &gt; Set Expression - MARRIED==1?"기혼":"미혼"
* Salary - displaytype : number
* Gender - text &gt; Set Expression - GENDER=="M"?"남자":"여자"
* Dept\_id - displaytype : combotext - CellCombo   combodataset : ds\_dept

### transaction

* strURL  - Spring의 @RequestMapping value값
* strinDatasets - Dataset 여러개를 보낼때 공백으로 구분하기 때문에    "input1=DataSet00 input2=DataSet01:   이처럼 작성해야지 =사이에 공백이 들어가면 안되니 주의 
* strArgument - key-value형식
* strCallbackFunc - 트랜잭션 결과를 받을 떄 호출될 콜백함수명
* bAsync - 트랜잭션을 비동기 처리할지 여부 - true\(default\) : 비동기, 동시처리   false : 동기, 동시처리 불가능 

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

* strInDatasets에 서버에 보낼 Id뒤에 :A를 작성하면 모든 data를 전송한다.
* 위처럼 코드를 작성하면 data변경 후 &gt; 저장 &gt; 조회 시 변경된 데이터가 들어있음을 볼 수 있다.

