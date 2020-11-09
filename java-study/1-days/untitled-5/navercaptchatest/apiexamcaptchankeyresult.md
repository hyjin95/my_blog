# ApiExamCaptchaNkeyResult

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

// 네이버 캡차 API 예제 - 키발급, 키 비교
public class ApiExamCaptchaNkeyResult {
```

* 캡차를 이용하는 이유는 접속하려는 사용자가 매크로가 아닌지 확인하기 위함이다.
* 이를 인증하기위해서는 사용자의 입력값과 캡차 이미지 값을 비교해야한다.
* 캡차 인증 클래스

### 생성자

```java
	
	public ApiExamCaptchaNkeyResult() {
		String clientId = "YOUR_CLIENT_ID";
        String clientSecret = "YOUR_CLIENT_SECRET";

        String code = "1"; // 키 발급시 0,  캡차 이미지 비교시 1
        String key = "YOUR_CAPTCHA_KEY"; // 캡차 키 발급시 받은 키값
        String value = "YOUR_INPUT"; // 사용자가 입력한 캡차 이미지 글자값
        String apiURL = "https://openapi.naver.com/v1/captcha/nkey?code=" + code 
                                              + "&key=" + key + "&value=" + value;

        Map<String, String> requestHeaders = new HashMap<>();
        requestHeaders.put("X-Naver-Client-Id", clientId);
        requestHeaders.put("X-Naver-Client-Secret", clientSecret);
        String responseBody = get(apiURL, requestHeaders);

        System.out.println(responseBody);
	}
```

* 클래스 자체를 화면구현 클래스에서 사용하기 위해 main메서드를 생성자로 변경하였다.
* 6번 : 이미지변경 용도 접속이므로 code값을 "1"로 하여 구분하도록 한다.
* 7-8번 : 인증을 위해서는 키 값, 사용자의 입력String값, URL이 필요하다. - URL의 쿼리스트링에는 code와 key 값, 사용자 입력값이 들어간다.

### get 메서드

```java
    private static String get(String apiUrl, Map<String, String> requestHeaders){
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

### connect 메서드

```java
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

### readBody 메서드

```java
    private static String readBody(InputStream body){
        InputStreamReader streamReader = new InputStreamReader(body);

        try (BufferedReader lineReader = new BufferedReader(streamReader)) {
            StringBuilder responseBody = new StringBuilder();

            String line;
            while ((line = lineReader.readLine()) != null) {
                responseBody.append(line);
            }

            return responseBody.toString();
        } catch (IOException e) {
            throw new RuntimeException("API 응답을 읽는데 실패했습니다.", e);
        }
    }
}
```

