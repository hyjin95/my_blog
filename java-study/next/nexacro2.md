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



