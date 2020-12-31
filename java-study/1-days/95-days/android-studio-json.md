# Android Studio : JSON 가져오기

## JSON 데이터

[http://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20120101](http://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20120101)

## 설정 : gradle

### gradle.properties

```markup
android.useAndroidX=true
android.enableJetifier=true
```

### buld.gradle : Module

```java
dependencies {
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.4'
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.google.code.gson:gson:2.8.6'
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}
```

## JAVA

### 코드 : MainActivity.java

```java
package com.example.volleyjson69;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

import com.android.volley.AuthFailureError;
import com.android.volley.Request;
import com.android.volley.RequestQueue;
import com.android.volley.Response;
import com.android.volley.VolleyError;
import com.android.volley.toolbox.StringRequest;
import com.android.volley.toolbox.Volley;
import com.google.gson.Gson;

import java.util.HashMap;
import java.util.Map;

public class MainActivity extends AppCompatActivity {
    //onCreate - onStart - onResume - onPause - onStop - onDestory
    Button btn_search = null;
    //boxofficeResult 덩어리 정보
    TextView tv_json = null;//json포맷의 1차 정보 출력
    //weeklyBoxofficeList의 하위 정보 출력
    TextView tv_movie = null;//josn포맷의 2차(서브,하위)정보 출력-MovieVO
    //영화 진흥원 공공프로세스의
    TextView tv_data = null;

    /*******************************************************************************************
     *
     * @param savedInstanceState : 이전 프로세스의 상태정보를 가지고 있는 변수이름
     *                           왜 갖고잇어야 하나? 번들 상태정보를 기억하고 있어야 이전 정보를 유지한다.
     *                           재생 일시정지 후 다시 재생 할때 이전 기록이 필요하듯이
     */
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        //부모클래스 메서드 호출, 메서드 오버라이딩, 부모 생성자 호출, 멤버변수 초기화 등을 하기 위함
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //설정 작업 = layout안에 있는 화면 컴포넌트에 대한 리소스 정보를 담는다.
        //해당 액티비티의 설정작업을 하는 영역
        btn_search = findViewById(R.id.btn_search);
        tv_json    = findViewById(R.id.tv_json);
        tv_movie   = findViewById(R.id.tv_movie);
        tv_data    = findViewById(R.id.tv_data);
        btn_search.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                //List가 2개이므로 List를 2개 담아야 하기 때문에 버튼이 두번눌려야 한다?
                sendRequest();
            }
        });
        //getApplicationContext함수는 Activity안의 Fragment인클루드에서 사용할 때
        //부모액티비티를 접근하는 경우 반드시 필요한 메서드이다.
        if(AppHelper.requestQueue != null) {
            AppHelper.requestQueue = Volley.newRequestQueue(getApplicationContext());
        }
    }//////////////////////////end of oncreate

    //영화관련 공공정보를 문자열로 가져와 JOSN포맷으로 변경 후 화면 출력 테스트
    public void process(String data) {
        Gson gson = new Gson();
        MovieList movieList = gson.fromJson(data, MovieList.class);
        Log.i("TAG",movieList.toString());
        if(movieList != null) {
            for(int i=0;i<movieList.boxOfficeResult.weeklyBoxOfficeList.size();i++) {
                MovieVO mvo = movieList.boxOfficeResult.weeklyBoxOfficeList.get(i);
                tv_movie.append(mvo.getMovieNm().toString()+"\n");
            }
        }
        println("박스오피스 타입 :"+movieList.boxOfficeResult.boxofficeType);
    }

    //영화관련 공공정보제공 서버에 접속해 필요한 정보를 요청한다.
    public void sendRequest() {
        String apiURL = "http://kobis.or.kr/kobisopenapi/webservice/rest/boxoffice/searchWeeklyBoxOfficeList.json?key=f5eef3421c602c6cb7ea224104795888&targetDt=20120101";
        try {
            //volley에서 제공하는 클래스를 이용해 스레드를 직접구현하지 않고도 웹 서버에 요청을 해 응답을 가져올 수 있다.
            //일단 get방식으로 구현해 단위테스트가 가능하게 한다.
            StringRequest request = new StringRequest(Request.Method.GET,apiURL,new Response.Listener<String>() {
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

    public void println(String msg) {
        tv_data.append(msg+"\n");
    }///////////////////end of println
}

```

