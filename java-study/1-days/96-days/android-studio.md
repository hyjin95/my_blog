# Android Studio

## JAVA

```java
package com.example.stopwatchbefore69;

import android.os.Bundle;
import android.os.Handler;
import android.view.View;
import android.widget.TextView;

import androidx.appcompat.app.AppCompatActivity;

import java.util.Locale;

public class MainActivity extends AppCompatActivity {

    //액티비티의 라이프 사이클 이해하기
    //상태벙조를 활용하기 실습
    //스탑워치에서 관리해야하는 상태값은 무엇이 있을까?
    //표시할 초 정보 : 초기화시 0으로 리셋되어야 한다.
    //실행중인 상태인지 상태관리 : true(실행상태), false(정지상태, 리셋상태)
    //이런 정보는 액티비티에 동기화된 정도 이여야 한다.

    private int seconds = 0;
    private boolean running = false;//실행상태

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        runTimer();
    }

    //시작 버튼 클릭시
    public void start(View v) {
        running = true;
    }

    //정지 버튼 클릭시
    public void stop(View v) {
        running = false;
    }

    //초기화 버튼 클릭시
    public void reset(View v) {
        //시간은 0으로 초기화하고
        seconds = 0;
        //실행 상태는 멈춤상태이다.
        running = false;
    }

    public void runTimer() {
        //내부 클래스에 접근하려면 final선언을 해야한다.
        final TextView tv_time = findViewById(R.id.tv_time);
        final Handler handler = new Handler();

        handler.post(new Runnable() {
            @Override
            public void run() {
                int hours = seconds/3600;
                int minutes = (seconds%3600)/60;
                int secs = seconds%60;

                String time = String.format(Locale.getDefault(), "%d:%02d:%02d",hours,minutes,secs);
                tv_time.setText(time);
                if(running)
                    seconds++;//진행중이면 1씩 증가

                //1000밀리세트 이후에 Runnable코드를 실행하도록 postDelayed를 호출한다.
                //이 코드가 run메서드 안에 정의되어있기때문에 반복 실행된다.
                //왜냐하면 스레드가 종료되지 않으므로
                handler.postDelayed(this, 1000);
            }
        });
    }
}
```

## XML

### activity\_main.xml 

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tv_time"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:textSize="56dp"
        android:text="00:00:00" />

    <Button
        android:id="@+id/btn_start"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="start"
        android:text="시작" />

    <Button
        android:id="@+id/btn_stop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="stop"
        android:text="정지" />

    <Button
        android:id="@+id/btn_reset"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:onClick="reset"
        android:text="초기화" />

</LinearLayout>
```

