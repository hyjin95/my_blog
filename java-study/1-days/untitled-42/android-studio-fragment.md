# Android Studio : Fragment

## JAVA

### 코드 : MainActivity.java

```java
package com.example.fragment69;

import android.content.Intent;
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentTransaction;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        //이부분 이 있으면 동기화 할 수 있다. jsp의 include같은것
        Fragment fragment = new LoginFragment();
        FragmentTransaction ft = getSupportFragmentManager().beginTransaction();
        ft.add(R.id.content_frame,fragment);
        ft.commit();
    }

    public void subMove(View view){
        Intent intent = new Intent(MainActivity.this, SubActivity.class);
        startActivity(intent);
    }
}
```

### 코드 : SubActivity.java

```java
package com.example.fragment69;

import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentTransaction;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;

public class SubActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sub);

        Fragment fragment = new LoginFragment();
        FragmentTransaction ft = getSupportFragmentManager().beginTransaction();
        ft.add(R.id.sub_frame,fragment);
        ft.commit();
    }
}
```

## XML

### 코드 : activity\_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <FrameLayout
        android:id="@+id/content_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <Button
            android:id="@+id/btn_move"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="subMove"
            android:text="이동" />
    </FrameLayout>

</LinearLayout>
```

### 코드 :  activity\_sub.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".SubActivity">

    <FrameLayout
        android:id="@+id/sub_frame"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </FrameLayout>

</LinearLayout>
```

### 코드 : fragment\_login.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".LoginFragment">

    <TableLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent" >

            <TextView
                android:id="@+id/textView"
                android:textSize="24sp"
                android:background="#F44336"
                android:textColor="#FFFFFF"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="아이디" />

            <EditText
                android:id="@+id/et_id"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:ems="10"
                android:inputType="textPersonName" />
        </TableRow>

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent" >

            <TextView
                android:id="@+id/textView2"
                android:textSize="24sp"
                android:background="#3F51B5"
                android:textColor="#FFFFFF"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="비밀번호" />

            <EditText
                android:id="@+id/et_pw"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:ems="10"
                android:inputType="textPersonName" />
        </TableRow>
    </TableLayout>
</FrameLayout>
```

