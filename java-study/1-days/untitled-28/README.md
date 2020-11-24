---
description: 2020.11.23 - 69일차
---

# 69 Days - 로그인:설계,프로시저, 비동기통신, MyBatis, cookie

### 사용 프로그램

* 사용언어 : JAVA\(JDK\)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org - Toad DBA Suite for Oracle 11.5
* 사용 서버 - WAS : Tomcat

## 필기

### GitHub검색

* 구글 : awesome + 플랫폼이름, 

### 도스 : sqlplus

* sqlplus scott/tiger/@ip주소:1521/orcl11

### JQuery를 사용하는 

* 크로스 브라우징 서비스 - 개발자가 브라우저별로 테스트를 별도로 진행하지 않아도된다. - 시간절약
* 대체로 표준을 만족한다.

## 로그인 프로시저 구현 : 비동기통신

### 설계

* 사용자의 입장 : 화면이 필요하다.
* 개발자 : 화면으로부터 입력된 id, pw를 받아온다.
* 서버 : ajax의 반환값인 name이 필요하다. - 로그인 성공시 회원마다 다른 화면을 제공하기 위해 변수 name을 받아온다. - name '님 환영합니다.'
* ajax : 결과값이 표시될 태그의 id가 필요하다.

### 흐름도 : 페이지이동, class관계

![](../../../.gitbook/assets/2%20%2858%29.png)

* Controller에는 Logic이, Logic에는 Dao가 인스턴스화되어 있어야 한다. - 의존성주입\(DI\)

### DI

* Dependency Injection : 의존성 주입
* Java에서는 인스턴스화가 여기에 해당한다.
* 외부에서 주입받아 사용함으로서 의존성이 생기는 것을 말한다.

### ajax

```javascript
$.ajax({
			 url:''
			,type:'get'|'post'
			,dataTypw: 'html'|'json'|'xml'
			,success:function(data){
						var result = JSON.stringify(data);
						var array = JSON.parse(result);
						//append를 사용해 html태그가 포함된 코드를 작성한다.
				    $("#d_xxx").html(imsi);
			}
```

* 5번에서 ajax를 거쳐 나온 data는 Object이다.
* 6번에서 Object element를 String형태로 바꾼다.
* 7번에서 JSON형태의 객체 배열로 전환한다.
* 8번에서 이 배열을 담은 html코드를 append를 사용해 작성한다.
* 9번에서 원하는 div에 출력한다.

### 로그인 비동기통신 구현

![](../../../.gitbook/assets/1%20%2875%29.png)

![&#xC778;&#xC99D; &#xC131;&#xACF5;&#xC2DC;](../../../.gitbook/assets/2%20%2857%29.png)

* 두번째 이미지처럼 로그인 인증이 성공하면 회원의 이름을, 실패하면 실패내용을 로그인 영역에 보여준다.

{% page-ref page="undefined.md" %}

## 로그인 프로시저 생성

### Procedure 생성 준비

![](../../../.gitbook/assets/1%20%2876%29.png)

1. 프로시저 이름정하기 : proc\_ajaxLogin\( \)
2. 파라미터 정하기 : IN, OUT - IN : p\_id, p\_pw - String, varchar2 - OUT : 아이디가 존재하지 않습니다.             패스워드가 다릅니다.             변수이름 : msg, result,...
3. 로그인 상태값 : r\_status - 프로시저의 IS구문에서 변수를 선언해 안에서 처리할 수 있지만, 화면에서 name이외에 로그인이 성공했는지 구분해야하는 값이 필요한 경우, OUT을 두개로 해 넘긴다. 동시에 사용자에대한 다른 정보를 불러오거나 다른 화면을 표시해야할때 사용할 수 있지만,  이 경우에는 트랜잭션처리가 필수일 것이다.

### Procedure 생성

```sql
CREATE OR REPLACE PROCEDURE proc_ajaxLogin
(p_id IN varchar2,p_pw IN varchar2, msg OUT varchar2, r_status OUT varchar2)
IS 
BEGIN
    SELECT NVL((SELECT mem_id FROM member69
                 WHERE mem_id = p_id),'-1') INTO r_status
      FROM dual;      
    IF  r_status = p_id then
    SELECT NVL((SELECT mem_name FROM member69
                 WHERE mem_id = p_id
                   AND mem_pw = p_pw), '비밀번호가 틀립니다.') INTO msg
      FROM dual;
    ELSIF r_status = '-1' then
        msg:='아이디가 존재하지 않습니다.';
    END IF;
END;
```

* 1번 OR REPLACE : 프로시저 재생성 - 재컴파일하기, 프로시저도 parsing을 통해 유효성 체크를 한다.
* 5-6번 : id인증, id가 있으면 id를, 없으면 -1을 r\_status에 담는다.
* 8-11번 : r\_status가 id면,\(id인증이 성공\) 패스워드를 인증한다.                id가 같은 row의 pw가 파라미터로 받은 pw와 같으면 msg에 회원의 이름을,                 아니라면 안내글을 담는다.
* 13-14번 : r\_status가 -1이라면 msg에 안내글을 담는다.
* 13번에서 else if 에 e가 있으면 오류가 발생한다.

### Procedure 호출 : CMD

![pw&#xAC00; &#xB2E4;&#xB978; &#xACBD;&#xC6B0;](../../../.gitbook/assets/3%20%2845%29.png)

![&#xC778;&#xC99D;&#xC774; &#xC644;&#xB8CC;&#xB41C; &#xACBD;&#xC6B0;](../../../.gitbook/assets/4%20%2838%29.png)

* 위 구문으로 sqlplus에 접속해 확인할 수도, 토드에서 직접 확인할 수도 있다.

### Procedure의 결과 값

* 결과 값은 자바에서 처럼 리턴타입으로 나와서 받을 수 있는 것은 아니고, 파라미터로 받게된다.
* MyBatis Layer를 사용하면, Dao에서 Mybatis의 sql문을 호출할때 selectOne\("sql문 id", 넘길값\)을 사용하는데, 이때 넘어가는 넘길 값이 파라미터이자, 결과를 담는 객체가된다. - selectOne\("sql id", pmap\) - pmap.get\("key"\).toString\( \);
* 그래서 MyBatis의 resultType을 작성하지 않는다. 
* sql문이 select여야만 값을 받을 수 있다.
* 결과값이 없어 null이 나오면 화면에서 nullPointerException이 발생할 수 있으니 주의하자.

## MyBatis : 프로시저호출

### Mybaits : statementType

* MyBatis를 사용해 프로시저를 호출하는 경우에는 **statementType** 속성이 있어야 프로시저를 인식한다. statementTyped을 "CALLABLE"로 지정하면 프로시저에서 out으로 지정한 값들이 sql을 요청할때 사용한 파라미터에 담긴다.
* Dao에서 sql문을 요청할떄 사용하는 selectOne, selectList함수들의 파라미터 값에 결과값을 담는다. - selectOne\("sql id", pmap\) - pmap은 파라미터이자 리턴값이다.

### 코드 : member.xml

```markup
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="oracle.mybatis.MemberMapper">
	<select id="proc_ajaxLogin" parameterType="map" statementType="CALLABLE"><!-- 프로시저 호출이기때문에 리턴값이 없어 리턴타입이 필요없다 -->
		{call proc_ajaxLogin( #{mem_id, mode=IN, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{mem_pw, mode=IN, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{msg, mode=OUT, jdbcType=VARCHAR, javaType=java.lang.String}
							 ,#{status, mode=OUT, jdbcType=VARCHAR, javaType=java.lang.String}
		)}
	 </select>
</mapper>
```

## cookie

### cookie

* cookie로 생성한 값이 페이지를 이동해도, 새로고침해도 남아있는 것을 확인 할 수 있다.
* 여러 회사들이 세션아이디로 사용자를 구분한다.
* 세션아이디는 각 회사들이 부여하고, 이 값은 사용자의 local pc에 저장된다.
* 시간이 보장되면 무한히 유지될 수 있다.
* cookie는 text type이다.

### cookie API

```markup
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-cookie/1.4.1/jquery.cookie.min.js"></script>
```

* 똑같은 기능을 하는 오픈소스 중에서도 가능한 표준에 부합한,  표준 방식을 따르는 API를 선택해 사용한다.

### JS: cookie

```javascript
$.cookie('이름', 값)
```

* Map과 비슷한 형태
* session.setAttribute\("이름", 값\)과 비슷한 형태

### cookie 옵션

```javascript
$.cookie('c_id', 'test', {expires:1, path:' ', domain: ' ', secure: true|false})
```

* 'c\_id'라는 이름으로 test를 cookie에 저장한다.
* expires: 숫자 - 시간, 얼마동안 유지될 것인지
* path: ' ' - 유지 경로, 해당 경로 내에서는 cookie가 유지된다. - cookie삭제시에도 주의해야한다. 같은 루트의 cookie를 다 지울 수 도 있으므로
* domain: ' ' - 어디서 생성된 쿠키인지 알려주는 주소
* secure: true\|false - true라면 https에서만 사용가능한 쿠키 - 정부기관에 시고되어 정부가 인증서를 부여해준 자 만 이용 가능한 프로토콜

### cookie와 session

* cookie - local pc에 text로 저장된다. - 시간이 보장되면 그 시간동안에는 무한히 유지된다. - session에 비해서는 보안에 취약하다.
* session - 서버의 cash메모리에 저장된다. - 한번에 하나만 담을 수 있다.  - FIFO

### cookie Test

{% page-ref page="cookie-test.md" %}

후기 : 으 영하2도 넘모 추워...내일부터는 지하철이 10시 이후로는 감축 운행한다. 집에 일찍가야지...그러려면 수업시간에 더 집중해서 정리를 해야한다 화이팅!!!

