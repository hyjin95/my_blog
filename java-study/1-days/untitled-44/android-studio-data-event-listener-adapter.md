# Android Studio : data, Event, Listener, Adapter

## manifests : xml

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

## Activity : java

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

### 코드 : CategoryActivity.java

```java
package com.example.coffee69;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.AdapterView;
import android.widget.ArrayAdapter;
import android.widget.ListView;

public class CategoryActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_category);

        //아답터 사용해보기
        ArrayAdapter<Drink> listAdapter = new ArrayAdapter<Drink>(this
                                                                  ,android.R.layout.simple_list_item_1
                                                                  ,Drink.dinks);
        ListView listView = findViewById(R.id.list_drinks);
        listView.setAdapter(listAdapter);//조립  -> 그 다음 리스너생성해야댐
        
        //리스너 생성
        AdapterView.OnItemClickListener itemClickListener = new AdapterView.OnItemClickListener() {//어댑터 코드
            @Override
            public void onItemClick(AdapterView<?> listView, View itemView, int position, long id) {
                //사용자가 클릭한 음료를 EditActivity로 전
                Intent intent = new Intent(CategoryActivity.this,EditActivity.class);//다른 액티비티와 연계
                //intent.putExtra(name:"",id);
                //CategoryActivity에서도, EditActivity에서도 같은 이름을 사용 하게 함
                //사용자가 선택한 항목의 id를 인탠트에 추, Drinks배열의 인덱스가 id
                intent.putExtra(EditActivity.EXTRA_DRINKID,(int)id);//변수 넣기
                startActivity(intent);
            }
        };//end of OnItemClickListener
        //리스너를 리스트뷰에 매핑
        listView.setOnItemClickListener(itemClickListener);//이벤트 처리 클래스 주소번지
    }
}
```

### c코드 : Drink.java -getter, setter

```java
package com.example.coffee69;

public class Drink {
    private String name = null;
    private String description = null;
    private int    imgResourceId = 0;
    
    public static final Drink[] dinks = {
             new Drink("Latte","라떼",R.drawable.latte)
           , new Drink("Filter","필터",R.drawable.filter)
           , new Drink("Cappuchino","카푸치노",R.drawable.cappuchino)
    };

    public Drink() { }

    public Drink(String name, String description, int imgResourceId) {
        this.name = name;
        this.description = description;
        this.imgResourceId = imgResourceId;
    }

    public String toString() {
        return this.name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public int getImgResourceId() {
        return imgResourceId;
    }

    public void setImgResourceId(int imgResourceId) {
        this.imgResourceId = imgResourceId;
    }
}
```

### 코드 : EditActivity.java

```java
package com.example.coffee69;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ImageView;
import android.widget.TextView;

public class EditActivity extends AppCompatActivity {
    
    public static final String EXTRA_DRINKID = "drinkid";//액티비티간의 멤버변수 사용해보기

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_edit);

        //인텐트에 담긴 정보 꺼내기
        Intent intent = getIntent();
        int drinkid = (Integer)intent.getExtras().get(EXTRA_DRINKID);
        int drinkid2 = (Integer)getIntent().getExtras().get(EXTRA_DRINKID);
        Drink drink = Drink.dinks[drinkid2];//0,1,2
        //음료이름 가져오기
        TextView name = findViewById(R.id.tv_name);
        name.setText(drink.getName());
        TextView description = findViewById(R.id.tv_description);
        description.setText(drink.getDescription());
        ImageView photo = findViewById(R.id.photo);
        photo.setImageResource(drink.getImgResourceId());
        photo.setContentDescription(drink.getName());//그림에 대한 설명 일단 이름으로
    }
}
```

## values : xml

### 코드 : strings.xml

```markup
<resources>

    <string name="app_name">Coffee69</string>
    <string-array name="options">
        <item>Drinks</item>
        <item>Food</item>
        <item>Stores</item>
    </string-array>

</resources>
```

## Layout : xml

### 코드 : activity\_top.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".TopActivity" >

    <ImageView
        android:id="@+id/imageView"
        android:layout_width="200dp"
        android:layout_height="100dp"
        android:contentDescription="커피 전문점 입니다."
        android:src="@drawable/starbuzz_logo" />

    <ListView
        android:id="@+id/list_options"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:entries="@array/options"/>
</LinearLayout>
```

### 코드 : activity\_category.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".CategoryActivity">

    <ListView
        android:id="@+id/list_drinks"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

### 코드 : activity\_edit.xml

```markup
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".CategoryActivity">

    <ListView
        android:id="@+id/list_drinks"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

