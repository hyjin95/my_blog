# Android Studio : data, Event, Listener, Adapter

## manifests

### 코드 : AndroidManifest.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.coffee69">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Coffee69">
        <activity android:name=".FoodCategoryActivity"></activity>
        <activity android:name=".EditActivity" />
        <activity android:name=".CategoryActivity" />
        <activity android:name=".TopActivity"> <!-- 런처마다 어플이 생성된다. 런처 = 메인 -->
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## Activity

### 코드 : TopActivity.java - main

```java
package com.example.coffee69;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ListView;

public class TopActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_top);
        //OnItemClickListener생성
        AdapterView.OnItemClickListener itemClickListener = new AdapterView.OnItemClickListener() {//어댑터 코드
            @Override
            public void onItemClick(AdapterView<?> listView, View itemView, int position, long id) {
                //0:drinks, 1:food, 2:stores
                if(position == 0){
                    Intent intent = new Intent(TopActivity.this,CategoryActivity.class);
                    startActivity(intent);
                }
                else if(position == 1){
                    Intent intent = new Intent(TopActivity.this,FoodCategoryActivity.class);
                    startActivity(intent);
                }
            }
        };//end of OnItemClickListener
        //리스너를 리스트뷰에 매핑
        ListView listView = findViewById(R.id.list_options);
        listView.setOnItemClickListener(itemClickListener);//이벤트 처리 클래스 주소번지
    }
}
```



