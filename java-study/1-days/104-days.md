---
description: 2021.01.18 - 105일차
---

# 105 Days - Android QR

### 사용 프로그램

* 사용언어 : JAVA(JDK)1.8.0\_261, JS, JQuery, JSP, Servlet, HTML, JSON
* 사용Tool  - Eclipse : Eclipse.org, Toad DBA Suite for Oracle 11.5 , Spring, Android Studio, Visual Studio Code, Nexacro
* 사용 서버 - WAS : Tomcat

## Android : QR

### 앱

* 웹앱\
  네트워크, 인터넷 연결을 모두 종료하고 페이지를 호출하면 컨텐츠를 확인할 수 없다.
* 네이티브앱\
  디바이스가 제공하는 카메라, gps등을 사용할 수 있다.
* 하이브리드 앱\
  Activity(Fragment+web view), Session을 사용할 수 있다.

### Manifest : Permission

```markup
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.CAMERA"/>
```

### MainActivity

```java
/*
xml로 화면을 그리게되면 데이터베이스에서 가져온 정보를 기준으로 화면을 그릴 수 없다.
따라서 이런 경우에는 레이아웃을 그려주는 xml문서를 사용하지 않고 자바코드로 화면을 그려야 한다.
레이아웃 객체를 인스턴스화해야 된다.
또 for문도 사용이 불가능하므로 java코드로 구현하는 방법도 확인해두어야 한다.
동적 화면도 구현할 수 있다.
 */
public class MainActivity extends AppCompatActivity {

    private IntentIntegrator qrScan = null;//qr스캔시 사용

    private Button createQR2 = null;
    private Button scanQR = null;
```

* 선언부

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
       // LinearLayout linearLayout = new LinearLayout(this);
        setContentView(R.layout.activity_main);
        //setContentView(linearLayout);

        /*
        Activity안의 Fragment로 화면을 그린 경우
        View view = getView();
        createQR = view.findViewById(r.id.createQR);
        액티비티내 부분페이지로 프래그먼트 사용시 액티비티의 컴포넌트 접근시
         */
        createQR2 = findViewById(R.id.createQR2);
        createQR2.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                String content = ((EditText)findViewById(R.id.et_url)).getText().toString();
                if(content.isEmpty()){
                    Toast.makeText(MainActivity.this, "문자를 입력하세요.",Toast.LENGTH_LONG).show();
                    return;//함수탈출
                }else {
                    generateQRCode(content);
                }
            }
        });
```

* createQR2버튼을 클릭하면 QR을 제작할 주소가 입력되어있는지 체크 후 생성하는 메서드를 호출한다.

```java
        scanQR = findViewById(R.id.scanQR);
        scanQR.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent intent = new Intent(MainActivity.this,ScanActivity.class);
                startActivity(intent);
            }
        });
    }
```

* scanQR버튼을 누르면 QR을 스캔할수 있는 Activity로 이동한다.

```java
    private void generateQRCode(String content) {
        QRCodeWriter qrCodeWriter = new QRCodeWriter();
        //한글처리 예외처리 필수
        try {
            content = new String(content.getBytes("UTF-8"), "ISO-8859-1");
            Bitmap bitmap = toBitmap(qrCodeWriter.encode(content, BarcodeFormat.QR_CODE,200,200));//이미지 생성
            ((ImageView)findViewById(R.id.iv_qrcode)).setImageBitmap(bitmap);
        }catch(WriterException we){

        }catch(Exception e){

        }
    }
```

* createQr2버튼에서 입력값 체크 후 이미지 뷰로 QR코드를 생성하는 메서드를 호출해 생성한다.

```java
    private Bitmap toBitmap(BitMatrix matrix) {
        int height = matrix.getHeight();
        int width = matrix.getWidth();
        Bitmap bmp = Bitmap.createBitmap(width, height, Bitmap.Config.RGB_565);
        for (int x = 0; x < width; x++) {
            for (int y = 0; y < height; y++) {
                bmp.setPixel(x, y, matrix.get(x, y) ? Color.BLACK : Color.WHITE);//참이면 검정 아니면 화이트 : 메세지면 검정, 아닌부분은 화이트
            }
        }
        return bmp;
    }
}
```

* QR코드를 만들어주는 메서드

### ScanActivity

```java
public class ScanActivity extends AppCompatActivity {
    
    Button btn_move = null;
    Button btn_exit = null;
    //네이티브앱 이지만 내용은 웹앱이다.
    WebView wv_content = null;
    IntentIntegrator intentIntegrator = null;
    String qrContent = null;//네이티브앱
```

* 선언부

```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_scan);
        
        btn_move = findViewById(R.id.btn_move);
        btn_exit = findViewById(R.id.btn_exit);

        btn_exit.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if(wv_content.canGoBack()) wv_content.goBack();
                else finish();
            }
        });
```

* 이 액티비티가 시작될때 호출 되는 메서드 
* btn_exit버튼은 나가기 버튼이다.

```java
        btn_move.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                wv_content = findViewById(R.id.wv_qr);
                WebSettings webSettings = wv_content.getSettings();
                //자바스크립트를 사용할 수 있도록 설정하기
                webSettings.setJavaScriptEnabled(true);
                webSettings.setSupportMultipleWindows(true);//멀티윈도우
                webSettings.setSupportZoom(true);//확대 축소
                webSettings.setBuiltInZoomControls(true);//안드로이드에서 제공되는 줌 아이콘 설정
                webSettings.setLoadsImagesAutomatically(true);//웹뷰가 앱에 등록된 이미지 리소스인식
                webSettings.setUseWideViewPort(true);
                webSettings.setCacheMode(WebSettings.LOAD_NO_CACHE);//웹뷰가 캐시를 사용하지 않도록 설정
                wv_content.setWebViewClient(new WebViewClient());
                wv_content.setWebChromeClient(new WebChromeClient());
                wv_content.loadUrl(qrContent);
            }
        });///////////////////end of event
        intentIntegrator = new IntentIntegrator(this);
        intentIntegrator.setOrientationLocked(false);
        intentIntegrator.setPrompt("QR코드에 맞추세요");
        intentIntegrator.initiateScan();
    }
```

* onCreate안에 정의된 btn_move버튼의 이벤트 처리 및 초기 액티비티 설정
* btn_move버튼을 누르면 웹 뷰에대한 설정을 하고 url을 load한다.
* 19-22번 코드가 작성되어야 QR스캔이 가능하다.

```java
    @Override
    public void onActivityResult(int requestCode, int resultCode, Intent data){
        IntentResult result =
                IntentIntegrator.parseActivityResult(requestCode,resultCode,data);
        if(result !=null){
            if(result.getContents() == null){//스캔한 결과가 없을 때
                Toast.makeText(this,"Cancelled",Toast.LENGTH_LONG).show();
            }else{
                Toast.makeText(this,"Scanned : "+result.getContents(),Toast.LENGTH_LONG).show();
                qrContent = result.getContents();
            }
        }else{
            super.onActivityResult(requestCode,resultCode,data);
        }
    }
}
```

* 활동, 작업 결과를 내보내주는 메서드

후기 : 촉박한 시간과 파이널 프로젝트로 하루하루 정신이 없다 ㅠㅠㅠㅠㅠㅠ 조금만 더 화이팅하자!!!
