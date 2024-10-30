---
title: Android Fragment
---

<br>

## 0) Activity
<br>

앞서, 우리는 여러 진입점을 통해 애플리케이션에 들어오고, 여러 독립적인 액티비티를 오간다고 했어요.<br><br>

액티비티로 애플리케이션을 다루게되면서, 모든게 다 행복할 것 같았지만....

<br><br><br><br><br><br>

## 1) Fragment의 기원
<br>

문제는 `큰 화면`을 다루기 시작하면서 시작되었어요.<br>

큰 화면에는 `들어갈 수 있는 정보의 양`이 많잖아요? 그럼 액티비티 안에 들어가야하는 요소는 심각하게 많아지게되죠.<br>

화면에 여러 다른 정보들을 담아야하는데, 액티비티 하나는 화면 하나만 나타내기 때문에 한계가 명확했어요.

문제는 또 있습니다. UI는 비슷한데 콘텐츠가 다를 경우, `비슷한 액티비티를 또 생성해야` 한다는거에요.
<br><br>

이처럼 Activity - Activity 구조는 여러 가지 문제점을 안고 있었습니다. 이 때 혜성처럼 등장한 구세주가 바로 `프래그먼트(Fragment)`입니다.
<br><br><br><br>


프래그먼트는 그 단어 뜻 그대로 파편이라는 의미를 가지고 있어요.

<center><img src="https://i.ibb.co/VYTqxxR/2024-10-30-7-17-53.png" alt=""></center>
<br><br>



즉, 액티비티에 들어가는 요소를 잘게 쪼개서 `재사용할건 재사용하고`, UI를 업데이트 할 때도 이 `프래그먼트만 업데이트해서` 성능을 올리자는거죠.



<br><br><br><br><br><br>



## 2) Fragment 개요
<br>

프래그먼트는 액티비티와는 그 관리 방식이 상이합니다.
<br><br>

1. 프래그먼트를 담을 레이아웃을 하나 선언해요.
2. 프래그먼트가 될 각각의 레이아웃들 역시 선언하고
3. 프래그먼트를 상속받은 각각의 커스텀 프래그먼트 클래스를 만들고,
4. FragmentManager가 프래그먼트를 변경하게끔 해요.
<br><br>

아래는 프래그먼트를 담을 레이아웃과 전환을 위한 버튼을 선언한 layout_main.xml파일입니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <FrameLayout
        android:id="@+id/fragment_containter"
        android:layout_width="match_parent"
        android:layout_height="0sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintBottom_toTopOf="@id/fragment_btn1"/>
    <Button
        android:id="@+id/fragment_btn1"
        android:layout_width="0sp"
        android:layout_height="wrap_content"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toLeftOf="@id/fragment_btn2"
        app:layout_constraintBottom_toBottomOf="parent"
        android:text="Fragment1" />
    <Button
        android:id="@+id/fragment_btn2"
        android:layout_width="0sp"
        android:layout_height="wrap_content"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintLeft_toRightOf="@id/fragment_btn1"
        app:layout_constraintBottom_toBottomOf="parent"
        android:text="Fragment2"/>
</androidx.constraintlayout.widget.ConstraintLayout>
```
<br><br><br>

다음으로 프래그먼트가 될 화면 두 개를 각각 xml로 정의하고,
<br>

```xml
<!-- fragment_one.xml -->
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/fragment_layout1"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/purple_200"
        android:text="Blank View"
        android:gravity="center"
        android:textSize="50sp"/>
</FrameLayout>
```

<br>

```xml
<!-- fragment_two.xml -->
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/fragment_layout2"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/purple_500"
        android:text="Blank View"
        android:gravity="center"
        android:textSize="50sp"/>
</FrameLayout>
```

<br><br><br>

커스텀 프래그먼트 클래스를 정의해요.
<br>

```kotlin
class FragmentOne : Fragment() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_one, container, false)
    }
}
```

<br>

```kotlin
class FragmentTwo : Fragment() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        return inflater.inflate(R.layout.fragment_two, container, false)
    }
}
```

<br><br><br>

마지막으로, `FragmentManager`를 통해 메인 액티비티에 프래그먼트를 생성하고, 프래그먼트가 들어갈 레이아웃에 트랜잭션을 추가하면 끝이랍니다!
<br>

```kotlin
class MainActivity : AppCompatActivity() {
    private val binding by lazy { LayoutMainBinding.inflate(layoutInflater) }
    private lateinit var first: Fragment
    private lateinit var second: Fragment

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)

        first = FragmentOne()
        second = FragmentTwo()

        binding.fragmentBtn1.setOnClickListener {
            supportFragmentManager.beginTransaction()
                .replace(R.id.fragment_container, first)
                .commit()
        }

        binding.fragmentBtn2.setOnClickListener {
            supportFragmentManager.beginTransaction()
                .replace(R.id.fragment_container, second)
                .commit()
        }
    }
}
```

<br><br><br>

액티비티를 변경할 필요없이 프래그먼트만 변경할 수 있으니 확실히 좀 더 코드도 보기 편하고 효율적이게 되었죠?


<br><br><br><br><br><br>



## 3) Fragment, 만능인가요?
<br>

이처럼 프래그먼트는 몇 가지 강력한 장점을 가지고 있어요.<br><br>

1. `재사용`이 가능하다.
<br>
이건 아주 당연한데요, 당연히 한 번 만들어놓은 프래그먼트는 `다른 액티비티에서도 재활용`해서 사용할 수 있어요.
<br><br><br>

2. `모듈화`가 가능하다.
<br>
흔히 코드 사이의 `결합도`는 낮추고, `응집도`는 높일수록 유지보수하기 쉬워진다고 하죠. 액티비티의 요소를 프래그먼트 단위로 쪼개면 특정 문제가 발생했을 때 코드를 더 분석하기 쉽고, 문제를 빠르게 추적할 수 있습니다.
<br><br><br>


2. `성능이 향상`된다.
<br>
UI를 업데이트 해야할 때, `필요한 부분만 업데이트`할 수 있어요.<br>
더 중요한건 프래그먼트를 추가하거나 제거할 때 입니다. 부분만 처리하니까, 액티비티 전체를 전환하는 것 보다 `오버헤드가 매우 적어져요.`
<br><br><br>


하지만, 이처럼 만능으로 보이는 프래그먼트에도 단점이 있답니다.
<br><br><br><br>



먼저, `비동기로 동작`합니다.<br>
프래그먼트는 액티비티 내에서 독립적이에요. 그래서 `비동기(Asynchronous)`적으로 움직인답니다.<br><br>

문제는 이렇게 비동기적으로 움직이다보니, 데이터 동시접근같은 트랜잭션 오류가 발생했을 때 발견하기 매우 힘들어질 수 있습니다.<br>

> *`Coroutine Mutex`을 사용하면, 프래그먼트가 전환되어 파괴될 때 실행하고 있던 비동기 작업을 취소하도록 할 수 있어요*

<br><br><br>

또 다른 문제는, `복잡한 생명주기`에 있습니다.
<br><br><br><br><br><br>




## 4) FragmentManager와 Fragment LifeCycle
<br>

프래그먼트는 액티비티와는 또 다른 `생명주기`를 가지고 있습니다. <br>
이 생명주기는 독특한 특징을 가지는데, 바로 액티비티에서 `FragmentManager`에 의해 관리된다는거에요.<br><br>


FragmentManager는 프래그먼트를 더하고, 삭제하고, 교체하고, 백스택에 더하는 등 `동작의 책임`을 지는 클래스입니다.
<br><br>

아래 생명주기는 FragmentManager에 의해서 호출된 이후, 즉 `onAttach()`함수가 호출된 이후의 생명주기를 나타낸 것이에요.<br>


<center><img src="https://developer.android.com/static/images/guide/fragments/fragment-view-lifecycle.png?hl=ko" alt=""></center>
<br><br>


- onAttach() <br>
프래그먼트가 FragmentManager에 의해 추가되고나서, `액티비티에 연결될 때` 자동으로 호출되는 콜백함수에요.
<br><br>

- onCreate() <br>
프래그먼트만 created된 상황이에요. 아직 FragmentView가 생성되지 않았기 때문에, `View와 관련된 초기화를 진행하지 않도록` 주의해야해요. <br>
만약 프래그먼트를 만들면서 넘겨준 값이 있다면, 여기서 변수에 집어넣으면 돼요!
<br><br>

- onCreateView() <br>
액티비티에서는 볼 수 없던 콜백함수가 나왔네요.<br>
프래그먼트가 Layout을 그리기 위해 infalte하는 부분이에요. 되도록 `여기서 View를 inflate` 해줍니다.
<br><br>

- onViewCreated(view: View) <br>
onCreateView()에서는 View를 반환하게 되는데, 그래서 여기서는 View를 초기값들을 설정해주게 돼요.
<br><br>

- onViewStateRestored() <br>
만약 이전에 프래그먼트가 파괴되면서 onSaveInstanceState()에 저장해둔 state값이 있다면, `여기에서 복구`돼요.<br>
이를테면 토글이 on되어있었는지, 체크박스에는 체크가 되어있었는지 등이 있겠네요.
<br><br>

...그리고 나머지 생명주기는 액티비티에서와 같은 활동을 수행하다가,<br><br>

- onStop() <br>
프래그먼트가 더 이상 화면에 보이지 않게 되면, onStop()을 호출해요. <br>
이 때, onSaveInstanceState()보다 onStop()이 먼저 실행되어서, 안전하게 `프래그먼트 트랜잭션(FragmentTransaction)`을 수행하게 해줍니다.<br>
이게 뭔 말이냐면, `완전히 프래그먼트의 기능이 정지하지 않은 상태에서 상태를 저장을 하는 것은 문제`가 있을 수 있겠죠?<br>
그래서 확실하게 프래그먼트 동작이 멈췄다고 판단한 뒤에 상태를 저장한다는 의미입니다.
<br><br>

- onDestoryView()
onDestroy()전에, flate했던 Binding View가 있다면 null을 대입해 인스턴스를 해제해줘야해요. 그렇지 않으면 `메모리 누수`가 발생할 수 있답니다.
