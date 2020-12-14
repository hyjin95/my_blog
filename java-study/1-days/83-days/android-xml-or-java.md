# Android 화면 : xml or java?

## XML

### 코드 : MainActivity2.java

```java
package com.example.intent69;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity2 extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        setContentView(R.layout.activity_main);
    }
}
```

* setContentView\( \)메서드로 xml문서를 그린다.
* 위 경우는 activity\_main.xml에 화면이 구현되어 있다.

### 코드 : Activity\_Main2.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:id="@+id/layout"
    tools:context=".MainActivity2">

    <Button
        android:id="@+id/btn_add"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="입력" />

    <Button
        android:id="@+id/btn_update"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="수정" />
</LinearLayout>
```

### JAVA

```java
package com.example.intent69;

import android.os.Bundle;
import android.widget.Button;
import android.widget.LinearLayout;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        LinearLayout linearLayout = new LinearLayout(this);
        Button btn1 = new Button(this);
        btn1.setText("입력");
        linearLayout.addView(btn1);
        Button btn2 = new Button(this);
        btn2.setText("수정");
        linearLayout.addView(btn2);
        setContentView(linearLayout);
        //setContentView(R.layout.activity_main);
    }
}
```

* JAVA에 직접 구현

### JAVA + XML

```java
package com.example.intent69;

import androidx.appcompat.app.AppCompatActivity;

import android.os.Bundle;
import android.widget.LinearLayout;

public class MainActivity2 extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        LinearLayout linearLayout = findViewById(R.id.layout);
    }
}
```

* 위와 같이 xml의 id값으로 xml 객체에 접근 할 수 도 있다.

