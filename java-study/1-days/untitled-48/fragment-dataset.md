# Fragment : 일반, DataSet

## JAVA : Activity

### 코드 : MainActivity2.java

```java
package com.example.listfragment69;

import androidx.appcompat.app.AppCompatActivity;
import androidx.fragment.app.Fragment;
import androidx.fragment.app.FragmentTransaction;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;

public class MainActivity2 extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main2);

        //프레그먼트 생성
        Fragment fragment = new BlankFragment();
        FragmentTransaction ft = getSupportFragmentManager().beginTransaction();//동기화를 위한 트랜잭션처리
        ft.add(R.id.content_frame, fragment);//activity xml에 붙일곳 id, fragment주소번지
        ft.commit();
    }

    public void move(View view){
        Intent intent = new Intent(MainActivity2.this, MainActivity.class);//나 자신, 이동할  activity
        startActivity(intent);
    }
}
```

* 20번코드에서 Fragment를 위치할 FrameLayout를 id로 지정해준다.\
  두번째 파라미터는 위치할 Fragment의 주소번지가 위치한다.
* 24번에서 이동시 호출되는 메서드를 정의한다.\
  Intent를 활용한 Activity 이동을 정의한다.

### 코드 : MainActivity.java

```java
package com.example.listfragment69;

import android.os.Bundle;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

* xml에서 Fragment를 활용한다.

## JAVA : Fragment

### 코드 : BalnkFragment.java : xml

```java
package com.example.listfragment69;

import android.os.Bundle;
import androidx.fragment.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class BlankFragment extends Fragment {

   public BlankFragment() {
        // Required empty public constructor
    }

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_blank, container, false);//일반 Fragment는 xml을 가져와 layout으로 사용한다.
    }
}
```

* xml을 갖는 Fragment

### 코드 : PizzaFragment.java : dataset

```java
package com.example.listfragment69;


import android.os.Bundle;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;

import androidx.fragment.app.ListFragment;

public class PizzaFragment extends ListFragment {

    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState){

        String names[] = {"등운동","가슴운동","하체운동"};
        ArrayAdapter<String> workout = new ArrayAdapter<>(inflater.getContext(),android.R.layout.simple_list_item_1,names);//세번째 파라미터는 배열이름
        setListAdapter(workout);//배열 어댑터를 리스트에 연결
        return super.onCreateView(inflater,container,savedInstanceState);//ListFragment의 경우에는 xml을 가져오지 않는다.
    }
}
```

* DataSet의 역할을 하는 Fragmet xml문서를 갖지 않는다.

## XML

### 코드 : activity_main2.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity2">

    <FrameLayout
        android:id="@+id/content_frame"
        android:layout_width="match_parent"
        android:layout_height="200dp"/>

    <Button
        android:onClick="move"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="이동"/>

</LinearLayout>
```

### 코드 : activity_main.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<fragment xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:name="com.example.listfragment69.PizzaFragment"/>
```

### 코드 : fragment_blank.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".BlankFragment">

    <!-- TODO: Update blank fragment layout -->
    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:text="@string/hello_blank_fragment" />

</FrameLayout>
```
