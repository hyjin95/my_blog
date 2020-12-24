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

* 
### 코드 : AndroidManifest

