# ApiExamCaptchaNkey

```java
package naver.net;
/*
 * java.io 패키지가 있어야 읽기, 쓰기가 가능해진다.
 * java.net 패키지가 있어야 통신이 가능해진다. 외부에서(네이버, 카카오, 아마존, 도커,...) 제공하는 서비스를 이용할 수 있다.
 * 위 두 패키지를 사용할때에는 반드시 예외처리를 해주어야한다.
 */
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;//UniformResourceLocator - http프로토콜을 사용해 서비스를 제공하는 시스템에 접속하려면 필요한 패키지
import java.util.HashMap;
import java.util.Map;

// 네이버 캡차 API 예제 - 키발급
public class ApiExamCaptchaNkey {
	//생성자활용연습, 클래스쪼개기, 인터페이스와 추상클래스 중심의 코딩, Framework:Spring, Deven, AnyFrame, Struts
	
	//메인 메서드(thread) - 시작점
	/*
	 * 27-37->38->45->46->66
	 * 
	 */
    public static void main(String[] args) {
        String clientId = "JVMdTwabK3fh1C8qVdDk"; //애플리케이션 클라이언트 아이디값";=사용자가 입력할 값, 파라미터
        String clientSecret = "diq1lvMWsz"; //애플리케이션 클라이언트 시크릿값";=사용자가 입력할 파라미터

        String code = "0"; // 키 발급시 0,  캡차 이미지 비교시 1로 세팅
        //웹서버에 요청을 보낼때 주소 뒤에 물음표를 붙이고 변수이름 = 값&변수이름2 = 값2&변수이름3 = 값3 -쿼리스트링
        //쿼리스트링 : 사용자가 입력한 값을 네이버 서버에 전송할 수 있다.
        String apiURL = "https://openapi.naver.com/v1/captcha/nkey?code=" + code;

        Map<String, String> requestHeaders = new HashMap<>();
        requestHeaders.put("X-Naver-Client-Id", clientId);
        requestHeaders.put("X-Naver-Client-Secret", clientSecret);
        //네이버 서버에서 전송된 키 값이 담기는 곳, get파라미터의 리턴타입은 String이다.
        String responseBody = get(apiURL, requestHeaders);//아이디와 비번을 파라미터로 받아서 네이버가 인증과정을 거친다.->46번

        System.out.println(responseBody);//키 출력
        
        //추가된 부분-생성자를 통해 출력된 키를 넘긴다.
        ApiExamCaptchaImage aeci = new ApiExamCaptchaImage(responseBody);
    }
    
    //소켓통신 대신 웹통신을 한다.
    //http프로토콜을 사용해 소켓을 직접 생성하고 관리할 필요가 없다.
    private static String get(String apiUrl, Map<String, String> requestHeaders){
    	//네이버 서버와의 연결통로 확보 ->55번
        HttpURLConnection con = connect(apiUrl);
        try {
            con.setRequestMethod("GET");
            for(Map.Entry<String, String> header :requestHeaders.entrySet()) {
                con.setRequestProperty(header.getKey(), header.getValue());
            }
            //네이버에서 보내주는 응답코드를 받는다 - 100,200,300,... int타입
            int responseCode = con.getResponseCode();
            //정상적으로 응답이 도착했는지 체크-200 ->57번
            if (responseCode == HttpURLConnection.HTTP_OK) { // 정상 호출
            	//반환값으로 메서드 호출 ->80번
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

    //네이버 서버에 건택하는 메서드
    private static HttpURLConnection connect(String apiUrl){//apiUrl : 네이버 서버주소
        try {
            URL url = new URL(apiUrl);
            return (HttpURLConnection)url.openConnection();
        } catch (MalformedURLException e) {
            throw new RuntimeException("API URL이 잘못되었습니다. : " + apiUrl, e);
        } catch (IOException e) {
            throw new RuntimeException("연결이 실패했습니다. : " + apiUrl, e);
        }
    }
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

