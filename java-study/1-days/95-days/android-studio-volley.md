# Android Studio : Volley

## 설정

### Manifest

```java
<uses-permission android:name="android.permission.INTERNET" />
```

* 사용하기 위해서는 Manifest에 인터넷 권한을 부여해야한다.

### build.gradle : Module

```java
implementation 'com.android.volley:volley:1.1.1'
```

* build.gradle : Module에 volley라이브러리를 추가한다.

## JAVA

### AppHelper

```java
package com.example.volleyjson69;

import com.android.volley.RequestQueue;

public class AppHelper {
    public static RequestQueue requestQueue = null;
}
```

* 한번 생성하면 어디서든 접근이 가능하도록 AppHelper클래스에 static으로 선언한다.

### MainActivity

```java
 @Override
    protected void onCreate(Bundle savedInstanceState) {
   
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        btn_search = findViewById(R.id.btn_search);
        btn_search.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                sendRequest();
            }
        });

        //getApplicationContext함수는 Activity안의 Fragment인클루드에서 사용할 때
        //부모액티비티를 접근하는 경우 반드시 필요한 메서드이다.
        if(AppHelper.requestQueue != null) {
            AppHelper.requestQueue = Volley.newRequestQueue(getApplicationContext());
        }
    }//////////////////////////end of oncreate
```

* 17번 : 사용할 Activity.java에서 requestQueue를 생성한다.
* 7번 : 이벤트를 감지할 버튼을 xml리소스로 초기화한다.
* 8-13번에서 버튼에 onClickListener를 부여해 이벤트 감지시 요청하도록 구현 한다. 11번의 sendRequest\( \)로 요청한다.

```java
//영화관련 공공정보제공 서버에 접속해 필요한 정보를 요청한다.
    public void sendRequest() {
        String apiURL = "http://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20120101";
        try {
            //volley에서 제공하는 클래스를 이용해 스레드를 직접구현하지 않고도 웹 서버에 요청을 해 응답을 가져올 수 있다.
            //일단 get방식으로 구현해 단위테스트가 가능하게 한다.
            StringRequest request = new StringRequest(Request.Method.GET,apiURL
            ,new Response.Listener<String>() {
                @Override
                public void onResponse(String response) {
                    println("응답 : "+response);//전체 보기
                    Log.i("TAG",response);
                    process(response);
                }
            }   , new Response.ErrorListener(){
                public void onErrorResponse(VolleyError error){
                    println("에러 : "+error.getMessage());
                }
            }){
                public Map<String,String> getParams() throws AuthFailureError {
                    Map<String,String> params = new HashMap<>();
                    return params;
                }
            };
            Log.i("Volley","53번까지");
            //이전 결과 있어도 새로 요청하여 응답을 보여준다.
            request.setShouldCache(false);
            RequestQueue requestQueue = Volley.newRequestQueue(this);
            requestQueue.add(request);
        }catch(Exception e){
            Log.i("Volley",e.toString());
        }
    }//////////////////////end of sendRequest
```

* sendRequest메서드에서는 7번에서 request객체를 먼저 생성한다. - 파라미터 : 요청방식\(GET, POST\), 요청보낼 url, 응답시 호출될 메서드, 에러시 호출될 메서드
* 8-14번 :  응답시 호출될 메서드를 정의했다. Response.Listener
* 15-18번 : 에러시 호출될 메서드를 정의했다. Response.ErrorListener
* 19-24번 : 요청시 포함할 파라미터를 정의했다. getParames\( \)매서드를 재정의해 Map에 데이터를 담아 return한다.
* 27번은 이미 요청한 결과가 있더라도 다시 요청이 들어오면 새로 요청을 하게 하는 코드이다. 28-29번에서 새 요청을 위해 RequestQueue를 초기화하고 생성된 새 요청을 add한다.

