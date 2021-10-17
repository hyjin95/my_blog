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
			
			String headerType = myCon.getContentType();//마임타입
			InputStream is = myCon.getInputStream();

			BufferedReader br = new BufferedReader(new InputStreamReader(is));
			//is.read();
			String data = null;
			while((data=br.readLine())!=null) {
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
* 7번 : myCon에서 마임타입을 가져온다.
* 8번 : is를 연결통로로 생성한다.
* 10번 : BufferedReader클래스의 InputStreamReader메서드를 생성해 받아온 정보를 읽는다.\
  \- BufferedReader클래스를 사용하는 이유는 **소스의 모든 라인**을 읽어오기 위함이다.\
  \- new를 통해 객체를 생성하는 경우는 단독으로 사용할때에만 사용가능하다.\
    A a = new A( );\
    A a = new B( );\
  \- 다른 디바이스, 시스템(하드웨어)과 연계하여 요청, 응답을 처리할때에는 주소번지.메서드( )형태로 객체주입을 받는다.\
    A a = XXX.methodA( );
* 13,14번 : 가져온 정보가 있다면 출력해 확인해보자
* 17,18번 : io패키지에서 **사용한 io는 반드시 닫아준다**.

### main 메서드

```java
	public static void main(String[] args) {
		//https:192.168.0.187:9000/dev_web/index.jsp?12365
		ViewURL vu = new ViewURL("http://192.168.0.187localhost:9000/dev_web/index.jsp");
	}
}
```

* 8번 : 쿼리스트링을 이용해보자\
  \- http : http프로토콜을 사용한다.\
  \- 도메인(ip + port번호)\
  \- 경로\
  \- 접근할 주소
