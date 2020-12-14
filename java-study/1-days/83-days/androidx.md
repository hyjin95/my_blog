# AndroidX Migrate하기 : Intent69

### File &gt; Setting 

![](../../../.gitbook/assets/.png%20%2847%29.png)

* Editor &gt; General &gt; Auto Import &gt; Auto import in compietion 체크 해제 &gt; Apply

### gradle.properties

```markup
android.useAndroidX=true
android.enableJetifier=ture
```

### build.gradle

![](../../../.gitbook/assets/search0.png)

* 상단 노란 팝업 Sync Now 클릭

![](../../../.gitbook/assets/search0-1.png)

* 붉은 표시 된 implementation 전구 &gt; Open

![](../../../.gitbook/assets/search3.png)

* Dependencies &gt; app &gt; + &gt; Library Dependency

![](../../../.gitbook/assets/search.png)

* appcompat &gt; Search

![](../../../.gitbook/assets/search2.png)

* Google 1.2.0 선택 &gt; OK

![](../../../.gitbook/assets/search-build-gradle.png)

* 새로 생긴 implmentation확인 &gt; 기존 implementation 주석처리

### MainActivity.java

![](../../../.gitbook/assets/import-.png)

* 기존 import 삭제 후 새로 import하면 androidX로 import되는 것을 볼 수 있다.

![](../../../.gitbook/assets/build-bundle.png)

* APK로 build

