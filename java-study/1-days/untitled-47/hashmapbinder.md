# HashMapBinder

```java
package com.util;

import java.util.Enumeration;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import jdk.nashorn.internal.ir.RuntimeNode.Request;

//사용자로부터 입력받는 값을 효과적으로 처리해본다.

public class HashMapBinder {
	public HttpServletRequest request = null;
	
	public HashMapBinder(HttpServletRequest request) {
		this.request = request;//원본을 사용해야한다.	서블릿에서 인스턴스화	
	}
	
	public void bind(Map<String, Object> target) {
		target.clear();
		Enumeration<String> en = request.getParameterNames();//getParameter로 받아오는 값의 리턴타입은 String이므로 Enumeration의 타입도 String으로 한다.
		
		while(en.hasMoreElements()) {//hasMoreElement는 boolean타입 메서드
			String key = en.nextElement();
			target.put(key, HangulConversion.toUTF(request.getParameter(key)));//한글 인코딩, post방식은 따로 해줘야댐
		}
	}
```

* request객체를 사용해 사용자가 입력한 값을 가져와주는 클래스
* getParameterName의 return type은 String이다.
* 20번 : 다음 파라미터 이름이 존재한다면
* 22번 : String key 라는 변수에 이름을 담는다.
* 23번 : 파라미터로 받은 map에 key와 해당 key를 이름으로하는 파라미터 값을 넣어준다.
* 반복
* 한글 data인 경우, 한글깨짐을 방지하기위해 23번에 인코딩 클래스를 활용했다.

