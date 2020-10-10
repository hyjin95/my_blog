# ViewURL - 웹 연결

### 선언부

```java
package naver.net;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URL;
import java.net.URLConnection;

public class ViewURL {
```

### 생성자

```java
	public ViewURL(String strURL) {
		try {
			URL myURL = new URL(strURL);
			URLConnection myCon = myURL.openConnection();
			myCon.connect();
			
			String headerType = myCon.getContentType();//마임타
			InputStream is = myCon.getInputStream();
			//new를 통해 객체를 생성하는 경우는 단독으로 사용할때에만 사용가능하고,
			//다른 디바이스나 다른 시스템(하드웨어 h/w)과 연계하여 요청과 응답을 처리할 때 필요하다.
			///new를 통해서가 아닌 주소번지.메섣()와 같은 형태로 객체주입을 받는다.
			/*
			 * A a = new A();
			 * A a = new B();
			 * A a = XXX.methodA();
			 */
			BufferedReader br = new BufferedReader(new InputStreamReader(is));
			//is.read();
			String data = null;
			while((data=br.readLine())!=null) {//소스의 모든 라인을 읽어오기위해 BufferedReader클래스를 사용한다.
				System.out.println(data);
			}
			//사용한 io는 반드시 닫아준다.
			br.close();
			is.close();
		} catch (Exception e) {
			// TODO: handle exception
		}
	}
```

* 3,5번 : URL클래스에 접근할 URL을 파라미터로 넣어 서버와의 연결통로를 open한다.

### main 메서드

```java
	//이렇게되면 해당 페이지의 정보가 외부에 노출될 수 있으므로 중요한 정보는 절대로 쿼리스트링으로 내보내지않는다.
	//또한 스트립트 코드들도 노출이 되지 않도록 외부에 별도로 작성하고 import하여 사용한다.
	//XXX.js, XXX.css로 따로 저장해둔다.
	//경로의 경우에도 절대경로가 노출당하면 서버에 대한 정보가 외부에 노출될 수도 있으므로 반드시 상대경로를 사용한다.
	//상대경로를 사용하면 전체 경로를 알 수 없다.
	public static void main(String[] args) {
		//https:192.168.0.187:9000/dev_web/index.jsp?12365
		ViewURL vu = new ViewURL("http://192.168.0.187localhost:9000/dev_web/index.jsp");

	}

}
```

