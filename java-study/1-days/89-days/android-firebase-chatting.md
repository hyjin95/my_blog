# Android : Firebase Chatting

### 순서

1. AndroidX프로젝트로 변경한다.
2. firebase에서 새 프로젝트를 생성해 연동한다.

{% page-ref page="../untitled-46/firebase-android.md" %}

## Manifest

### 코드 : AndroidManifest.java

```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.firebasechat201221">

    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.FirebaseChat201221">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## Activity.java

### 코드 : MainActivity.java

```java
package com.example.firebasechat201221;

import android.os.Bundle;
import android.view.View;
import android.widget.ArrayAdapter;
import android.widget.Button;
import android.widget.EditText;
import android.widget.ListView;

import androidx.annotation.NonNull;
import androidx.annotation.Nullable;
import androidx.appcompat.app.AppCompatActivity;

import com.google.firebase.database.ChildEventListener;
import com.google.firebase.database.DataSnapshot;
import com.google.firebase.database.DatabaseError;
import com.google.firebase.database.DatabaseReference;
import com.google.firebase.database.FirebaseDatabase;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;

public class MainActivity extends AppCompatActivity {
    private Button btn_send = null;
    private EditText et_send = null;
    private ListView lv_list = null;
    //데이터셋 구성에 필요한 선언하기
    //ListView와 데이터 연계에 필요함.
    private ArrayList<String> list = new ArrayList<>();
    private ArrayAdapter<String> arrayAdapter = null;

    //데이터 베이스 연동 추가
    private DatabaseReference refrence = FirebaseDatabase.getInstance().getReference().child("message");
    //채팅에 필요한 변수
    private String name, chat_msg, chat_user = null;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btn_send = findViewById(R.id.btn_send);
        et_send = findViewById(R.id.et_send);
        lv_list = findViewById(R.id.lv_list);

        arrayAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_list_item_1,list);
        lv_list.setAdapter(arrayAdapter);

        name = "TOMATO";
        btn_send.setOnClickListener(new View.OnClickListener(){

            @Override
            public void onClick(View v) {
                Map<String,Object> map = new HashMap<>();
                String key = refrence.push().getKey();
                refrence.updateChildren(map);
                DatabaseReference root = refrence.child(key);
                Map<String,Object> smap = new HashMap<>();
                smap.put("name",name);
                smap.put("text",et_send.getText().toString());//사용자가 입력한 메시지 담기
                root.updateChildren(smap);
                //메시지 입력 초기화
                et_send.setText("");
            }
        });

        refrence.addChildEventListener(new ChildEventListener() {
            @Override
            public void onChildAdded(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {
                chatConversation(snapshot);
            }

            @Override
            public void onChildChanged(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {
                chatConversation(snapshot);
            }

            @Override
            public void onChildRemoved(@NonNull DataSnapshot snapshot) {

            }

            @Override
            public void onChildMoved(@NonNull DataSnapshot snapshot, @Nullable String previousChildName) {

            }

            @Override
            public void onCancelled(@NonNull DatabaseError error) {

            }
        });

    }////////////////////end of onCreate

    private void chatConversation(DataSnapshot dataSnapshot){
        Iterator iter = dataSnapshot.getChildren().iterator();
        while(iter.hasNext()){
            chat_user = (String)((DataSnapshot)iter.next()).getValue();
            chat_msg = (String)((DataSnapshot)iter.next()).getValue();
            arrayAdapter.add("["+chat_user+"] : "+chat_msg);
        }////end of while
        arrayAdapter.notifyDataSetChanged();
    }
}
```

## XML

### activity\_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ListView
        android:id="@+id/lv_list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/layout"
        />
    <LinearLayout
        android:id="@+id/layout"
        android:layout_alignParentBottom="true"
        android:orientation="horizontal"
        android:padding="7dp"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <EditText
            android:id="@+id/et_send"
            android:hint="메시지를 입력하세요"
            android:layout_weight="1"
            android:layout_width="0dp"
            android:layout_height="wrap_content"/>
        <Button
            android:id="@+id/btn_send"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="전송"/>
    </LinearLayout>
</RelativeLayout>
```

##  결과

### 에뮬레이터

![](../../../.gitbook/assets/1%20%2899%29.png)

* 기존에 작성한 채팅도 인터넷이 연결되어 있다면 불러올 수 있다.

### firebase

![](../../../.gitbook/assets/3%20%2856%29.png)

