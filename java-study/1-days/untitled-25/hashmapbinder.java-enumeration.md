# HashMapBinder.java : Enumeration

## HashMapBinder

### HashMapBinder.java

```java
package com.util;

import java.util.Enumeration;
import java.util.Map;

import javax.servlet.http.HttpServletRequest;

import jdk.nashorn.internal.ir.RuntimeNode.Request;

/*
 * 공통코드 체험하기
 * 사용자로부터 입력받는 값을 효과적으로 처리해본다.
 * API보는 방법과 활용 능력을 키워본다.
 */

public class HashMapBinder {
	public HttpServletRequest request = null;
	
	public HashMapBinder() {//디폴트생성자
		
	}
	
	public HashMapBinder(HttpServletRequest request) {
		this.request = request;//원본을 사용해야한다.	서블릿에서 인스턴스화	
	}
	
	public void bind(Map<String, Object> target) {
		target.clear();
		Enumeration<String> en = request.getParameterNames();//getParameter로 받아오는 값의 리턴타입은 String이므로 Enumeration의 타입도 String으로 한다.
		while(en.hasMoreElements()) {//hasMoreElement는 boolean타입 메서드
			//<input name="meme_id"
			String key = en.nextElement();//mem_id, mem_pw, mem_addr,.....
			target.put(key, HangulConversion.toUTF(request.getParameter(key)));//한글 인코딩, post방식은 따로 해줘야댐
		}
	}
```

```java
	public Map<String, Object> bind2 (Map<String, Object> target) {
		target.clear();
		Enumeration<String> en = request.getParameterNames();//getParameter로 받아오는 값의 리턴타입은 String이므로 Enumeration의 타입도 String으로 한다.
		while(en.hasMoreElements()) {//hasMoreElement는 boolean타입 메서드
			//<input name="meme_id"
			String key = en.nextElement();//mem_id, mem_pw, mem_addr,.....
			String[] values = request.getParameterValues(key);
			for(int i=0;i<values.length;i++) {
				target.put(key, request.getParameterValues(key));
			}
		}
		return target;
	}

}
```

