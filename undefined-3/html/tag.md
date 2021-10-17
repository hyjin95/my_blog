# Tag 태그

## \<head>

### \<Meta>

#### 설명

* meta태그는 해당 문서에 대한 정보인 메타데이터(metadata)를 정의할 때 사용한다.
* \<head>태그 안에 작성된다.\
  화면에 보이지는 않지만 브라우저, 검색엔진, 다른 웹 서비스에서 읽어 사용하게된다.
* HTML5에서는 meta태그의 scheme속성을 지원하지 않는다.\
  문자속성을 정의하기 쉽게 charset속성이 추가되었다.

#### 속성

```markup
<meta name="keyword" content="HTML, meta, tag, element, reference">
<meta name="description" content="HTML meta tag page">
<meta name="author" content="TCPSchool">
<meta http-equiv="refresh" content="5;url=http://www.tcpschool.com">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

* **keyword**\
  검색엔진을 위한 키워드(keyword)를 정의하는 메타데이터\
  검색엔진에게 해당 문서의 중요 태그, 키워드 정보를 제공한다.\
  오늘날의 주요 검색엔진에서는 사용되지 않는다.
* **description**\
  웹 페이지에 대한 설명(description)을 정의하는 메타데이터\
  검색엔진은 검색결과와 함께 이 설명을 가져간다.\
  설명이 없거나 부정확한 경우 검색엔진은 페이지의 다른 정보로 대체해 버린다.
* **author**\
  문서의 저자(author)를 정의하는 메타데이터
* **refresh**\
  ****content='초; url='\
  작성된 초(시간)뒤에 다른 페이지로 리다이렉트(redirect)시키는 메타데이터\
  url 생략시 현재페이지를 다시 읽는다.\
  오늘날에는 서버단에서 처리하므로 잘 사용하지 않는다.
* **viewport**\
  모든 장치(디바이스)에서 웹 사이트가 잘 보이도록 뷰포트(viewport)를 설정하는 메타데이터
* **robots**\
  content="noindex, nofollow"\
  위 처럼 content를 작성하면 구글에서 자신의 사이트가 색인화 되거나 링크연결이 되지 않도록 설정할 수 있다. 사이트 개발중이거나 일부 페이지가 노출되지 않길 원할때 사용하는 속성이다.\
  content="index, follow"\
  페이지 색인화, 링크 연결을 다시 활성화 한다.
* **generator**\
  해당문서를 생성하기 위해 사용된 소프트웨어 메타데이터
* **content**\
  name이나 http-equiv속성에 대한 메타 정보를 명시한다.\
  name 또는 http-equiv속성이 명시되었다면 content속성이 반드시 작성되어야 한다.\
  반대로 content속성은 name이나 http-equiv속성이 없다면 작성될 수 없다
* **http-equiv**\
  \- content-type : 문서의 문자 타입 지정, html5부터는 meta태그에 charset속성을 추가했다.\
  \- refresh : 새로고침, 페이지 이동을 위한 문서의 간격 및 url을 정의한다.\
  \- default-style : 선호 스타일 시트\
  HTTP header대신 사용되기도 한다. http-equiv속성은 content에 명시된 값에 대한 헤더를 제공한다.\
  더보기 : HTTP header(헤더)
* **charset**\
  html5부터 지원하는 속성\
  기존의 http-equiv="content-type" content="html/text"방식보다 간결하다.
* **scheme**\
  html5부터 지원하지 않는 속성\
  scheme="format 또는 URI"\
  해당 문서의 내부 값을를 브라우저가 이해하기 위한 format이나 정보의 URI를 명시한다. \


## \<body>

## 더보기

### HTTP header(헤더)

* 클라이언트와 서버가 요청, 응답으로 부가적인 정보를 전송할 수 있다.\
  요청일때에는 요청에 대한 정보를, 응답일 때에는 응답에 대한 정보를 담아 서버, 클라이언트에 보낸다.
* 공통헤더\
  요청, 응답 모두에서 사용되는 헤더\
  Date, Connectionm, Content-Length, Cache-Control, Content-Type, Content-Language
* 요청 헤더\
  Host, User-Agent, Accept, Cookie, Origin, If-Modified-Since, Aurhorization
* 응답 헤더\
  Server, Access-Control-Allow-Origin, Allow, Content-Disposition, Location, Content-Secutiry-Policy\
