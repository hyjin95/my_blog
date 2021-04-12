# cookie

### cookie

* 브라우저에 저장되는 작은 크기의 문자열로 Http프로토콜의 일부이다.
* 주로 웹 서버에 의해 만들어지고 서버가 http응답 헤더\(header\)의 set-cookie에 내용을 넣어 전달하면 브라우저가 이를 저장하게 된다.
* 브라우저는 사용자가 쿠키를 생성하도록 한 동일 서버 접속시 마다 쿠키의 내용을 cookie 요청 헤더에 넣어 함께 전달한다.
* 클라이언트 식별자로서 주로 사용한다. - 사용자가 로그인하면 서버는 cookie를 설정한다. - 사용자가 동일 도메인에 접속시 브라우저는 cookie정보를 같이 전송한다. - 서버는 브라우저가 보낸 요청 헤더를 읽어 사용자를 식별한다.
* JS의 document.cookie 프로퍼티를 이용하면 브라우저에서도 cookie에 접근할 수 있다.
* 브라우저에 따라 저장할 수 있는 쿠키의 갯수는 한정되어 있다.

### document.cookie : 쿠키 읽기

```javascript
//모든 쿠키 읽어오기
alert( document.cookie ); // cookie1=value1; cookie2=value2;...
```

* 현재 브라우저에 현재 페이지에 관련된 쿠키가 있는지 알아볼 수 있다.
* name = value 의 쌍으로 구성되어 있다.
* 각 쌍은 ; 세미 콜론으로 구분한다. 쌍 하나는 하나의 독립된 쿠키를 나타낸다.

### document.cookie : 쿠키 쓰기

```javascript
document.cookie = "user = smith";//user쿠키 값 갱신
```

* 브라우저가 저장한 cookie중 name이 user인 cookie 값만 smith로 갱신한다.

```javascript
// 특수 값(공백)은 인코딩 처리해 줘야 합니다.
let name = "my name";
let value = "John Smith"

// 인코딩 처리를 해, 쿠키를 my%20name=John%20Smith 로 변경하였습니다.
document.cookie = encodeURIComponent(name) + '=' + encodeURIComponent(value);

alert(document.cookie); // ...; my%20name=John%20Smith
```

* 쿠키의 이름과 값에는 특별한 제약이 존재하지 않는다. 모든 글자가 허용되지만 형식의 유효성의 일관성을 유지하기 위해 내장함수 encodeURIComponent를 사용해 이름과 값을 escape처리해야 한다.
* encodeURIComponent로 인코딩한 후 name=value쌍은 4kb를 넘을 수 없다. 넘으면 cookie에 저장될 수 없다.

### cookie 옵션

```javascript
document.cookie = "user=John; path=/; expires=Tue, 19 Jan 2038 03:14:07 GMT"
```

* 쿠키는 여러 옵션을 갖고있는데 몇가지 옵션은 반드시 명시해야 한다.
* 옵션은 key=value의 뒤에 나열하고 ; 세미콜론으로 구분한다.
* **path** path=/mypath, path=/admin, path=/ URL path의 접두사로 해당 경로나 경로의 하위에 있는 페이지에서만 cookie에 접근할 수 있도록 한다. 절대경로를 사용하고 기본값은 현재경로
* **domain** domain=site.com cookie에 접근가능한 도메인을 지정한다. 제약이 존재한다. 옵션에 값이 없으면 쿠키를 설정한 도메인으로 지정된다. 서브도메인에서는 쿠키 정보를 얻을 수 없다. 서브도메인에서 접근하려면 쿠키 설정시 domain옵션에 설정해주어야 한다.
* **exprise, max-age** 유효일자와 만료기간 옵션이 지정되지 않으면 쿠키는 브라우저가 닫힐때 삭제된다. = session cookie exprise나 max-age를 설정하면 브라우저를 닫아도 cookie는 삭제되지않는다.
* **exprise\(유효일자\)** 유효일자는 GMT 포맷으로 설정해야한다. date.toUTCString으로 포맷을 GMT로 변경할 수 있다. 옵션값을 과거로 지정하면 쿠키는 삭제된다.
* **max-age\(만료기간\)** 현재부터 설정하고자 하는 만료일시 까지의 시간을 초로 환산한 값을 갖는다. 0이나 음수값을 설정하면 쿠키는 삭제된다.
* **secure** 이 옵션을 설정하면 Https로 통신하는 경우에만 cookie가 전송된다. 설정하지 않으면 기본값으로 http, https모두에서 쿠키에 접근할 수 있다. 쿠키에 보안을 지켜야해 암호화되지않는 http연결을 막을때 사용하는 옵션
* **semesite** xx

