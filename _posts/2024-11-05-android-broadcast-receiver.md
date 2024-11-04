---
title: Android Broadcast Receiver
---

<br>

## 1) Broadcast
<br>

Broadcast는 `방송하다`, `널리 알리다`의 의미를 가지고 있어요.<br>
OSI 7계층에서의 `브로드캐스트`는, 로컬 환경에 연결되어있는 모든 노드에 패킷을 전달하는 통신방식이기도 해요.<br>
아무튼 간에 널리 알린다는 사실 자체는 동일하죠.
<br><br>

그럼 왜 굳이 안드로이드는 `브로드캐스트`를 사용할까요?<br><br>

iOS나 안드로이드같은 모바일 환경은 참으로 바쁩니다.<br>
네트워크 환경도 주시해야하고, 배터리, 각종 센서 등등.. 심지어 다른 애플리케이션에서 알리는 정보들에 대해서도 민감하게 반응해야하죠.<br><br>

굳이 멀리가지 않아도, 안드로이드 OS는 `브로드캐스트를 통해 기기의 각종 상태`에 대해 알립니다.<br><br>

애플리케이션 전역은 해당 정보들을 수신하고, 애플리케이션의 동작이 변경되어야 한다면, 이를 수행하게 됩니다.
<br><br><br><br><br><br>


## 2) vs Activity Intent
<br>

어떤 상태나 의도를 가지고 전달한다는 점에서 `Activity Intent`와 어떻게 보면 유사하다고 할 수도 있어요.<br><br>

하지만 Activity Intent와는 그 목적이 다릅니다.<br><br>

<center><img src="https://i.ibb.co/gtqwpfW/1.png" alt=""></center>
<br><br><br>


Activity Intent는 그 목적이 명확합니다. <br>
`자신이 어디로 가야할지를 아주 명확히` 알고있기 때문에, 만약 존재하지 않는 Activity를 호출한다면 문제가 발생할 수 있습니다.<br><br>

반면에 브로드캐스트의 경우는 조금 달라요. <br>
<center><img src="https://i.ibb.co/RjfYCkg/1212.png" alt=""></center>

<br><br>

인텐트가 발생해서 브로드캐스트를 전송하더라도, 받을 리시버는 받고, 해당 action을 받지 않는 리시버라면 `그냥 흘려보냅니다.` <br>

또, 만약 다른 액티비티들 곳곳에서 실행할 리시버가 여러 개라면 `여러 번 모두 실행`해요.
<br><br><br>


아참, 안드로이드 4대 컴포넌트 중에서 특이하게, 브로드캐스트는 `딱히 목적이 없어요.` <br>
그래서 간단한 목적이나 이벤트 처리하는 곳에 맞춰서 사용하면 된답니다.
<br><br>



<br><br><br><br><br><br>

## 3) Broadcast Receiver 구현
<br>

먼저 Manifest에 `특정한 액티비티 또는 서비스`에서 사용할 `Intent filter`를 구성합니다. <br>

Intent의 경우에는 앞서 말했다시피, OS에서 보내는 브로드캐스트와, 개발자가 만든 브로드캐스트를 모두 사용할 수 있어요. <br>

만약 상속받은 클래스로 리시버를 만들고 싶다면, <br>
```kotlin
class LowBatteryBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        // ...
    }
}
```
<br>
클래스에서 리시버를 정의하고, <br><br>

```xml
<receiver android:name=".LowBatteryBroadcastReceiver" android:enabled="true" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BATTERY_LOW" />
    </intent-filter>
</receiver>
```
<br>

이렇게 xml을 활용해 정적으로 등록할 수 있어요. <br>

해당 방법을 사용하게되면 `애플리케이션이 꺼져있을 때에도` 브로드캐스트를 수신할 수 있답니다. <br><br>

registerReceiver를 런타임에 설정할 수도 있어요.<br>
```kotlin
val filter = IntentFilter(Intent.ACTION_BATTERY_LOW)
registerReceiver(lowBatteryReceiver, filter)
```
<br>

대신 애플리케이션이 실행 중일 때에만 수신할 수 있답니다. <br><br><br><br>
