---
title: Android Widget
---

<br>

## 1) Widget
<br>

`위젯(Widget)`이란 뭘까요?<br>
본래 위젯이란, 작은 장치를 의미했습니다.<br>

<center><img src="https://i.ibb.co/Qvy6MSn/2024-10-31-9-27-21.png" alt=""></center>
<br><br><br>


하지만 요새는 아래의 뜻으로 더 많이 활용되죠.<br><br>
<center><img src="https://i.ibb.co/YRjw53b/2024-10-31-9-28-41.png" alt=""></center>
<br><br><br>


위젯이 언제부터 위의 뜻으로 활용되기 시작했는지는 명확하지 않아요.<br>
하지만 확실한건 오래 전에도 위젯은 존재했다는거죠.<br><br>
<center><img src="https://folklore.org/images/Macintosh/desk_accessories.gif" alt=""></center>
<br><br><br>


사무실에서의 `작은 기계장치`들, 이를테면 메모장, 계산기 등등을 인터넷 연결과 독립적으로 컴퓨터 장치안에 넣으려는 시도는 아주 오래전부터 있었답니다.
<br><br>


물론, 지금은 그 의미가 확장되어서 모바일 환경에서 런처(홈 화면)에 애플리케이션의 중요한 기능들만 조그마하게 배치할 수 있게 되었죠.<br><br>

<center><img src="https://i.ibb.co/nwKqq64/2024-10-31-9-21-27.png" alt=""></center>
<br><br><br><br><br><br>



## 2) Widget 유형
<br>

위젯을 만들고자 할 때는, `애플리케이션에서 어떤 기능을 제공할 것인가`가 명확해야해요.<br><br><br>


- 정보 위젯
<center><img src="https://developer.android.com/static/images/appwidgets/weather-size-5x2.png?hl=ko" alt=""></center>
<br><br>

말 그대로 정보를 표시하는 위젯입니다.<br>
날씨, 시계, 스포츠정보 등을 표시해줍니다.
<br><br><br>



- 컬렉션 위젯
<center><img src="https://lh3.googleusercontent.com/5LxvvHroArC3oaa0vUGb8Sc9itXp5SGthsgM22QZlrRHfn4mLR3xrVRyjoxqX3C74wnokXWGKd2vZonZ5aoljO0XykiZ5OtcSd4Dm5s=w1064-v0" alt=""></center>
<br><br>

뉴스 기사, 앨범, 이메일 등을 넘겨가며 바로 볼 수 있는 위젯을 의미해요.
<br><br><br>




- 관리 위젯
<center><img src="https://developer.android.com/static/images/appwidgets/light-list.png?hl=ko" alt=""></center>
<br><br>

사용자가 `자주 사용할 법`한 함수를 표시해서, 홈 화면에서 바로 트리거할 수 있는 위젯이에요.
<br><br><br><br><br><br>



## 2) WidgetProvider와 WidgetHost
<br>

여기까지 보면, `위젯은 미니 앱이구나!`라고 생각할 수도 있어요.<br><br>

하지만 위젯은 엄연히 `View`의 일종입니다.<br>
View가 나오면 같이 마중나와야 하는 요소가 있죠. 바로 `ViewGroup`입니다.<br><br>

어라? 뷰를 담는 눈에 보이지 않는 요소가 ViewGroup이라고 했는데, 그럼 `홈 화면은 일종의 ViewGroup의 역할을 수행한다`고 볼 수 있겠네요?<br><br><br>



맞습니다. 하지만 위젯의 측면에서는 해당 용어들을 다소 수정해야할 필요가 있어요.
<br><br><br><br>


먼저 위젯 뷰와 액티비티 사이의 동작을 정의한 `WidgetProvider`가 있습니다.<br><br>

액티비티는 보통 어떤 동작/다른 트리거에 의해 위젯이 업데이트되거나, 비활성화 되거나, 삭제되거나 등의 이벤트가 발생하게 되면 `브로드캐스트`를 수신해요.
<br><br><br><br>




그리고 이러한 위젯 View를 담는 주체, 홈 화면같은 애플리케이션을 `WidgetHost`라고 부릅니다.<br><br>

이번 포스트에서는 주로 WidgetProvider에 관한 설명을 다루게 됩니다.
<br><br><br><br><br><br>



## 3) Widget의 기본 요소
<br>

위젯을 만드려면, 다음 세 가지의 요소가 필요해요.<br><br>

1. 레이아웃 <br>

당연히, `위젯 뷰를 담을 레이아웃`이 필요합니다.<br>
이는 우리가 액티비티, 프래그먼트를 만들 때 처럼 xml로 초기 레이아웃을 정의할 수 있습니다.<br><br>

단, 위젯 레이아웃을 그리기 위해 사용되는 View나 layout에는 `제한`이 있습니다.

<center><img src="https://i.ibb.co/jh8jYC9/2024-10-31-11-09-28.png" alt=""></center>
<br><br>

위젯 레이아웃을 그릴 때는, `RemoteViews`라는 클래스의 하위 요소들만 사용할 수 있습니다. <br>
RemoteView 클래스에서는, 다른 프로세스(WidgetHost) 위에서 볼 수 있는 View 혹은 ViewGroup을 정의해놓은 클래스입니다. <br>

> *RemoteViews의 하위 요소에 관한 내용은 다음 링크를 참고하세요.*
> https://developer.android.com/reference/android/widget/RemoteViews

<br><br><br>



2. AppWidgetProviderInfo <br>
`AppWidgetProviderInfo`는 `위젯 그 자체의 속성`에 대해 정의한 xml파일입니다. <br>


<br><br><br>



3. AppWidgetProvider
위젯과 액티비티가 프로그래밍 방식으로 소통할 수 있는 기본 메소드들이 있는 클래스에요. `Manifest`에서 AppWidgetProvide를 정의하고, 그 뒤에 구현해서 사용하게 됩니다.
<br><br>

<center><img src="https://developer.android.com/static/images/appwidgets/flow-diagram.png" alt=""></center>
<br><br>



<br><br><br><br><br><br>



## 4) Widget 구현
<br>


먼저, 위젯 프로바이더가 위젯과 상호작용할 수 있도록 Manifest에 정의해요.<br><br>

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MyApplication"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:label="@string/app_name"
            android:theme="@style/Theme.MyApplication">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <receiver
            android:name="ExampleAppWidgetProvider"
            android:exported="true">
            <intent-filter>
                <action android:name="android.appwidget.action.APPWIDGET_UPDATE" />
            </intent-filter>
            <meta-data android:name="android.appwidget.provider"
                android:resource="@xml/provider_info" />
        </receiver>

    </application>

</manifest>
```

<br><br><br>


다음으로, 최소 레이아웃 크기, 초기 레이아웃 리소스, 앱 위젯을 업데이트하는 빈도, 작성 시 실행할 구성 활동 등등 위젯 자체에 설정할 config파일 `AppWidgetProviderInfo`를 만들어요.<br>

```xml
<?xml version="1.0" encoding="utf-8"?>
<appwidget-provider xmlns:android="http://schemas.android.com/apk/res/android"
    android:minWidth="40dp"
    android:minHeight="40dp"
    android:targetCellWidth="2"
    android:targetCellHeight="2"
    android:maxResizeWidth="250dp"
    android:maxResizeHeight="120dp"
    android:updatePeriodMillis="86400000"
    android:previewLayout="@layout/widget"
    android:resizeMode="horizontal|vertical">
</appwidget-provider>
```
<br><br><br>


이제 위젯의 layout을 설정하고, <br>
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center_vertical">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:layout_gravity="center">

        <TextView
            android:id="@+id/widget_aircon_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Aircon"
            android:textSize="40sp"/>

        <Button
            android:id="@+id/widget_aircon_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="20dp"
            android:text="ON"/>

    </LinearLayout>


    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:layout_gravity="center">

        <TextView
            android:id="@+id/widget_bulb_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Bulb"
            android:textSize="40sp"/>

        <Button
            android:id="@+id/widget_bulb_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="20dp"
            android:text="ON"/>

    </LinearLayout>


    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:layout_gravity="center">

        <TextView
            android:id="@+id/widget_tv_text"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="TV"
            android:textSize="40sp"/>

        <Button
            android:id="@+id/widget_tv_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginLeft="20dp"
            android:text="ON"/>

    </LinearLayout>

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:orientation="horizontal"
        android:layout_gravity="center"
        android:layout_marginTop="20dp">

        <Button
            android:id="@+id/activity_button"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Activity"/>

    </LinearLayout>

</LinearLayout>
```

<br><br><br>


`AppWidgetProvider` 클래스를 생성해 위젯이 애플리케이션의 브로드캐스트를 받았을 때, 또는 위젯이 생성되었을 때의 동작을 선언해주면 돼요. <br>

```kotlin
package hyundaiht.allwinners.myapplication

import android.app.PendingIntent
import android.appwidget.AppWidgetManager
import android.appwidget.AppWidgetProvider
import android.content.Context
import android.content.Intent
import android.os.Bundle
import android.widget.Button
import android.widget.RemoteViews

class ExampleAppWidgetProvider : AppWidgetProvider() {

    // 앱 위젯은 여러개가 등록 될 수 있는데, 최초의 앱 위젯이 등록 될 때 호출 됩니다. (각 앱 위젯 인스턴스가 등록 될때마다 호출 되는 것이 아님)
    override fun onEnabled(context: Context) {
        super.onEnabled(context)
    }

    // onEnabled() 와는 반대로 마지막의 최종 앱 위젯 인스턴스가 삭제 될 때 호출 됩니다
    override fun onDisabled(context: Context) {
        super.onDisabled(context)
    }

    // android 4.1 에 추가 된 메소드 이며, 앱 위젯이 등록 될 때와 앱 위젯의 크기가 변경 될 때 호출 됩니다.
    // 이때, Bundle 에 위젯 너비/높이의 상한값/하한값 정보를 넘겨주며 이를 통해 컨텐츠를 표시하거나 숨기는 등의 동작을 구현 합니다
    override fun onAppWidgetOptionsChanged(
        context: Context,
        appWidgetManager: AppWidgetManager,
        appWidgetId: Int,
        newOptions: Bundle
    ) {
        super.onAppWidgetOptionsChanged(context, appWidgetManager, appWidgetId, newOptions)
    }

    // 위젯 메타 데이터를 구성 할 때 updatePeriodMillis 라는 업데이트 주기 값을 설정하게 되며, 이 주기에 따라 호출 됩니다.
    // 또한 앱 위젯이 추가 될 떄에도 호출 되므로 Service 와의 상호작용 등의 초기 설정이 필요 할 경우에도 이 메소드를 통해 구현합니다
    override fun onUpdate(
        context: Context,
        appWidgetManager: AppWidgetManager,
        appWidgetIds: IntArray
    ) {
        super.onUpdate(context, appWidgetManager, appWidgetIds)

        appWidgetIds.forEach { appWidgetId ->
            // 메인 액티비티 실행을 위한 Intent를 만들어요
            val pendingIntent: PendingIntent = Intent(context, MainActivity::class.java)
                .let { intent ->
                    PendingIntent.getActivity(context, 0, intent, PendingIntent.FLAG_IMMUTABLE)
                }
            // onClickListener를 만들기 위해 뷰 인스턴스를 생성해요.
            val views: RemoteViews = RemoteViews(
                context.packageName,
                R.layout.widget
            ).apply {
                setOnClickPendingIntent(R.id.activity_button, pendingIntent)
            }

            // AppWidgetManager에게 현재 위젯을 업데이트하라고 알려줍니다.
            appWidgetManager.updateAppWidget(appWidgetId, views)
        }
    }

    // 이 메소드는 앱 데이터가 구글 시스템에 백업 된 이후 복원 될 때 만약 위젯 데이터가 있다면 데이터가 복구 된 이후 호출 됩니다.
    // 일반적으로 사용 될 경우는 흔치 않아요.
    // 위젯 ID 는 UID 별로 관리 되는데 이때 복원 시점에서 ID 가 변경 될 수 있으므로 백업 시점의 oldID 와 복원 후의 newID 를 전달합니다
    override fun onRestored(
        context: Context,
        oldWidgetIds: IntArray,
        newWidgetIds: IntArray
    ) {
        super.onRestored(context, oldWidgetIds, newWidgetIds)
    }

    // 해당 앱 위젯이 삭제 될 때 호출 됩니다
    override fun onDeleted(context: Context, appWidgetIds: IntArray) {
        super.onDeleted(context, appWidgetIds)
    }

    // 앱의 브로드캐스트를 수신하며 해당 메서드를 통해 각 브로드캐스트에 맞게 메서드를 호출한다.
    override fun onReceive(context: Context, intent: Intent) {
        super.onReceive(context, intent)
    }
}
```
