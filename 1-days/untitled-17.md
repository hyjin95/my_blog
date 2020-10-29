---
description: 2020.10.29 - 52일차
---

# 52 Days - combobox, table-style

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5 - Visual Studio Code
* 사용 서버 - WAS : Tomcat

## 복습

### 화면

* HTML -&gt; e\(브라우저\)가 html해석 -&gt; dom 트리 구조 완료
* HTML - 처리주체 : client\(local\) - 정적 페이지 \(결정 되어있다\)
* JS -  html에게 동적 페이지 지원, 변수 지원, 함수 지원 - JS로는 화면을 그리지 않는다. 코드가 길어지므로 - JQuery를 사용해 코드를 더 줄 일 수 있다.
* CSS - html에 style지원

### 서버

* JSP, Servlet을 이용해 html과 java가 연결된다. - 처리주체 : 서버, 동적이다.\(미정\) - JSP : Servlet을 이용해 html에 자바코드를 작성 - Servlet : 자바에 html코드를 작성
* Servlet이 제공해주는 request, response객체 - 화면에 출력할 수 있는, 브라우저가 읽을 수 있게 하는 out내장객체를 지원한다.
* dataSet은 json형식을 사용

### 위치

* HTML : &lt;head&gt;, &lt;body&gt;의 차이점 - **재사용성 : &lt;head&gt;안에서 함수를 선언, &lt;body&gt;는 함수를 호출**한다. - &lt;head&gt;안에서 전역변수를 선언할 수 있다.   전역변수를 식별자\(pk\)로서 where절에 조건으로 사용할 수 있다.   global variable = member variable

## API활용

### API 사용

* API를 사용하려면 해당 API의 jar파일을 부여해주어야 한다. - WEB-INF의 lib폴더 안에 배치, 사용시 import, link - myBatis.jar, ojdbcd.jar, log4j.jar, ......
* API에서 제공하는 class들은 변수, 메서드를 갖는다. - 비슷한 계열의 class들은 같이 살펴본다 합쳐서 응용 활용할 수 있다.
* API에서 제공하는 함수를 사용할때에는 파라미터를 확인한다.

### UI/UX솔루션

* 스크립트 기반 UI : 넥사크로, easyui, 시맨틱UI, 부트스트랩 등 - XML기반 : 넥사크로, 안드로이드 등 - JS기반 : easyui, 부트스트랩, 시맨틱UI 등
* 솔루션을 잘 사용하기위해서 API를 보고 활용해야한다.

### 스크립트 API활용

* $\("\#아이디"\).객체이름\( \); - ID의 객체를 구체화시켜준다. - &lt;body&gt;의 해당 id 태그의 객체를 JS가 구체화 시킨다.
* $\("\#아이디"\).객체이름\( { } \); - 구현문 
* $\("\#아이디"\).객체이름\( 메서드이름, 값 \); - 열거형
* $\("\#아이디"\).객체이름\( { 속성명:값, 속성명: } \);
* $\("\#아이디"\).객체이름\( { 속성명:값, 속성명:값, 이벤트명:function\( \){ } } \);  - 이벤트까지 장착 가능
* $ = JQuery제공, **API를 부여하기위해 ID에 접근하려면 JS\(JQuery\)가 필요**하다. - id : html문서 내에 유일한 값으로 식별자로서 사용됨 - name : html문서 내에 중복 부여 가능한 값으로 배열로 사용할떄 사용됨

### 태그에 UI/UX솔루션 class 부여

* &lt;태그 class="접두어\(제공자\)-제공 클래스이름&gt; - &lt;input class="easyiui-dategrid&gt;
* JS에서 해당 태그에 접근할때 객체를 붙여 구체화 시켜준다.

### 태그 속성 우선

* 더 가까운, 더 나중에 부여된 속성이 적용된다. - &lt;tr width="300px"&gt; &lt;td width="200px"&gt;&lt;/td&gt;&lt;/tr&gt;   td의 width속성이 부여된다.

## empManager - table에 style

### 기존 화면

![&#xAE30;&#xC874;](../.gitbook/assets/2%20%2840%29.png)

```markup
<table border="0" width="100%">
<!-- 조건 검색 화면 시작 -->
	<tr>
		<td>
			<table>
				<tr>
					<td>
						<input id="nb_empno" class="easyui-numberbox" value="0">
						<input id="nb_sal" 	class="easyui-numberbox" value="0"	 data-options="groupSeparator:','"></td>
						<input id="dd_hiredate" label="입사일자 : " class="easyui-datebox"   value="today" required="required" style="width:200px">
					</td>
				</tr>
			</table>			
		</td>		
	</tr>
<!-- 조건 검색 화면 끝 -->
</table>
```

* 기존 화면

### 스타일 적용화면

![&#xC2A4;&#xD0C0;&#xC77C; &#xC801;&#xC6A9;](../.gitbook/assets/3%20%2833%29.png)

```markup
<table border="0" width="100%">
	<tr>
		<td>
			<table>
				<tr>
					<td width="300px">
						<label width="100px">사원번호 : </label>
						<input id="nb_empno" class="easyui-numberbox" value="0">
					</td>
					<td width="300px">
						<label width="100px">연봉 : </label>
						<input id="nb_sal" 	class="easyui-numberbox" value="0"	 data-options="groupSeparator:','">
					</td>
					<td width="300px">					
						<input id="dd_hiredate" label="입사일자 : " class="easyui-datebox"   value="today" required="required" style="width:200px">
					</td>
				</tr>
			</table>			
		</td>		
	</tr>
</table>
```

* 스타일과 여백을 주기위해 각 input태그를 &lt;td&gt;태그로 묶었다.

## empManager - combobox

### combobox

* easyui제공 : [http://jeasyui.com/documentation/index.php\#](http://jeasyui.com/documentation/index.php#)

