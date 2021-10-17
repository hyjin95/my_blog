# ApiExamCaptchaNkey

### 선언부

```java
package naver.net;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;

// 네이버 캡차 API 예제 - 키발급
public class ApiExamCaptchaNkey {
	//생성자활용연습, 클래스쪼개기, 인터페이스와 추상클래스 중심의 코딩
  //Framework:Spring, Deven, AnyFrame, Struts
```

* 클라이언트 id, secret정보로 key값을 발급받아주는 클래스
* java.io 패키지 : 읽기, 쓰기 기능을 지원하는 패키지
* java.net 패키지 : 외부(네이버, 카카오, 아마존, 도커,..)와의 통신을 가능하게 해주는 패키지\
  \- java.net.URL : http프로토콜을 사용해 서비스를 제공하는 시스템에 접속하려면 필요한 패키지
* 위 두 패키지는 예외처리가 필수이다.

### main 메서드 

```java
	//메인 메서드(thread) - 시작점
    public static void main(String[] args) {
        String clientId = "JVMdTwabK3fh1C8qVdDk"; 
        String clientSecret = "diq1lvMWsz"; 

        String code = "0"; // 키 발급시 0,  캡차 이미지 비교시 1로 세팅
    
        String apiURL = "https://openapi.naver.com/v1/captcha/nkey?code=" + code;

        Map<String, String> requestHeaders = new HashMap<>();
        requestHeaders.put("X-Naver-Client-Id", clientId);
        requestHeaders.put("X-Naver-Client-Secret", clientSecret);
        //네이버 서버에서 전송된 키 값이 담기는 곳, get파라미터의 리턴타입은 String
        String responseBody = get(apiURL, requestHeaders);

        System.out.println(responseBody);//키 출력
        
        //추가된 부분-생성자를 통해 출력된 키를 넘긴다.
        ApiExamCaptchaImage aeci = new ApiExamCaptchaImage(responseBody);
    }
```

* 3번 : 애플리케이션 클라이언트 아이디, 사용자가 입력해야하는 파라미터 값
* 4번 : 애플리케이션 클라이언트 시크릿, 사용자가 입력해야하는 파라미터 값
* 6번 : 접속 용도가 키 발급인지, 이미지 비교인지 알려주는 code
* 9번 : 쿼리스트링을 이용한 캡차 네이버 접속 URL
* 12번 : key발급 메서드를 사용하기위해 id와 secret을 HashMap에 담는다.
* 14번 : 네이버 서버에서 전송된 키 값이 담기는 변수를 선언하여 get메서드를 호출한다.\
  \- id와 secret을 파라미터로 받아서 네이버가 인증과정을 거치게 하는 메서드, 반환값은 String타입
* 16번 : key값을 제대로 받아왔는지 단위테스트
* 19번 : key값으로 이미지파일을 발급받는 클래스의 생성자에 key값을 파라미터로 넘긴다.

### get 메서드

```java
    private static String get(String apiUrl, Map<String, String> requestHeaders){
    	//네이버 서버와의 연결통로 확보 
        HttpURLConnection con = connect(apiUrl);
        try {
            con.setRequestMethod("GET");
            for(Map.Entry<String, String> header :requestHeaders.entrySet()) {
                con.setRequestProperty(header.getKey(), header.getValue());
            }
            int responseCode = con.getResponseCode();
            if (responseCode == HttpURLConnection.HTTP_OK) { // 정상 호출
                   return readBody(con.getInputStream());
            } else { // 에러 발생
                return readBody(con.getErrorStream());
            }
        } catch (IOException e) {
            throw new RuntimeException("API 요청과 응답 실패", e);
        } finally {
            con.disconnect();
        }
    }
```

* 소켓통신대신 http프로토콜을 이용한 웹통신을 처리한다.\
  \- 소켓을 직접 생성, 관리하지 않는다.
* 3번 : 네이버 서버와의 연결통로 확보 - connect메서드에 URL을 파라미터로 넘겨 호출한다.
* 10번 : 네이버에서 보내주는 응답코드를 받는다.\
  \- 채팅서버에서 우리가 구현했던 Protocol과 같은 원리\
  \- 100, 200, 300, ... int 타입의 코드
* 11번 : 정상적으로 응답이 도착했다면, \
  \- HTTP_OK = 200번\
  \- 받아온 code가 정상 호출 응답 코드 라면,
* 12번 : 반환값으로 readBody메서드를 connect메서드에서 getInputStream하는 파라미터로 호출한다.

### connect 메서드

```java
    //네이버 서버에 택하는 메서드
    private static HttpURLConnection connect(String apiUrl){
        try {
            URL url = new URL(apiUrl);
            return (HttpURLConnection)url.openConnection();
        } catch (MalformedURLException e) {
            throw new RuntimeException("API URL이 잘못되었습니다. : " + apiUrl, e);
        } catch (IOException e) {
            throw new RuntimeException("연결이 실패했습니다. : " + apiUrl, e);
        }
    }
```

* get메서드에서 호출되는 네이버 서버에 컨택하는 메서드
* 파라미터로 받아온 apiURL은 접근할 네이버 서버주소이다.
* 4번 : String으로 받아온 URL주소로 네이버 서버에 컨택한다.
* 5번 : 반환값으로 연결을 제공한다.

### readBody 메서드

```java
    /*
     * @return 으로 받는것은 키 값이다.
     * @param 으로 받는 것은 서버에서 전송된 정보이다.
     */
    private static String readBody(InputStream body){
        InputStreamReader streamReader = new InputStreamReader(body);

        try (BufferedReader lineReader = new BufferedReader(streamReader)) {
            StringBuilder responseBody = new StringBuilder();

            String line;
            while ((line = lineReader.readLine()) != null) {
                responseBody.append(line);
                //responseBody.append("{key:키값}");
            }

            return responseBody.toString();
        } catch (IOException e) {
            throw new RuntimeException("API 응답을 읽는데 실패했습니다.", e);
        }
    }
}
```

* get메서드에서 반환값으로 호출되는 메서드
* 파라미터로 is타입변수에 서버에서 전송된 정보를 받는다.
* 6번 : InputStreamReader클래스를 사용해 파라미터로 받아온 정보를 읽는다.
* 8번 : 문자가 아닌 문자열을 읽어오기 위해 BufferedReader를 사용한다.
* 9번 : StringBuilder클래스를 이용해 결과를 담을 변수를 생성한다.\
  \- StringBuilder타입으로 하는 이유는 append함수를 사용하기 위함이다.\
  \- 새로운 객체를 생성하는 대신 기존 데이터에 더하기 위해
* 11,12번 : String변수를 하나 생성해 8번에서 읽어몬 문자열이 빈 값이 아닌경우에 만들어둔 StringBuilder타입 변수에 받아온 문자열을 append한다.
* 13번 : line = "{key : 키 값}"
