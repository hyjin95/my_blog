# NaverCaptchaTest - 이미지 새로고침

### 선언부

```java
package naver.net;

import java.awt.BorderLayout;
import java.awt.Container;
import java.awt.Image;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.HashMap;
import java.util.Map;

import javax.swing.BorderFactory;
import javax.swing.ImageIcon;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JTextField;

public class NaverCaptcharTest extends JFrame implements ActionListener {
	
	JPanel     jp_south    = new JPanel();
	JLabel     jlb         = new JLabel("");
	JButton    jbtn_reload = new JButton("새로고침");
	JButton    jbtn_okay   = new JButton("확인");
	JTextField jtf_enter   = new JTextField();
	
	 Container cont = this.getContentPane();	
	
	 String path = "C:\\workspace_java\\dev_java\\";//캡처이미지가 생성된 경로
```

* 34번 : 새로고침을 위해 Container인스턴스 변수를 this(JFrame)의 패널을 읽어오는 getContentPane메서드로 구현한다.
* 36번 : 캡처이미지가 생성되는 주소가 필요하다.

### 생성자

```java
	public NaverCaptcharTest() {
		initDisplay();
	}////////////////////////end of 생성자//////////////////////////////

```

### 화면구현부

```java
	public void initDisplay() {
		String fileName = getImage();
		//이미지 미리보기 시작 처리
		ImageIcon originIcon = new ImageIcon(path+fileName);
		Image originImg = originIcon.getImage();
		Image changeImg = originImg.getScaledInstance(350, 150, Image.SCALE_SMOOTH);
		//새로운 Image로 ImageIcon객체 생성
		ImageIcon icon = new ImageIcon(changeImg);
		jlb.setIcon(icon);
		
		jbtn_reload.addActionListener(this);
		
		jlb.setBorder(BorderFactory.createEmptyBorder());
		
		jp_south.setLayout(new BorderLayout());
		jp_south.add("West",jbtn_reload);
		jp_south.add("Center",jtf_enter);
		jp_south.add("East",jbtn_okay);
		
		this.add("Center", jlb);
		this.add("South", jp_south);
		this.setSize(350, 250);
		this.setVisible(true);
	}

	public static void main(String[] args) {
		new NaverCaptcharTest();
```

* JLabel에 image를 붙이기위한 작업이 필요하다.
* 4번 : ImageICon클래스의 인스턴스변수를 멤버변수의 이미지 경로+이미지이름 변수로 생성한다.
* 5번 : Image타입의 변수에 ImageIcon에서 추출한 image를 담는다.
* 7번 : 추출한 image를 getScaledInstance메서드로 크기조절하여 새로운 image객체를 생성한다.
* 8번 : 새롭게 정의한 image를 새 ImageIcon클래스 변수에 정의한다.
* 9번 : JLabel에 setIcon메서드를 이용해 이미지를 붙인다.
* 13번 : JLable의 Layout의 효과를 비운다.

### getImage메서드 - key발급

```java
	public String getImage() {
		//key받아오는 코드
		String clientId = "JVMdTwabK3fh1C8qVdDk"; //애플리케이션 클라이언트 아이디값";
    String clientSecret = "diq1lvMWsz"; //애플리케이션 클라이언트 시크릿값";

    String code = "0"; // 키 발급시 0,  캡차 이미지 비교시 1로 세팅
    
    String apiURL = "https://openapi.naver.com/v1/captcha/nkey?code=" + code;

    Map<String, String> requestHeaders = new HashMap<>();
    requestHeaders.put("X-Naver-Client-Id", clientId);
    requestHeaders.put("X-Naver-Client-Secret", clientSecret);
    String responseBody = get(apiURL,requestHeaders);     
        
    System.out.println(responseBody);
        
		ApiExamCaptchaImage aci = new ApiExamCaptchaImage();
		String fileName = aci.getFileName(responseBody);//새로고침할때마다 바뀐다.
		fileName = fileName+".jpg";
		
		System.out.println(fileName);
		
		return fileName;
	}
```

* key를 발급받는 메서드를 구현
* key를 발급받으려면 clientID와 clientSecret이 필요하다.
* 6번 : code변수로 요청한 일이 key발급인지, 값비교인지 구분한다.
* 9번 : 사용자가 입력한 값을 네이버서버에 전송하기 위해 쿼리스트링을 작성한다.
* 10번 : String,String타입의 Map 함수를 제공한다.\
  \- HashMap타입 변수로,  동기화를 지원하지않는다.
* 11,12번 : put메서드를 이용해 지정된 key값과 id, secret을 담은 변수를 넣어준다.
* 13번 : responseBody변수는 get메서드를 호출한다.\
  \- 파라미터에 apiURL와 id,Secret이 담긴 HashMap변수가 담긴다.\
  \- get메서드는 key값을 반환하는 메서드이다.
* 15번 : key값 출력 단위테스트
* 17,18번 : Image를 발급받는 클래스를 호출하여 파라미터 key값으로 이미지이름을 받아온다.\
  \- 이미지 발급 클래스.getFileName(responseBody=key)
* 19번 : file이름만 넘어온것으로, JVM이 이미지를 찾으려면 확장자까지 필요하다.\
  \- String값은 원본이 변하지않으므로 반드시 대입연산자를 이용해 값을 변경한다.
* 21번 : fileName가 새로고침 되었는지 단위테스트

### actionPerformed - 새로고침

```java
	@Override
	public void actionPerformed(ActionEvent ae) {
		Object obj = ae.getSource();
		if(obj==jbtn_reload) {
			System.out.println("새로고침 이벤트 호출");
			String fileName = getImage();
			ImageIcon originIcon = new ImageIcon(path+fileName);
			Image originImg = originIcon.getImage();
			Image changeImg = originImg.getScaledInstance(350, 150, Image.SCALE_SMOOTH);
			ImageIcon icon = new ImageIcon(changeImg);
			jlb.setIcon(icon);
			cont.revalidate();
		}
	}
}
```

* 4-11번 : 새로고침 이벤트가 발생하면 getImage메서드를 실행해 새로운 fileName을 받아와 jlb에 새 이미지를 set한다.
* 12번 : cont.ravalidate( )메서드로 화면 레이아웃을 새 이미지로 재설정한다.

### Nkey, NkeyResult클래스에서 가져온 메서드

```java
	//////////////////////////가져온 메서드 key 발급/////////////////////////////////
    private static String get(String apiUrl, Map<String, String> requestHeaders){
    	//네이버 서버와의 연결통로 확보
        HttpURLConnection con = connect(apiUrl);
        try {
            con.setRequestMethod("GET");
            for(Map.Entry<String, String> header :requestHeaders.entrySet()) {
                con.setRequestProperty(header.getKey(), header.getValue());
            }
            //네이버에서 보내주는 응답코드를 받는다 - 100,200,300,... int타입
            int responseCode = con.getResponseCode();
            //정상적으로 응답이 도착했는지 체크
            if (responseCode == HttpURLConnection.HTTP_OK) { // 정상 호출
            	  //반환값으로 메서드 호출
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

```java
    //네이버 서버에 건택하는 메서드, apiUrl : 네이버 서버주소
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

```java
    /*
     * @return 으로 받는것은 키 값
     * @param 으로 받는 것은 서버에서 전송된 정보
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
    /////////////////////////가져온 메서드 key발급///////////////////////////////
```
