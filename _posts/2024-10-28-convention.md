---
title: 자바/코틀린 코드 컨벤션
---

<br>

## 0) 개발자들의 약속, 코드 컨벤션
<br>

더불어사는 현대 사회에서 언어적인 약속은 매우 중요해요. <br><br>

왜인가요? <br>
언어적인 규약이 잘 짜여져 있어야 사회 구성원들이 무언가를 요구하거나 정보를 교환하기 원할하기 때문이에요. <br><br>

<center><img src="https://i.namu.wiki/i/EcedZevhEQkYjKL3USrgCWlm6IOmSs5BCLmE4eFNp2zLIK-Oogt5gKmtFwWC3UMCug5vwwkijrr2s4zrNv1hBLczV5ZZ0jqNgsKYEyFsmJTFYYEsA3zscJH5LZuG3YZiEtFwJaYDpxwhmWzUrn0ZVw.webp" alt=""></center>

*수박을 멋대로 몽미라고 부르면 안되겠죠?*
<br><br>

이처럼 컴퓨터 언어를 사용할 때에도 사용하는 방식에 대한 약속을 보통 정해놓고 사용하는데, 이를 `코드 컨벤션(Code Convention)`이라고 부릅니다.<br><br>

작게는 전개 된 스코프 탭을 몇 번 할 것이냐부터, 크게는 변수나 함수이름이 정의되어야하는 방식 등 개발자들 사이에서의 언어사용 양식이 되고는 하죠.
<br><br><br><br><br><br>


## 1) 그런데.. 왜...굳이...?
<br>

혼자 코드를 작성할 때는, 스스로 알아만 볼 수 있으면 됩니다.<br>
내가 만들거고, 내가 수정할거니까요.
<br><br>

문제는 `다른 인원과 협업할 때`죠. 코드 컨벤션이 필요한 이유는 크게 두 가지 입니다.<br>
1. 내가 짠 코드를 다른사람이 유지보수하는 경우가 많다.
2. 컨벤션을 사용하는 개발자들 사이의 가독성이 향상된다.

즉, 내가 다른사람 코드를 볼 때, 반대로 다른사람의 코드를 내가 볼 때 코드를 빠르게 식별하고 이해할 수 있다는 뜻이죠.
<br><br><br><br><br><br>



## 2) 구글의 JAVA 컨벤션
<br>

구글에서는 자바 언어를 사용해서 개발할 때 적용되는 컨벤션을 제공하고 있답니다.<br>
https://google.github.io/styleguide/javaguide.html

<br>
구글에서 어떤 컨벤션을 주로 정의하고 있는지 주요한 것만 살펴보도록 해요. <br>
<br><br><br><br><br><br>





## 2-1) 특수기호
<br>

`특수문자`들은 일반적으로 문자열 내에서 바로 사용할 수 없어요.
```java
public static void main(String args[]) {
    System.out.println("Hello {enter} world!");     //????
} 
```

엔터나 백스페이스, 탭 등의 특수문자를 나타내기 위해서는 보통 `이스케이프 시퀀스(Escape Sequence)`라는 역슬래시와 문자의 조합을 주로 사용한답니다.<br>

원래 특수문자 역시 문자이기 때문에, 아스키코드로 나타낼 수 있어요. 이를테면, `줄바꿈`의 아스키코드는 `U+000A`이므로 다음과 같이 나타낼 수 있죠.
```java
public static void main(String args[]) {
    System.out.println("Hello \u000a world!");
} 
```
<br><br>

여기서 발생하는 문제가 있습니다. 너무 알아보기 힘들다는거죠..<br>
그래서 자주 사용하게 되는 특수문자들은, 이스케이프 시퀸스를 활용해서 작성하는 것이 올바른 방법입니다.
```java
public static void main(String args[]) {
    System.out.println("Hello \n world!");      //New Line
} 
```
```java
Hello
world!
```
<br>
보통 사용하는 특수문자는 다음과 같답니다. <br><br>


|특수문자|의미|
|:------|------:|
|\b|백스페이스|
|\t|탭|
|\n|줄바꿈|
|\\"|큰따옴표|
|\\'|작은따옴표|
|\ \ |백슬래시|
<br><br><br><br><br><br>


## 2-2) 코드 포맷팅
<br>


먼저 중괄호의 경우, `if`, `else`, `for`, `do`, `while`처럼 생략가능한 중괄호들이 있더라도 `반드시 표시해야`해요. <br>
```java
//잘못된 예시
if (n > 0)
    return true;
else
    return false;



if (n > 0) {
    return true;
}
else {
    return false;
}
```


<br><br>
제가 가장 힘들어했던(지금도 힘들어하는..) 구조인데, 내부가 비어있지 않다면 `K&R스타일`을 따라요.<br>

- 여는 중괄호 앞 : 줄 바꿈을 하지않음 <br>
- 여는 중괄호 뒤 : 줄 바꿈을 함 <br>
- 닫는 중괄호 앞 : 줄 바꿈을 함 <br>
- 닫는 중괄호 뒤 : 줄 바꿈을 함 <br>
  - 메서드 또는 클래스의 끝, else닫는 경우 중괄호 뒤 줄 바꿈이 없을 수 있음
```java
//보통의 경우 중괄호의 줄 바꿈을 다음과 같이 사용 
if (n > 0) {
    return true;
}
else {
    return false;
}

//단 아래처럼 줄바꿈이 없는 경우도 존재함.
if (n > 0) {
    return true;
} else {
    return false;
}
```
<br><br>




빈 블록의 경우, 마음대로 중괄호를 열고 닫아도 괜찮아요. <br>
단, try-catch문 같은 `멀티블록형`에서 만약 하나의 블록을 열었다면, 나머지 블록들도 모두 열어주어야 해요.
```java
// OK
void doNothing() {}

// OK
void doNothingElse() {
}

// NOT OK (try에 내용 존재)
try {
    doSomething();
} catch (Exception e) {}

// OK
try {
    doSomething();
} catch (Exception e) {

}
```
<br><br>

새 블록을 열 때 마다, `2칸`의 스페이스를 추가해서 영역을 분리해요.
```java
// 각 중괄호 depth마다 2칸의 차이
if {
  if {
    // ...
  }
}
```
<br><br>

한 줄에 하나의 명령문 만을 작성해요.
```java
wake();
go();
work();
```
<br><br>


`import`구문이나 `URL`처럼 불가피한 상황을 제외하고는, `한 줄 길이가 100자를 넘어서는 안돼요.`
```java
// NOT OK
String name = "Kimsuhanmugeobukkiwhadurumisamchungabjadongbangsak..."

// OK
String name = "Kimsuhanmugeobukkiwhadurumi"
 + "samchungabjadongbangsak..."
```
<br><br><br><br><br><br>




`줄바꿈`의 경우, 다음과 같은 상황에서 주로 수행해요.<br>
- 점 구분자('.') 앞에서
```java
someObject
    .doSomething()
    .doSomethingElse();
```
<br>

- 메소드 참조 콜론('::') 앞에서
```java
Function<String, Integer> converter = 
    Integer
        ::parseInt;
```
<br>

- 메소드 참조 콜론('::') 앞에서
```java
Function<String, Integer> converter = 
    Integer
        ::parseInt;
```
<br>

- 제네릭 타입 확장 경계 앰퍼센트('&') 앞에서
```java
class SomeClass<T extends Foo 
    & Bar> {
    // ...
}
```
<br>

- catch블록 파이프('|') 앞에서
```java
try {
    // ...
} catch (FooException 
    | BarException e) {
    // ...
}
```
<br>

- 대입 연산자 앞에서
```java
new StringBuilder()
.append("a")
.append("b");

"a"
+= "b";
```
<br>

- 메소드, 생성자 이름에서 '(' 뒤
```java
Something stk = new Something(
    name = "kim";
);
```
<br>

- 쉼표(',') 뒤
```java
public Something(
    String name,
    int age,
    // ...
)
```
<br>

이 때, 줄바꿈 뒤 들여쓰기는 `4칸`의 스페이스를 추가해요.
<br><br><br><br><br><br>


메소드, 생성자, 멤버 변수 등 클래스 멤버를 구분할 때엔 `세로 공백을 한 줄` 사용하세요. <br>
이 때, 멤버 변수는 사이에 다른 코드가 없다면 공백이 없어도 괜찮아요. 
```java
public class Something {
    String name;
    int age;

    public String getName() {
        return name;
    }

    public void setName(String nameIn) {
        this.name = nameIn;
    }
}
```
<br><br>


다음의 경우에는 `가로 공백`을 사용하세요.<br>
- if, for or catch, 뒤에 오는 '(, )' 사이와 else, catch의 '{, }' 공백
```java
if (a > 3) {
    // ...    
}

if (a > 3) { ... }
```
<br>

- 이항, 삼항 연산자
```java
a + b;
if ((a + b) == 5) ? true : false
```
<br>

- 주석(//) 뒤
```java
// 이것은
// 주석
```
<br>

- 변수 선언 시 타입과 변수명 사이
```java
List<String> list
```
<br>

- 배열 선언문 사이
```java
new int[] {5, 6}  // OK
new int[] { 5, 6 }  // OK
```


가독성을 위해 `가로 정렬`을 하는 경우도 있지만, 구글 스타일에서는 딱히 요구하지는 않아요.
```java
private int x; // OK
private Color color; // OK

private int   x;      // NOT OK
private Color color;  // OK
```
<br><br>


되도록이면 연산자의 괄호는 사용하도록 합니다.<br>
```java
int sum = 3 * 8 * 9;  // NOT OK

int sum = (3 * 8) * 9;  // OK
```
<br><br><br><br><br><br>




`enum`클래스의 경우, `각 상수 뒤 개행`은 선택사항입니다.<br>
```java
private enum Answer {
  YES {
    @Override 
    public String toString() {
      return "yes";
    }
  },

  NO,
  MAYBE
} // OK

private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS } // OK
```
<br><br><br><br><br><br>


`변수를 선언`할 때, `한 줄에 하나씩` 하도록합니다.<br>
(단, `for문 제외`)<br>
```java
int a, b; // NOT OK

int a;
int b; // OK

for(int i=1, j=0; i < 5; i++) {
    // ...
} // OK
```
<br><br><br><br><br><br>



`배열`의 경우 초기화 할 때, `블록과 같은 구조로` 초기화 할 수 있어요.<br>
```java
new int[] {           new int[] {
  0, 1, 2, 3            0,
}                       1,
                        2,
new int[] {             3,
  0, 1,               }
  2, 3
}                     new int[]
                          {0, 1, 2, 3}
```
<br><br><br><br><br><br>




`switch문`의 경우, 중간에 `break`, `return` 등을 사용해 switch문을 종료시킬 수 있죠.<br>
이 때, 의도적으로 종료하지 않아 다음 구문이 실행될 경우, 주석 역시 `실행되는 마지막 구문의 뒤`에 작성해요.<br><br>

default에 실제 동작하는 코드가 오지 않더라도, `포함해서 작성`합니다.
```java
witch (input) {
  case 1:
  case 2:
    prepareOneOrTwo();
    // fall through
  case 3:
    handleOneTwoOrThree();
    break;
  default:
}
```
<br><br><br><br><br><br>



단순 `주석`의 경우, 양식에 큰 제한은 없으나 여러 줄의 `/* ... */`을 사용했다면 반드시 이전 줄과 다음 줄의 `*`는 위치가 일치해야해요.
```java
/*
 * This is          // And so           /* Or you can
 * okay.            // is this.          * even do this. */
```
<br><br><br><br><br><br>







## 2-3) 네이밍
<br>


`클래스 이름`은 `UpperCamelCase`로 작성해요.<br>
띄어쓰기 되는 부분은 모두 대문자로 치환하고, 맨 앞글자 역시 대문자로 작성합니다.
```java
public class HelloThink {
    // ...
}
```
<br>

`메소드 이름`은 `lowerCamelCase`로 작성해요.<br>
띄어쓰기 되는 부분은 모두 대문자로 치환하고, 맨 앞글자는 소문자로 작성합니다.
```java
public class HelloThink {
    // ...
    public void callSomething() {
        // ...
    }
}
```
<br>


`상수 이름`은 `CONSTANT_CASE`로 작성해요.<br>
띄어쓰기 대신 언더바('_')를 사용하고, 모든 문자를 대문자로 치환합니다.
```java
static final int NUMBER = 5;
```
<br>


`상수가 아닌 필드이름이나 지역변수`는 `lowerCamelCase`로 작성해요.<br>
```java
static String nonFinal = "non-final";
```
<br>


`파라미터 이름`은 `lowerCamelCase`로 작성해요.<br>
```java
public void doSomething(int firstIn, int secondIn) {
    // ...
}
```
<br><br><br>



가끔 "IPv6" 또는 "iOS"와 같이 `비정상적인 형식`이 있을 수 있어요.<br>
해당 케이스들은,

1. 문장을 일반 적인 ASCII로 변환하고 어퍼스트로피를 지운다.<br>
"Müller's algorithm" → "Muellers algorithm"<br>
2. 결과를 남은 공백과 구두점을 기준으로 단어로 나눈다.<br>
3. 이미 캐멀 케이스면 단어로 나눈다. (AdWords → ad words)<br>
4. 이제 모두 소문자로 바꾸고 첫 번째 글자만 대문자로 바꾼다.<br>
5. 모든 단어를 합친다.

위의 순서로 모두 변환해요.
<br><br>

예시)
- XML HTTP request <br>
  -   XmlHttpRequest
- new customer ID<br>
  -   newCustomerId
- inner stopwatch<br>
  -   innerStopwatch
- supports IPv6 on iOS?<br>
  -   supportsIpv6OnIos
   
<br><br><br><br><br><br>






## 3) 코틀린 코드 컨벤션
<br>

코틀린 역시 구글의 자바 컨벤션과 일치하는 부분이 아주 많습니다. <br>
안드로이드 공식문서에서는 구글 자바컨벤션과 상당히 유사한 부분만 적혀있지만, 코틀린 공식페이지에서는 방대한 양의 컨벤션이 있습니다. <br>
다른부분만 잠시 살펴볼게요. <br><br>


코틀린에서는 `밀접한 관계를 가졌다면` 여러 선언(클래스, 전역 함수, property 등)을 한 파일에 배치하는 것이 가능해요.<br>
```kotlin
// UserService.kt

class UserService() {

    fun getUser(): User? { ... }

    fun updateUser() { ... }
}

data class User(
    val id: Long,
    val type: Type,
    val name: String? = "default",
    val address: Address,
    val phones: List<String>,
    val active: Boolean
) {

    data class Address(
        val zipCode: String,
        val basicAddress: String,
        val detailAddress: String
    )

    enum class Type {
        ADMIN,
        USER
    }
}
```
<br><br>



클래스의 내용은 다음 순서로 작성되어야해요.
```kotlin
class MyClass {

    // 속성 선언
    private val property1: String
    private val property2: Int

    // 초기화 블록
    init {
        property1 = "Hello"
        property2 = 42
    }

    // 보조 생성자
    constructor(param1: String) { ... }

    // 메소드 선언
    fun method1() { ... }

    // Companion object
    companion object { ... }
}
```
<br><br>


인터페이스를 구현할 때는, 인터페이스의 멤버변수와 `같은 순서`대로 구현하세요.
```kotlin
interface MyInterface {
    fun method1()
    fun method2()
    fun method3()
}

class MyClass : MyInterface {
    override fun method1() { ... }

    override fun method2() { ... }

    override fun method3() { ... }

    private fun helperMethod() { ... }
}
```
<br><br>



오버로딩한 함수끼리는 묶으세요.
```kotlin
class Calculator {
    fun add(num1: Int, num2: Int): Int {
        return num1 + num2
    }

    fun add(num1: Double, num2: Double): Double {
        return num1 + num2
    }

    fun subtract(num1: Int, num2: Int): Int {
        return num1 - num2
    }

    fun subtract(num1: Double, num2: Double): Double {
        return num1 - num2
    }
}
```
<br><br>



보통 코틀린에서는 타입을 지정하거나 클래스를 상속받을 때 콜론(:)을 붙이고는 하죠. 콜론은 다음과 같은 상황에서는 `그 앞에` 공백을 넣어야 해요.

- 타입과 상위 타입을 구분하는 데 사용될 때 <br>
- 슈퍼클래스 생성자로 위임하거나 동일한 클래스의 다른 생성자로 위임할 때 <br>
- "object" 키워드 뒤에 사용될 때 <br>
- 선언과 해당하는 타입을 구분하는 ":" 앞에는 공백을 넣지 마세요. <br>
- ":" 뒤에는 항상 공백을 넣으세요. <br>

```kotlin
abstract class Foo<out T : Any> : IFoo {
    abstract fun foo(a: Int): T
}

class FooImpl : Foo() {
    constructor(x: String) : this(x) { /*...*/ }

    val x = object : IFoo { /*...*/ }
}
```
<br><br>



생성자 매개변수가 적다면, 한 줄로 쓸 수 있어요.<br>
하지만 많다면, 들여쓰기로 구분해야 한답니다.
```kotlin
class Person(id: Int, name: String)

class Person(
    id: Int,
    name: String,
    surname: String
) : Human(id, name) { /*...*/ }
```
<br><br>


메소드의 시그니처가 한 줄에 다 들어가지 않으면, 다음과 같이 정의하세요.
```kotlin
fun longMethodName(
    argument: ArgumentType = defaultValue,
    argument2: AnotherArgumentType,
): ReturnType {
    // body
}
```
<br><br>

메소드의 본문이 단일 표현식이라면, 표현식 본문을 사용하세요.
```kotlin
fun foo(): Int {     // NOT OK
    return 1
}

fun foo() = 1        // OK
```
<br><br>


`if...else` 구문의 경우, `한 줄에 다 들어갈 수 있다면` 중괄호를 생략할 수 있어요,
```java
val value = if (string.isEmpty()) 0 else 1  // OK
```
<br><br>

요소들의 마지막에 `트레일링 컴마(Trailing commas)`를 사용할 수 있어요.

```kotlin
class Person(
    val firstName: String,
    val lastName: String,
    val age: Int, // trailing comma
)
```
<br><br>

변경될 여지가 없다면, 항상 변경할 수 없는 `불변(Immutable) 데이터`를 사용하는 것이 좋아요.(스레드 안정성) <br>
```kotlin
// 나쁜 예: 변경되지 않을 값에 대해 가변 컬렉션 유형을 사용함
fun validateValue(actualValue: String, allowedValues: HashSet<String>) { ... }

// 좋은 예: 변경되지 않을 값에 대해 불변 컬렉션 유형 사용
fun validateValue(actualValue: String, allowedValues: Set<String>) { ... }

// 나쁜 예: arrayListOf()는 ArrayList<T>를 반환하며, 이는 가변 컬렉션 유형입니다
val allowedValues = arrayListOf("a", "b", "c")

// 좋은 예: listOf()는 List<T>를 반환합니다
val allowedValues = listOf("a", "b", "c")
```
<br><br>

오버로드 된 함수를 정의하는 대신, 기본 매개변수 값을 갖는 함수를 선언하세요.
```kotlin
// NOT OK
fun foo() = foo("a")
fun foo(a: String) { /*...*/ }

// OK
fun foo(a: String = "a") { /*...*/ }
```
<br><br>

함수가 매개변수를 여러 개 받을 경우, 문맥 상 이해되지 않을 것 같다면 반드시 `인자 구문을 명명해서` 사용하세요.
```kotlin
drawSquare(x = 10, y = 10, width = 100, height = 100, fill = true)
```
<br><br>


`if...else`와 `when`을 사용할 때, 대부분의 경우에서 `표현식`을 사용하세요.
```kotlin
if(a) {
    b = 1;
} else {
    b = 0;
} // NOT OK

b = if(a) 1 else 0  // OK
```
<br><br>


`열린 범위`를 반복하려면 `until`을 사용하세요.
```kotlin
for (i in 0..n - 1) { /*...*/ }  // NOT OK
for (i in 0 until n) { /*...*/ }  // OK
```
<br><br>





<br><br><br><br><br><br>

## 4) 어떻게 지키지?
<br>

물론 모든 사항을 외워서 다 적용할 수 있으면 좋겠지만, 인간이 된 이상 그게 쉬운 일은 아니죠.<br>

안드로이드의 프로젝트들이 죄다 코틀린으로 바뀌고있는 상황에서, 안드로이드 스튜디오는 `ktlint`라는 컨벤션 체크 플러그인을 제공합니다.

<a href="https://msyu1207.tistory.com/entry/%EA%B9%94%EB%81%94%ED%95%9C-%ED%8F%AC%EB%A7%B7%ED%8C%85%EC%9D%84-%EC%9C%84%ED%95%9C-ktlint-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0-feat-kotlin"> 
 ktlint 커밋 훅 사용하기 </a>
 
 <a href="https://velog.io/@lyh990517/Android-Ktlint-Plugin%EC%9D%84-%ED%86%B5%ED%95%B4%EC%84%9C-%EA%B0%95%EC%A0%9C%EB%A1%9C%EC%BD%94%EB%94%A9%EC%BB%A8%EB%B2%A4%EC%85%98%EC%9D%84-%EC%A0%81%EC%9A%A9%ED%95%B4%EB%B3%B4%EC%9E%90"> 
 안드로이드 스튜디오 편집기에 ktlint 적용하기 </a>
