# Untitled

## Manifest

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
        <activity android:name=".OrderActivity" android:label="@string/create_order"
            android:parentActivityName=".MainActivity">
        </activity>
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

* 15번에서 OrderActivity에 resource의 string의 create\_order를 라벨로 하고 있다.

## res &gt; values &gt; strings.xml

### 코드 : string.xml

```markup
<resources>
    <string name="app_name">ActionBar69</string>
    <string name="create_order">주문페이지</string>
</resources>
```

## JAVA : com.example.actionbar69

### 코드 : MainActivity

```java
package com.example.actionbar69;

import android.content.Intent;
import android.os.Bundle;
import android.util.Log;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.ShareActionProvider;
import androidx.appcompat.widget.Toolbar;
import androidx.core.view.MenuItemCompat;

public class MainActivity extends AppCompatActivity {

    private ShareActionProvider shareActionProvider = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //위 코드 이후에 아이디에 접근할 수 있다.
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);//앱바를 툴바를 재사용해 생성한다.
    }
```

```java
    public boolean onCreateOptionsMenu(Menu menu) {
        //메인 인플레이트 아이템을 앱바에 추가한다.
        getMenuInflater().inflate(R.menu.menu_main,menu);
        MenuItem menuItem = menu.findItem(R.id.action_share);
        shareActionProvider = (ShareActionProvider)MenuItemCompat.getActionProvider(menuItem);
        setShareActionIntent("저녁에 피자 주문할까?");
        return super.onCreateOptionsMenu(menu);
    }
```

* 3번 코드에서 앱바에 메뉴를 추가한다.
* 4번이 메뉴의 첫번째 아이템이다.  공유 아이템
* 5번에서 ShareActionProvider를 가져온다.
* 6번에서 인텐트를 지정한다.

```java
    private void setShareActionIntent(String s) {
        Intent intent = new Intent(Intent.ACTION_SEND);
        intent.setType("text/plain");
        intent.putExtra(Intent.EXTRA_TEXT,s);
        shareActionProvider.setShareIntent(intent);
    }
```

* 공유할 인탠트를 생성해서 메서드를 구현한다.

```java
    //액션 아이템 클릭 처리
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()){//선택된 아이템에 대한 정보
            case R.id.action_create_order:
                Intent intent = new Intent(this,OrderActivity.class);
                startActivity(intent);
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
```

```java
    public void order(View view) {
        if(R.id.btn_order==view.getId()){//view.getID - 주소번지값
            Intent intent = new Intent(this,OrderActivity.class);
            startActivity(intent);
        }
        Log.i("MainAvtivity","order 호출 성공");
    }
}
```

### 코드 : OrderActivity

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
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }
}
```

## XML : res &gt; layout

### 코드 : activity\_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <include
        layout="@layout/toolbar_main"
        android:id="@+id/toolbar"/>

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
<androidx.appcompat.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:background="@color/purple_200"
    />
```

## 결과

![](../../../.gitbook/assets/1%20%2898%29.png)

* MainActivity 
* + 버튼, '주문하기' 버튼을 누르면 OrderActivity를 불러온다.

![](../../../.gitbook/assets/2%20%2874%29.png)

* OrderActivity

