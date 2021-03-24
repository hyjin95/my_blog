# JSON과 JavaScript Object 차이

### 구조

```java
{
    "name" : "smith",
    "age" : 10
} 
```

* JSON
* 파일 형식 중 하나
* 함수를 값으로 할당할 수 없다. 프로퍼티는 " " 로 표시해야한다.

```javascript
let obj = {
    name : "smith",
    age : function(){
        return 10;
    }
}
```

* JS Object
* JS Engine 메모리 안에 있는 데이터 구조

### JS의 JSON.parse\( \) 

```javascript
'{ "name":"smith", "age":10}'

var obj = JSON.parse('{ "name":"smith", "age":10}');
```

* JS는 JSON을 Object로 파싱할 수 있도록 해주는 JSON.parse\( \) 메서드를 제공한다.
* http 요청은 문자열로 전송되므로 JSON을 문자열로 만들어 보내고 받은 JS에서는 다시 JS Object로 변환한다. JSON.stringify\( \) &gt; JSON.parse\( \)
* JSON으로 서버와 클라이언트가 데이터를 주고받는다.

