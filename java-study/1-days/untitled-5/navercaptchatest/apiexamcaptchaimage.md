# ApiExamCaptchaImage

### 선언부

```java
package naver.net;

import java.io.*;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Date;
import java.util.HashMap;
import java.util.Map;

// 네이버 캡차 API 예제 - 캡차 이미지 수신
public class ApiExamCaptchaImage {
```

* key값으로 캡차 이미지를 수신하는 클래스

### 생성자

```java
	public ApiExamCaptchaImage() {}
	
	public ApiExamCaptchaImage(String key) {
		    String clientId = "JVMdTwabK3fh1C8qVdDk"; 
        String clientSecret = "diq1lvMWsz"; 
        key = key.substring(8,  24);

        String apiURL = "https://openapi.naver.com/v1/captcha/ncaptcha.bin?key=" + key;

        Map<String, String> requestHeaders = new HashMap<>();
        requestHeaders.put("X-Naver-Client-Id", clientId);
        requestHeaders.put("X-Naver-Client-Secret", clientSecret);
        String responseBody = get(apiURL, requestHeaders);//아이디와 비번을 파라미터로 받아서 네이버가 인증과정을 거친다.->46번

        System.out.println(responseBody);
	}
```

* 이미지를 받아오려면 서버에 접근하기 위한 클라이언트 ID, 클라이언트Secret정보와 key값이 필요하다.
* 6번 : k파라미터로 받아온 key값은 문자열 상태로, 키 값만 꺼내야한다.\
  \- 넘어온 형태 : {key : keyValue}\
  \- key.substring(몇번째부터, 몇번째까지);\
  \- 8번째 string부터 24번째 string까지만 꺼내 담는다.
* 8번 : 서버 접속을 위한 URL
* 10-12번 : Map타입 변수를 HashMap으로 생성해 id와 secret값을 담는다.
* 13번 : 응답받을 변수에 URL과 id, secret을 파라미터로 넣어 get메서드를 호출한다.

### getFileNam(String key) 메서드

```java
	public String getFileName(String key) {
		String clientId = "JVMdTwabK3fh1C8qVdDk"; //애플리케이션 클라이언트 아이디값";=사용자가 입력할 값, 파라미터
		String clientSecret = "diq1lvMWsz"; //애플리케이션 클라이언트 시크릿값";=사용자가 입력할 파라미터
		key = key.substring(8,  24);//{key : djfjdsfkjsdl}에서 값만 꺼내오기 위해 자르기, 8-24번만 꺼내 key에 담는다.
		
		String apiURL = "https://openapi.naver.com/v1/captcha/ncaptcha.bin?key=" + key;
		
		Map<String, String> requestHeaders = new HashMap<>();
		requestHeaders.put("X-Naver-Client-Id", clientId);
		requestHeaders.put("X-Naver-Client-Secret", clientSecret);
		String fileName = get(apiURL, requestHeaders);//아이디와 비번을 파라미터로 받아서 네이버가 인증과정을 거친다.->46번
		
		return fileName;
	}
```

### main 메서드

```java
    public static void main(String[] args) {
        String clientId = "JVMdTwabK3fh1C8qVdDk"; //애플리케이션 클라이언트 아이디값";
        String clientSecret = "diq1lvMWsz"; //애플리케이션 클라이언트 시크릿값";

        String key = "uuC2zubirBBllhKk"; // https://openapi.naver.com/v1/captcha/nkey 호출로 받은 키값
        String apiURL = "https://openapi.naver.com/v1/captcha/ncaptcha.bin?key=" + key;

        Map<String, String> requestHeaders = new HashMap<>();
        requestHeaders.put("X-Naver-Client-Id", clientId);
        requestHeaders.put("X-Naver-Client-Secret", clientSecret);
        String responseBody = get(apiURL,requestHeaders);

        System.out.println(responseBody);
    }
```

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
                return getImage(con.getInputStream());
            } else { // 에러 발생
                return error(con.getErrorStream());
            }
        } catch (IOException e) {
            throw new RuntimeException("API 요청과 응답 실패", e);
        } finally {
            con.disconnect();
        }
    }
```

* 파라미터로 서버에 접근할 URL과 필요한 id, secret을 받는다.
* HttpURLConnection변수를 활용해 connect클래스에 URL을 파라미터로 넘겨 서버와 연결을 시도한다.
* 2번 : 네이버 서버와의 연결통로 확보
* 10번 : connect메서드의 반환값인 구분번호code가 정상호출 code라면 getImage메서드를 호출한다.\
  \- socket을 사용하지않고 http의 connect메서드를 이용해 getInputStream메서드로 들을 수 있다.
* 18번 : 연결 종료

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

* 파라미터로 받아온 URL로 서버와 연결을 시도하는 메서드

### getIamge(InputStream is) 메서드

```java
    private static String getImage(InputStream is){
        int read;
        byte[] bytes = new byte[1024];
        // 랜덤한 이름으로  파일 생성
        String filename = Long.valueOf(new Date().getTime()).toString();
        File f = new File(filename + ".jpg");
        try(OutputStream outputStream = new FileOutputStream(f)){
            f.createNewFile();
            while ((read = is.read(bytes)) != -1) {
                outputStream.write(bytes, 0, read);
            }
            //return "이미지 캡차가 생성되었습니다.";
            return filename;
        } catch (IOException e) {
            throw new RuntimeException("이미지 캡차 파일 생성에 실패 했습니다.",e);
        }
    }
```

* 파라미터로 is를 받아 듣기가 가능한 메서드
* 5번 : 랜덤한 이름으로 파일을 생성한다.
* 6번 : 파일이름뿐아니라 확장자까지 있어야 JVM이 파일을 찾을 수 있다.
* 7-11번 : 위에서 지정한 서식의 파일로 os를 구현해 말하기한다.
* 13번 : 반환값 :  생성된 이미지파일 이름.jpg

### error(InputStream body) 메서드

```java
    private static String error(InputStream body) {
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

