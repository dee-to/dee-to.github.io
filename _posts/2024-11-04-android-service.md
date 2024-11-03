---
title: Android Service
---

<br>

## 1) Service
<br>

개발 직군을 나누다보면 흔히 들을 수 있는 말이 있습니다.<br>
`프론트엔드(Front-End)`가 뭐고, `백엔드(Back-End)`가 뭐고 하는 말들 말이에요.<br><br>


물론 그 차이를 설명하려면 내용은 끝도 없이 길어지겠지만, 명확한 차이 하나는 있습니다.<br>
프론트엔드는 `눈에 보인다`는거고, 백엔드는 `눈에 안보인다`는거죠.<br><br>

<center><img src="https://i.imgur.com/pcK7QkY.jpg" alt=""></center>
<br><br><br>


안드로이드에서 제공하고 있는 `서비스(Service)`는 이 `백엔드`와 매우 유사합니다.<br><br>

Activity는 진입점이라고 했죠? Service역시 진입점입니다.<br>
액티비티와의 차이점은 `눈에 보이지 않는다는 것`과, `장기적으로 실행될 프로세스`를 위해 사용된다는거죠. <br><br>

가령 예를 들어보죠. <br>
음악 애플리케이션을 통해 음악을 들을 때, 우리는 그 한 곡만 듣지는 않아요. 들어보고 노래가 좋으면 플레이리스트에 담기도 하고, 친구한테 온 카톡도 답장해야하고, 다른 노래를 듣고싶을 수도 있죠.<br>
이런 와중에도, 음악은 `계속해서 재생`되고 있어야겠죠? 이럴 때 Service를 활용합니다. <br><br>


또, IoT 애플리케이션에서 특정 기기를 실행한다고 해요.<br>
이 때, 애플리케이션은 무수히 많은 작업을 수행할거에요. 정보를 얻어오거나, 기기와 블루투스로 연결작업을 수행한다거나, 패킷을 보낸다거나 하는 작업들 말이에요.<br>
하지만 그 모든 정보들을 사용자에게 보일 필요는 없죠.<br><br>

중요한건, `실행한 기기가 켜졌냐, 꺼졌냐`일테니까요.<br>
그래서 딱히 사용자의 눈 앞에 나타날 필요가 없는 긴 작업들 역시 Service를 활용합니다.
<br><br><br><br><br><br>




## 2) Service의 특징
<br>


단순히 서비스가 눈에 보이지 않는 작업이다. <br>
라고 하면 서비스가 안드로이드 4대 구성요소로 올라갈 이유가 없겠죠?<br><br>

서비스를 만들 때는 아주 중요한 것들이 있습니다. 지금부터 하나 씩 살펴보도록 해요.<br><br><br><br>


1. 스레드 <br>
서비스의 가장 큰 핵심은 `눈에 보이지 않는다`는거에요.<br>
이 말을 다시 정리해보면, `서비스는 눈에 보이지 않음에도 무언가가 돌아가고 있다`는 것을 암시합니다.<br><br>
그럴리는 없지만, 특정 기기와 데이터를 주고받는데 아주 긴(약 60초) 시간이 걸린다고 가정해봐요.<br><br>
만약 이 작업이 그냥 같은 하나의 스레드에서 실행된다면, '기기 실행'을 누른 직후 데이터를 주고받는 이 1분의 시간동안 사용자는 아무것도 수행할 수 없을거에요.<br>
데이터를 주고받는 작업이 스레드 하나를 통으로 점유해버렸으니까요.<br><br>
그래서, `수행시간이 긴` 서비스를 실행할 때에는 사용자가 '응답 없음' 화면을 보지 않도록 `다른 스레드`에서 서비스를 실행할 의무가 있답니다.<br><br><br>
참, 반대로 의도적으로 `사용자와 상호작용하는 동안`에만 서비스를 실행해야 한다고 하면, 스레드로 이를 제어할 수도 있답니다.
<br><br><br><br>


2. 포그라운드(Foreground) <br>
반대로, 사용자에게 `"나 이러한 서비스를 지금 실행 중이야~"`를 명시해야하는 상황도 있습니다.<br><br>
아까 전의 음악 애플리케이션같은 경우가 그렇죠. 음악이 실행되고 있는 동안에는, 우리는 알림 표시줄(iOS라면 잠금화면이나 제어 센터에서)에서 `현재 음악이 재생되고 있다`는 것을 확인할 수 있답니다.<br><br>
이렇게 서비스지만, `특별히 사용자가 실행을 확인할 수 있는` 서비스를 `포그라운드 서비스(Foreground Service)`라고 합니다.
<br><br><br><br>



3. 바운드(Bound) <br>
서비스를 그 때 그 때 새롭게 만들어서 사용하는 행위는 상당히 골치아플 수 있어요.<br>
바운드 서비스(Bound Service)는 모듈화된 서비스를 bindService()를 통해 사용하고, stopSelf()나 stopService()를 호출하여 서비스를 중지할 수 있답니다. 일종의 `서버`같은 역할을 수행해줘요.<br><br>

> *현재 게시글에서는 단순 서비스에 대해 작성합니다.*
<br><br><br>

서비스 역시 자신만의 생명주기를 가지고 있어요.<br>

<center><img src="https://developer.android.com/static/images/service_lifecycle.png" alt=""></center>

<br><br>

- onCreate() <br>
보통 onCreate에서 서비스를 실행하는 스레드를 새로 생성해요. 일반적으로, 서비스는 메인(보통은 UI)스레드에서 동작하게 돼요. 그래서 만약 서비스가 오래걸리는 작업이라면 스레드를 하나 생성해줍니다.
<br><br>

- onStartCommand() <br>
서비스 시작 요청을 받았을 때 실행되는 함수에요. <br>
보통은 Handler에게 Message(일련의 명령 단위)를 전달한답니다.
<br><br>

- onDestroy() <br>
서비스가 종료되었을 때, 사용자에게 알림을 전달하거나 자원을 할당 해제할 때 사용합니다.
<br><br><br><br><br><br>


## 3) Service 구현
<br>

> *일반적으로 Service 클래스를 상속받아 구현되지만, Android Jetpack에서는 WorkManager를 도입할 것을 강력히 권고하고있어요.*

<br><br>

```kotlin
class CountingService : Service() {

    /**
     * Looper.
     * 한 Thread 안에는 반드시 하나의 Looper를 가지게 되어있어요.
     * Looper에서는 MessageQueue라는 큐가 있는데, 여기에는 Thread가 작업해야할
     * 작업들이 Message형태로 기록되어 있답니다.
     **/
    private var serviceLooper: Looper? = null

    /**
     * Handler.
     * 이름에서 알 수 있다시피, 무언가를 다루는 객체라고 할 수 있어요.
     * 보통은
     *      Looper의 MessageQueue에 Message를 넣거나,
     *      MessageQueue에서 받아온 Message를 처리하는 역할을 수행해요.
     **/
    private var serviceHandler: ServiceHandler? = null


    // Queue로부터 데이터를 받아올 Handler입니다.
    private inner class ServiceHandler(looper: Looper) : Handler(looper) {

        override fun handleMessage(msg: Message) {
            try {
                Log.i("BackgroundService", "Service is executing...")
                Thread.sleep(3000)
                msg.arg2 += 100
                val intent = Intent(IntentConstant.COUNTING_RESULT)
                intent.putExtra("result", msg.arg2)
                sendBroadcast(intent)
            } catch (e: InterruptedException) {
                // 스레드가 갑작스레 종료되면(IO입력 등) 인터럽트 시킵니다.
                Thread.currentThread().interrupt()
            }

            // 여러 개의 요청을 받아서 서비스가 동작 중이라면, 갑자기 서비스를 종료할 수가 없게돼요.
            // 그래서 예기치 못한 동작을 피하기 위해, 잠시 모든 요청이 종료되길 기다렸다가 스스로 종료할 수 있게 합니다.
            stopSelfResult(msg.arg1)
        }
    }




    // 서비스를 실행하는 스레드를 새로 생성해요.
    // 일반적으로, 서비스는 메인(보통은 UI)스레드에서 동작하게 돼요.
    // 그래서 이를 방지하기 위해 스레드를 하나 생성해줍니다.
    override fun onCreate() {
        HandlerThread("ServiceStartArguments", Process.THREAD_PRIORITY_BACKGROUND).apply {
            start()

            // HandlerThread의 Looper를 가져와서 Handler에서 사용할 수 있게 해요.
            serviceLooper = looper
            serviceHandler = ServiceHandler(looper)
        }
    }

    override fun onStartCommand(intent: Intent, flags: Int, startId: Int): Int {
        Toast.makeText(this, "서비스를 시작해요", Toast.LENGTH_SHORT).show()

        // 각각의 시작 요청마다 Id를 함께 포함하여 나중에 작업이 끝날 때 뭘 멈춰야할지 알 수 있도록 해요.
        serviceHandler?.obtainMessage()?.also { msg ->
            msg.arg1 = startId
            msg.arg2 = intent.getIntExtra("count", 1)
            serviceHandler?.sendMessage(msg)
        }

        // 만약 서비스가 시작된 채로 중단된다면, 시스템은 서비스를 다시 시작하도록 요청할거에요.
        return START_STICKY
    }

    override fun onBind(intent: Intent): IBinder? {
        // 아무것도 바인드 하지 않으므로, null을 반환해요.
        return null
    }

    override fun onDestroy() {
        Toast.makeText(this, "서비스가 끝났어요.", Toast.LENGTH_SHORT).show()
    }
}
```

<br><br>


서비스 구현에서의 기본 순서는 다음과 같습니다. <br>
1. 빈스레드에서 Looper를 받아와 작업을 등록시킬 준비를 한다.
2. 동시에 새로운 ThreadHandler에게 Looper를 할당해 Handler가 해당 스레드의 MessageQueue를 사용할 수 있게 한다.
3. 서비스 시작을 요청받으면, MessageQueue에 Message를 등록한다.
4. Message는 작업을 완료하고, 브로드캐스트를 보낸다.

<br><br>

```kotlin
val intent = Intent(this, CountingService::class.java)
                    intent.putExtra("count", count)
                    startService(intent)
```
<br>
액티비티로 보내듯이, Intent를 담아 서비스에게도 전달이 가능하답니다.<br><br><br>


```kotlin
val receiver: BroadcastReceiver = object : BroadcastReceiver() {
        override fun onReceive(context: Context, intent: Intent) {
            val result = intent.getIntExtra("result", 1)
            count = result
            counting.text = count.toString()
            btnBackGround.isChecked = false
        }
    }
```
<br>
이후 브로드캐스트 수신을 통해 서비스의 결과를 처리할 수 있어요.
<br><br><br>


참, Manifest에 서비스를 등록하는 것도 잊으면 안됩니다. <br>
```xml
<application
       ...
        <service android:name=".CountingService" />
        ...
            <intent-filter>
                ...
                <action android:name="com.example.servicesample.CountResult"/>
                ...
            </intent-filter>
        </activity>
    </application>
```
