---
title: Android Layout
---

<br>

## 1) Layout
<br>


`레이아웃(layout)`이란 ***각 구성요소를 제한된 공간 안에 효과적으로 배치하는 작업***을 의미합니다.<br>

사실 레이아웃은 우리에게 매우 친숙한 단어에요.<br>
곳곳에 레이아웃이 적용되지 않은 분야가 없기 때문이죠. 책, 신문, 포스터 내지는 프로그램이나 애플리케이션에 달하기까지 우리 삶 구석구석에 레이아웃이 적용되어있죠.<br><br><br>


안드로이드에서는 어떨까요?<br>
안드로이드에서 레이아웃이란, `뷰(View)를 효과적으로 배치하는 작업`이라고 의미할 수 있겠습니다.<br>

집이 `애플리케이션`, 방이 `액티비티`, 배치도는 `레이아웃` , 가구는 `뷰`라고 생각하면 쉽죠.<br><br>

`뷰를 어떻게 배치해서 꾸밀 것인가`는 개발자들이 생각할 영역은 아닙니다.<br>
이는 각종 색상 배치부터 사용자 경험 등 여러 요소들이 고려되어야 해요. 꾸미는건 전문가들에게 맡기고, 우리는 기능적인 측면을 좀 더 살펴보자구요.
<br><br><br><br>




## 1-1) View와 Layout
<br>



뷰는 종종 `위젯(Widget)`이라고도 불리고, 레이아웃역시 `뷰 그룹(ViewGroup)`이라고 불리기도 합니다.<br>

<center><img src="https://developer.android.com/static/images/viewgroup_2x.png?hl=ko" alt=""></center>
<br>

그림에서 느낌이 조금 오셨을지도 모르지만, 결국 레이아웃은 *`눈에 보이는 요소(뷰)`를 `눈에 보이지 않는 요소(뷰그룹)`로 묶어 표현하는 과정이다* 라고 생각하면 될 것 같아요.<br>

> 📌 *여담으로, 안드로이드 레이아웃이 위와 같이 트리구조로 되어있다면 `전위순회`를 돌며 뷰를 순차적으로 그리게 됩니다. <br>그래서 뷰의 depth가 깊어지면 렌더링에 시간이 많이 소요될 수 있어요.* 

<br><br>

안드로이드에서 기본적으로 제공하는 위젯은 다음과 같아요.

- TextView <br>
텍스트뷰는 문자또는 문자열을 표시해요. 이 때 텍스트뷰에 담긴 내용은, 유저가 임의로 수정할 수 없습니다.<br>

- EditText <br>
에디트텍스트는 문자또는 문자열을 입력받는 위젯이에요. 사용자에게 입력을 받을 수 있답니다.

- Button<br>
클릭할 수 있는 위젯을 버튼이라고 합니다. 흔히 사용하는 네, 아니오 등의 버튼이 있어요.

- CheckBox<br>
사용자가 `체크됨`과 `체크되지 않음` 중에서 고를 수 있는 체크박스 위젯이에요. 여러 개가 선택될 수 있습니다. 

- Switch / ToggleButton<br>
주로 `온/오프` 상태를 표시하는데 사용하는 버튼이에요.

- RadioButton<br>
체크박스와 기능이 같아요. 다른 점은, 라디오버튼은 반드시 `하나만` 선택됨으로 고를 수 있답니다. 

- ImageView / ImageButton (이미지뷰 / 이미지버튼)<br>
이미지뷰는 단순히 그림을 출력하는 기능을 하고, 이미지버튼은 버튼처럼 클릭하는 용도로 쓰입니다.

<br><br><br>


이번에는 기본적으로 제공되는 레이아웃에 대해 알아봐요.

- LinearLayout<br>
선형 레이아웃이라고도 해요. 레이아웃의 `왼쪽 위부터 아래쪽 또는 오른쪽`으로 차례대로 뷰를 배치합니다.

- RelativeLayout<br>
상대 레이아웃이라고도 합니다. 위젯 스스로가 부모로부터, 또는 다른 위젯으로부터 `상대적인 위치`를 정해 정렬될 수 있어요.

- TableLayout<br>
위젯을 행과 열의 개수를 지정한 표 형태로 배치해요.

- GridLayout<br>
테이블레이아웃보다 조금 덜 엄격한 배치가 필요할 때 배치해요.

- FrameLayout<br>
위젯을 왼쪽 위에 `겹쳐서 배치하여 중복`되어 보이는 효과를 낼 수 있습니다. 여러 개의 위젯을 겹쳐 배치한 후, 상황에 따라서 필요한 위젯을 보이는 방식에 주로 활용된답니다.

- ConstraintLayout<br>
"constraint" 는 `제약` 이라는 뜻으로 여러 제약을 적용하여 위젯의 위치와 크기를 결정해요. 요즘 안드로이드에서 밀고있는 레이아웃이에요.

<br><br><br><br><br><br>




## 1-2) Layout 선언 및 조작
<br>



레이아웃은 크게 두 가지 방법으로 선언될 수 있어요.

- UI에 들어가는 요소들을 xml로 정의하기
- 런타임에 레이아웃 요소들을 인스턴스화하기

<br>

보통은 아무것도 없이 레이아웃 요소들을 갑자기 런타임에 생성하지는 않지만, <br>
안드로이드 프레임워크에서는, `두 가지 방법 모두`를 사용하여 요소를 선언하고 조작할 수 있게 도와준답니다.
<br><br><br>

그럼 잠시, 보편적인 레이아웃을 그리고 조작하는 과정에 대해 잠깐 살펴볼까요?<br><br>

xml로 기본적인 레이아웃을 만들게되면, 마치 html을 그리는 것 처럼 빠르게 레이아웃을 그릴 수 있어요.<br>
xml에는 반드시 `하나의 루트 레이아웃`이 먼저 정의되어야 합니다. 그 이후에는 여러 요소들을 추가해가면서 점진적으로 UI를 확대해 나갈 수 있어요.<br>
아래는 a TextView와 a ButtonView를 세로로 배치하는 예시코드입니다.<br>

```xml
<?xml version="1.0" encoding="utf-8"?>

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <TextView 
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, I am a TextView" />
    <Button 
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello, I am a Button" />
</LinearLayout>
```
<br><br>


이렇게 xml 레이아웃 파일을 정의하고나면, 이는 `View Resource`로 컴파일됩니다. <br>
이렇게 컴파일 되는 과정에서, 정의된 레이아웃 파일은 `R.java` 파일의 `R Class`에 의해서 `임의의 int형식 id로 변환`돼요.
<br>

이제 이 id를 가지고, 우리는 레이아웃을 액티비티에 연결하여 띄우거나, 속성을 변경하는 등의 조작을 할 수 있게 된답니다!<br>

아래는 `main_layout.xml`파일을 액티비티에 연결하여 사용자에게 보일 수 있게 하는 예시코드에요.

```kotlin
fun onCreate(savedInstanceState: Bundle) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.main_layout)
}
```


<br><br><br><br><br><br>


## 1-3) Layout xml 톺아보기
<br>

View나 ViewGroup은 모두 아주 다양한 `xml 속성`을 지원해요.<br><br>

우리가 가장 흔히 볼 수 있는건 `id`입니다. 방금 레이아웃 파일에 관해 설명할 때 잠깐보았죠?<br>
```xml
android:id="@+id/my_button"
```

<br>

레이아웃 내부에서 요소에 이렇게 id를 추가해주면, 사용자는 이 요소들도 활용하고 조작할 수 있게 됩니다.<br><br>
참고로 @는 XML Parser에게 해당 이름을 id로서 사용해야 한다고 알리고, <br>+는 R.java에게 해당 id를 요소로서 추가해야 한다고 알리는 역할을 수행해요.<br><br>

예를 들어, 위에서의 my_button을 불러와 인스턴스를 생성하고 싶다면, 다음과 같이 코드를 작성할 수 있어요.

```kotlin
val myButton: Button = findViewById(R.id.my_button)
```

<br>


특정 요소들은 다른 속성들을 지원하기도 해요.<br>

기본적인 크기나 위치설정부터, 마진, 패딩설정 등을 말이죠.<br>

아래는 TextView와 Button에서 지원하는 여러 가지 xml속성을 설정하는 예시 코드입니다.

```xml
  <?xml version="1.0" encoding="utf-8"?>
  <LinearLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    <TextView 
        android:id="@+id/text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:padding="8dp"
        android:text="Hello, I am a TextView" />
      <Button 
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:paddingBottom="4dp"
        android:paddingEnd="8dp"
        android:paddingStart="8dp"
        android:paddingTop="4dp"
        android:text="Hello, I am a Button" />
  </LinearLayout>
  
```
<br>

추가적으로, 해당 속성들은 `런타임에서 프로그래밍방식으로도` 설정이 가능해요.

<br><br><br><br><br><br>




## 2) RecyclerView
<br>

만약 사용할 뷰가 동적으로 동작해야하거나, 런타임에 확정되어야 하는 경우에는 `RecyclerView`나 `AdapterView 클래스를 직접 상속`받아 구현할 수 있어요.<br><br>

> 📌 *현재는 대부분 RecyclerView를 사용하도록 권장하고있습니다.*

<br>

RecyclerView는 단어 의미 그대로 `재활용 할 수 있는 뷰`를 이야기해요.<br>
더 중요한건, RecyclerView는 단어는 View이지만, `ViewGroup`으로 취급됩니다.<br><br>

어라? 뷰그룹은 앞서 `눈에 보이지 않는 요소`라고 했잖아요.<br>
그럼 RecyclerView의 내부 View들은 어떻게 정의하죠?<br><br><br>

여기서 바로 `ViewHolder`와 `Adapter`가 사용됩니다.
<br><br><br><br><br><br>


## 2-1) Adapter와 ViewHolder
<br>

ViewHolder는 이름에서 알 수 있듯이, `특정한 뷰를 꽉 잡고있는` 의미를 가지고 있습니다.<br><br>

RecyclerView는 가지고 있는 View가 없기 때문에, `프로그래밍적 방법으로 런타임`에서 실시간으로 렌더링돼요.<br><br>

결국 ViewHolder는 `RecyclerView라는 ViewGroup안에 실시간으로 정의되고 사용되는 View`라고 할 수 있겠네요.<br><br><br><br>

이제 ViewGroup도 있고, View도 있으니까, 내용만 채워넣으면 되겠죠?<br>
이 내용은 `Adapter`를 사용해서 정의한답니다.<br>
어댑터라는 말의 뜻은 다음과 같아요.

<center><img src="https://i.ibb.co/YT7KsMy/2024-10-30-8-28-28.png" alt=""></center>
<br>

결국 어댑터는 다른 장치 사이에 놓이는 징검다리가 되는 셈이에요.<br>
바로 `데이터`와 `ViewHolder`사이를 말이에요!<br><br><br>



아래의 코드는 ViewHolder와 Adapter를 정의하는 예시를 보여줍니다.

```kotlin
class CustomAdapter(private val dataSet: Array<String>) :
        RecyclerView.Adapter<CustomAdapter.ViewHolder>() {

    /**
     * 현재 사용 중인 View에 대한 참조를 제공해요.
     * (custom ViewHolder)
     */
    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val textView: TextView

        init {
            // ViewHolder의 View에 ClickListener를 정의합니다.
            textView = view.findViewById(R.id.textView)
        }
    }

    // 새 View를 만듭니다. (LayoutManager에 의해 호출돼요.)
    override fun onCreateViewHolder(viewGroup: ViewGroup, viewType: Int): ViewHolder {
        // 목록의 항목을 정의하는 새 뷰를 만들어요.
        val view = LayoutInflater.from(viewGroup.context)
                .inflate(R.layout.text_row_item, viewGroup, false)

        return ViewHolder(view)
    }

    // 뷰의 내용을 새로 갱신해요. (LayoutManager에 의해 호출돼요.)
    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {

        // dataSet으로 부터 position index마다 요소를 가지고 와 ViewHolder의 텍스트로 정의해요.
        viewHolder.textView.text = dataSet[position]
    }

    // dataSet의 크기를 구합니다. (LayoutManager에 의해 호출돼요.)
    override fun getItemCount() = dataSet.size

}
```

<br><br><br>

그러고나면, Activity에서 비로소 RecyclerView를 사용할 수 있게 된답니다.<br>

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val dataset = arrayOf("January", "February", "March")
        val customAdapter = CustomAdapter(dataset)

        val recyclerView: RecyclerView = findViewById(R.id.recycler_view)
        recyclerView.layoutManager = LinearLayoutManager(this)
        recyclerView.adapter = customAdapter

    }

}
```
