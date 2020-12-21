# Untitled

### Manifests

### 코드 : AndroidManifest.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.actionbar69">

    <!-- 인터넷 사용 인증 클라우드 플랫폼을 이용하기 위해서는 인터넷이 필수-->
    <uses-permission android:name="android.permission.INTERNET"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ActionBar69">
        <activity android:name=".OrderActivity"></activity>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## Activity - JAVA

### 코드 : MainActivity.java

```java
package com.example.actionbar69;


import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.View;

import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //위 코드 이후에 아이디에 접근할 수 있다.
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);//앱바를 툴바를 재사용해 생성한다.
    }

    public void order(View view) {
        if(R.id.btn_order==view.getId()){//view.getID - 주소번지값
            Intent intent = new Intent(this,OrderActivity.class);
            startActivity(intent);
        }
        Log.i("MainAvtivity","order 호출 성공");
    }
}
```

### 코드 : OrderActivity.java

```java
package com.example.actionbar69;

import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;

import android.os.Bundle;
import android.util.Log;

public class OrderActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_order);
        Toolbar toolbar = findViewById(R.id.common_tbar);
        setSupportActionBar(toolbar);
    }
}
```

## Activity - XML

### 코드 : activity\_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="@color/teal_200"
        /><!-- attr/actionBarSize - appbar크기만큼, 리소스(기본제공색상)는 앞에 @를 붙여 접근 가능하다. -->

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

    <Button
        android:id="@+id/btn_order"
        android:onClick="order"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="주문하기" />

</LinearLayout>
```

### 코드 : activity\_order.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".OrderActivity">

    <include layout="@layout/toolbar_main"
        android:id="@+id/toolbar"/>

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="주문페이지입니다."
        tools:layout_editor_absoluteX="190dp"
        tools:layout_editor_absoluteY="170dp" />
</LinearLayout>
```

### 코드 : toolbar\_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<androidx.appcompat.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/common_tbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="@color/purple_200"
    />
```

## res &gt; values &gt; themes.xml

```markup
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.ActionBar69" parent="Theme.AppCompat.Light.NoActionBar">
    <!--<style name="Theme.ActionBar69" parent="Theme.AppCompat.Light.NoActionBar">-->
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryDark">@color/purple_700</item>
        <item name="colorAccent">@color/teal_200</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```

