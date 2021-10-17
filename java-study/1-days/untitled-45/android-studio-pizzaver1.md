# Android Studio : PizzaVer1

## Manifest

### 코드 : AndroidManifest

```markup
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.pizza201224ver1">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Pizza201224Ver1">
        
        <activity android:name=".OrderActivity"
            android:label="@string/order"
            android:parentActivityName=".MainActivity"/>
            
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

* 13-15번 : OrderActivity의 부모액티비티를 MainActivity로 지정한다.

## Java.class

### 코드 : MainAtivity.java

```java
package com.example.pizza201224ver1;

import android.content.Intent;
import android.os.Bundle;
import android.view.Menu;
import android.view.MenuItem;

import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
    }
```

* 16번에서 MainActivity가 화면으로 그릴 xml을 지정
* 17번에서 activity_main에서 toolbar라는 id를 갖는 뷰객체를 가져와 Toolbar를 선언한다.
* 28번에서 기존 ActionBar에 toolbar를 set한다.

```java
    //@param1 : 메뉴에 대한 리소스파일
    //@param2 : 메뉴 리소스 파일을 자바로 표현한 menu객체
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        getMenuInflater().inflate(R.menu.menu_main,menu);
        return super.onCreateOptionsMenu(menu);
    }
```

* 메뉴에 대한 리소스를 정의할 수 있는 메서드
* 5번에서 메뉴로 사용할 xml을를 정한다.
* 6번에서 menu_main.xml로 만든 메뉴를 반환한다.

```java
    //액션 아이템 클릭 처리
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()){//선택된 아이템에 대한 정보
            case R.id.action_create_order:
                //activity에서 activity로 이동할때 intent를 사용한다.
                Intent intent = new Intent(MainActivity.this,OrderActivity.class);
                //실제로 이동이 일어나는 함수는 startActivity( )이다.
                startActivity(intent);
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }
}
```

* 아이템 선택 이벤트를 처리하느 메서드
* 파라미터로 메뉴의 아이템을 받는다.
* 3번의 switch문에서 선택된 아이텐의 아이디를 받아온다.
* 4번 : 선택된 아이템의 아이디가 action_create_order라면
* 6번 : Intent를 생성해 이동할 Acticity클래스를 지정한다.
* 8번 : Activity이동 메서드

### 코드 : OrderActivity.java

```java
package com.example.pizza201224ver1;

import androidx.appcompat.app.ActionBar;
import androidx.appcompat.app.AppCompatActivity;
import androidx.appcompat.widget.Toolbar;

import android.os.Bundle;

public class OrderActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_order);
        Toolbar toolbar = findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        
        ActionBar actionBar = getSupportActionBar();
        //액션바 유형의 객체를 반환하고 아래 메서드에 true값을 인자로 전달해 호출한다.
        actionBar.setDisplayHomeAsUpEnabled(true);
    }
}
```

* 18번 코드로 ActionBar의 참조를 받아온다.
* 뒤로가기 버튼(홈버튼)을 추가하기위해 ActionBar의 기능을 빌려 쓰는 것이다.
* 18번에서 ActionBar 유형의 객체를 반환해주고 20번 코드로 true값을 주면 뒤로가기 버튼이 toolbar에 생성된다.
* OrderActivity화면에서 뒤로가기 버튼을 클릭하면 부모액티비티인 MainActivity로 이동한다.

## res > layout

### 코드 : activity_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <include layout="@layout/toolbar_main"
        android:id="@+id/toolbar"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />

</LinearLayout>
```

* includ 태그를 이용해 res > layout내의 xml을 id로 접근해 가져와 재사용할 수 있다.
* res에서 가져오는 id값이기 때문에 '@+'를 붙여야 한다.

### 코드 : activity_order.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".OrderActivity">

    <include layout="@layout/toolbar_main"
        android:id="@+id/toolbar"/>

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="주문페이지 입니다."
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

* 부모 액티비티와 자식 액티비티의 관계를 이용해 화면을 그릴때에는 constraintLayout이 적합하다.
* 16-19번 코드처럼 부모와의 관계에 layout을 설정 할 수 있기 때문이다.

### 코드 : toolbar_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<androidx.appcompat.widget.Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:background="?attr/colorPrimary"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"><!-- 툴바는 화면에 높이를 채우지 않는다. 위쪽에만 액션바 사이즈만큼 둔다 -->
</androidx.appcompat.widget.Toolbar>
```

* ActionBar대신 사용할 Toolbar를 사용하기 위해 생성한 리소스 문서
* 4번 : 툴바의 넓이는 기기와 맞기만 하면 된다.
* 5번 : 툴바는 화면의 상단에만 존재해야하므로 기기와 맞추면 안된다.\
  ?attr/actionBarSize 값을 줘 액션바와 같은 크기를 갖게 한다.

## res > menu

### 코드 : menu_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">
    <item
        android:title="@string/order"
        android:id="@+id/action_create_order"
        android:icon="@drawable/ic_add_white_24dp"
        android:orderInCategory="1"
        app:showAsAction="ifRoom"/>
</menu>
```

* 7번 아이콘을 아이템으로서 자리가 있다면 첫번째 카테고리에 붙인다.

## res > values

### 코드 : strings.xml

```markup
<resources>
    <string name="app_name">Pizza201224Ver1</string>
    <string name="order">주문서</string>
</resources>
```

## res > themes

### 코드 : themes.xml

```markup
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Theme.Pizza201224Ver1" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Primary brand color. -->
        <item name="colorPrimary">@color/purple_500</item>
        <item name="colorPrimaryDark">@color/purple_700</item>
        <item name="colorAccent">@color/teal_200</item>
        <!-- Customize your theme here. -->
    </style>
</resources>
```

## 결과 : 애뮬레이터

![](<../../../.gitbook/assets/1 (100).png>)

* 시작 화면. MainActivity
* 오른쪽 상단에 + 아이콘이 붙어있다.
* 아이콘을 클릭하면 OrderActivity로 이동한다.

![](<../../../.gitbook/assets/2 (77).png>)

* OrderActivity화면
* 왼쪽 상단에 홈버튼(뒤로가기)이 붙어있다.
* 뒤로가기를 누르면 부모액티비티인 MainActivity로 이동한다.
