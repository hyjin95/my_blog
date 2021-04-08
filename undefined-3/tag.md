# Tag 태그

## &lt;head&gt;

### &lt;Meta&gt;

#### 설명

* meta태그는 해당 문서에 대한 정보인 메타데이터\(metadata\)를 정의할 때 사용한다.
* &lt;head&gt;태그 안에 작성된다. 화면에 보이지는 않지만 브라우저, 검색엔진, 다른 웹 서비스에서 읽어 사용하게된다.
* HTML5에서는 meta태그의 scheme속성을 지원하지 않는다. 문자속성을 정의하기 쉽게 charset속성이 추가되었다.

#### 속성

* name 또는 http-equiv속성이 명시되었다면 content속성이 반드시 작성되어야 한다. 반대로 content속성은 name이나 http-equiv속성이 없다면 작성될 수 없다.

```markup
<meta name="keyword" content="HTML, meta, tag, element, reference">
```

* 검색엔진을 위한 키워드\(keyword\)를 정의하는 메타데이터 검색엔진에게 해당 문서의 중요 태그, 키워드 정보를 제공한다.

```markup
<meta name="description" content="HTML meta tag page">
```

* 웹 페이지에 대한 설명\(description\)을 정의하는 메타데이터

```markup
<meta name="author" content="TCPSchool">
```

* 문서의 저자\(author\)를 정의하는 메타데이터

```markup
<meta http-equiv="refresh" content="5;url=http://www.tcpschool.com">
```

* 5초뒤에 다른 페이지로 리다이렉트\(redirect\)시키는 메타데이터

```markup
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

* 모든 장치\(디바이스\)에서 웹 사이트가 잘 보이도록 뷰포트\(viewport\)를 설정하는 메타데이터

## &lt;body&gt;

