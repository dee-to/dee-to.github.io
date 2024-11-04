---
title: Android Content Provider
---

<br>

## 1) Content Provider
<br>

<center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsATF5%2FbtryyXWnAHp%2FDIxXdo38TXYNSdWN13MVB0%2Fimg.png" alt=""></center>
<br><br><br>

콘텐츠 제공자(Content Provider)는, 내 어플의 데이터를 다른 어플하고 교환하고자 할 때 사용하는 컴포넌트에요.<br><br><br>

콘텐츠 제공자는 마치 `관계형 릴레이션`과 비슷한 결을 가지고 있습니다. 데이터베이스처럼 행과 열을 가지고 있기도 해요.<br><br>

그럼 그냥 DB에 저장하고 갖다쓰지 왜 굳이 콘텐츠 제공자를 사용해야하죠?<br><br><br><br>



이는 크게 두 가지 이유가 있습니다.<br><br>

1. 수 많은 형태의 데이터 <br>
세상에는 `많은 종류의 애플리케이션`이 있다는걸 이해해야 해요. <br>
만약 B앱이 단순히 A앱의 DB에 접근한다고 하면, A앱의 `스키마`가 어떻게 되어있는지 알 수 있는 방법이 없죠.<br>
그래서 B앱은 A앱의 컨텐츠 프로바이더에게서 CRUD를 통해 데이터를 받아오는 것이 합리적이에요.
<br><br>

2. 보안 <br>
사진 앱은 `민감한 사진(파일)`과 사`진 촬영 일자/위치(데이터)`를 모두 가지고 있는 케이스입니다.<br>
다른 여러 앱이 무분별하게 이러한 정보들에 접근해서는 당연히 안되겠죠?


<br><br><br><br><br><br>

## 2) Content Provider, 언제?
<br>

사실, 굉장히 애매한 위치를 가진 컴포넌트가 콘텐츠 제공자라고 할 수 있어요. <br><br>
간단히 Intent를 다른 액티비티에 전달하거나 브로드캐스트를 송신하는 걸 떠나서, 도대체 어떤 경우에 이걸 써먹어야하는지 판단하기가 조금 힘들죠.<br>
안드로이드 공식에서는 다음과 같은 상황에서 콘텐츠 제공자를 사용하기를 권장하고 있어요.

<br>

- 다른 애플리케이션에 복잡한 데이터나 파일을 제공하고자 하는 경우 <br>
-> 연락처 애플리케이션의 경우, 한 사람 한 사람의 연락처를 내보내과 할 때 사용할 수 있어요.<br><br>

- 검색 프레임워크를 사용한 사용자 지정 검색 제안을 제공하고자 하는 경우 <br>
-> 홈 화면에서 '김'을 검색했을 때, 연락처 애플리케이션에서 김으로 시작하는 사람들을 자동완성으로 표시해줄 때 사용할 수 있어요. <br><br>

- 애플리케이션 데이터를 위젯에 노출하고자 하는 경우 <br>
-> 사진 애플리케이션에서, 사진을 위젯에 표시하고자 하는 케이스에 사용할 수 있어요.


<br><br><br><br><br><br>

## 2) Content Provider 구현
<br>


콘텐츠 제공자는 보통 `ContentProvider` 클래스를 상속받아 작성해야하고, `모든 함수를 오버라이드`해야해요.<br><br>

다른 애플리케이션에서 콘텐츠 제공자를 통해 정보를 받을 수 있게 하는 만큼, `표준화`가 중요하겠죠?


```kotlin
class ExampleContentProvider : ContentProvider() {

    // Provider을 초기화하는 메서드
    // ContentResolver 객체가 ContentProvider 객체에 접근하기전까지 만들어지지 않습니다
    override fun onCreate(): Boolean {
        TODO("Implement this to initialize your content provider on startup.")
    }

    // 데이터를 삭제하는 메서드
    // 리턴값으로는 삭제된 행(데이터)의 개수
    override fun delete(uri: Uri, selection: String?, selectionArgs: Array<String>?): Int {
        TODO("Implement this to handle requests to delete one or more rows")
    }
    
    // content URI에 응답하는 MIME type을 반환하는 메서드
    override fun getType(uri: Uri): String? {
        TODO(
            "Implement this to handle requests for the MIME type of the data" +
                    "at the given URI"
        )
    }

    // 새로운 데이터를 넣는 메서드
    // 리턴값은 새롭게 생긴 행(데이터)의 URI
    override fun insert(uri: Uri, values: ContentValues?): Uri? {
        TODO("Implement this to handle requests to insert a new row.")
    }
    
    // 데이터를 획득하는 메서드
    // 리턴되는 데이터는 Cursor 객체
    override fun query(
        uri: Uri, projection: Array<String>?, selection: String?,
        selectionArgs: Array<String>?, sortOrder: String?
    ): Cursor? {
        TODO("Implement this to handle query requests from clients.")
    }

    // 존재하는 데이터를 수정하는 메서드
    // 리턴값은 수정된 행(데이터)의 개수
    override fun update(
        uri: Uri, values: ContentValues?, selection: String?,
        selectionArgs: Array<String>?
    ): Int {
        TODO("Implement this to handle requests to update one or more rows.")
    }
}
```
<br>

콘텐츠 제공자의 생명주기 함수는 `onCreate()`만 존재해요. <br>
나머지는 모두 외부 앱에서 필요할 때만 호출되는 구조랍니다.<br><br>

- query() <br>
프로바이더로부터 데이터를 반환합니다. 반환되는 데이터는 Cursor 객체에요.
<br><br>

```kotlin
override fun query(
    uri: Uri, projection: Array<String>?, selection: String?,
    selectionArgs: Array<String>?, sortOrder: String?
): Cursor? {
    val db = databaseHelper.readableDatabase
    val match = uriMatcher.match(uri)

    return when (match) {
        USERS -> db.query("users", projection, selection, selectionArgs, null, null, sortOrder)
        USER_ID -> {
            val id = ContentUris.parseId(uri)
            db.query("users", projection, "_id=?", arrayOf(id.toString()), null, null, sortOrder)
        }
        else -> throw IllegalArgumentException("Unknown URI: $uri")
    }
}
```
<br><br>

- insert() <br>
프로바이더로 새로운 데이터를 넣습니다. 특정 uri에 새로 ContentsValues를 집어넣어 새로운 행을 만들게 됩니다. <br>
콘텐츠를 제공해주는 쪽에서는 이를 받아 db에 insert할 수 있어요. <br>
마지막으로는 새로 만들어진 행의 uri를 반환해요.
<br><br>

```kotlin
override fun insert(uri: Uri, values: ContentValues?): Uri? {
    val db = databaseHelper.writableDatabase
    val match = uriMatcher.match(uri)

    return when (match) {
        USERS -> {
            val id = db.insert("users", null, values)
            if (id == -1L) {
                throw SQLException("Failed to insert row into $uri")
            }
            ContentUris.withAppendedId(uri, id)
        }
        else -> throw IllegalArgumentException("Unknown URI: $uri")
    }
}
```
<br><br>

- update() <br>
제공자로부터 특정 행의 데이터를 새롭게 업데이트 해요. <br>
반환되는 값은 업데이트된 데이터(행)의 개수입니다.
<br><br>

```kotlin
override fun update(
    uri: Uri, values: ContentValues?, selection: String?,
    selectionArgs: Array<String>?
): Int {
    val db = databaseHelper.writableDatabase
    val match = uriMatcher.match(uri)

    return when (match) {
        USERS -> db.update("users", values, selection, selectionArgs)
        USER_ID -> {
            val id = ContentUris.parseId(uri)
            db.update("users", values, "_id=?", arrayOf(id.toString()))
        }
        else -> throw IllegalArgumentException("Unknown URI: $uri")
    }
}
```
<br><br>

- delete() <br>
제공자로부터 데이터를 삭제해요. <br>
반환되는 값은 삭제된 삭제된 데이터(행)의 개수입니다.
<br><br>

```kotlin
override fun delete(uri: Uri, selection: String?, selectionArgs: Array<String>?): Int {
    val db = databaseHelper.writableDatabase
    val match = uriMatcher.match(uri)

    return when (match) {
        USERS -> db.delete("users", selection, selectionArgs)
        USER_ID -> {
            val id = ContentUris.parseId(uri)
            db.delete("users", "_id=?", arrayOf(id.toString()))
        }
        else -> throw IllegalArgumentException("Unknown URI: $uri")
    }
}
```
<br><br>


- getType() <br>
content의 URI에 응답하는 MIME type을 반환해요. <br>
여기서 MIME type이란 데이터 표현 방식을 나타낸 타입을 의미해요. (text/plain 등) <br>
파일을 제공받는 상황이 아니라면, 단일 데이터/복수 데이터를 구분하는데 쓰이기도 한답니다.
<br><br>

```kotlin
override fun getType(uri: Uri): String? {
    val match = uriMatcher.match(uri)

    return when (match) {
        USERS -> "vnd.android.cursor.dir/vnd.com.example.myapp.users"
        USER_ID -> "vnd.android.cursor.item/vnd.com.example.myapp.user"
        else -> throw IllegalArgumentException("Unknown URI: $uri")
    }
}
```
<br><br>

- onCreate() <br>
프로바이더를 초기화해요. `ContentResolver`가 이 객체에 접근하기 전까지는 Content Proider가 만들어지지 않습니다. <br>
주로 데이터를 제공하는 쪽에서 데이터베이스같은 것들을 초기화 할 때 사용해요.
<br><br>

```kotlin
override fun onCreate(): Boolean {
    // 데이터베이스 초기화
    databaseHelper = DatabaseHelper(context!!)
    return true
}
```
<br><br>

<br><br><br>



다음으로는 Manifest에 등록해야해요. <br>
```xml
<provider
    android:name=".ExampleContentProvider"
    android:authorities="com.example.Provider"
    android:enabled="true"
    android:exported="true">
</provider>
```
<br>

이 때, `android:authorities는 반드시 작성`해야 합니다.<br>
임의로 적어도 상관은 없지만 작성은 되어있어야 해요. <br>
콘텐츠 제공자를 서로 구별 하는데 사용된답니다.
<br><br><br><br><br><br>




## 3) Content Provider 이용
<br>

콘텐츠 제공자를 사용할 때는, `ContentResolver`라는 클래스를 사용해요. <br> 
이 ContentResolver에는 모든 애플리케이션의 콘텐츠 제공자가 등록되어있죠.
<br><br>

그런데 이 때, 컨텐츠를 제공받는 쪽은 `어느 데이터가 어디에 있는 줄 알고` 데이터를 요청할까요? <br><br>

이럴 때 위에서 언급했던 `Uri`가 필요합니다.
<br><br>



<center><img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F994692425C4FAFA40C" alt=""></center>
<br><br>


이 URI는 컨텐츠를 제공받는 쪽에서 각종 CRUD기능에 접근가능한 주소라고 할 수 있습니다. <br><br>


보통은 `authority`는 Manifest에서, 나머지 path나 id는 임의의 object로 선언하게 돼요. 아래처럼요. <br>

```kotlin
companion object{
    const val AUTHORITY = "com.example.contentprovider"
    const val BASE_PATH = "person"
    val CONTENT_URI = Uri.parse("content://" + AUTHORITY + "/" + BASE_PATH)

    const val PERSONS = 1
    const val PERSON_ID = 2

    val uriMatcher = UriMatcher(UriMatcher.NO_MATCH).apply {
        addURI(AUTHORITY, BASE_PATH, PERSONS)
        addURI(AUTHORITY, BASE_PATH + "/#", PERSON_ID)
    }
}
```

<br>


<center><img src="https://i.ibb.co/Qd2DtHz/2024-11-05-6-58-58.png" alt=""></center>
<br>

카카오에서도 위와같이 API문서를 통해 URL 스킴들을 제공을 하고 있고, 위는 그 예시 중의 하나로 볼 수 있습니다.


<br><br>
이제 데이터를 요청하는 등의 crud 기능을, sql요청하듯이 요청할 수 있어요. <br> <br>

```kotlin
 // where절에 들어가게 될 데이터
private lateinit var selectionArgs: Array<String>

// UI에서 값 받아오기
searchString = searchWord.text.toString()

// 만약 searchString 값이 있다면 where절에 값 셋팅 아니면 where절 없이 모든 행 불러오기
selectionArgs = searchString?.takeIf { it.isNotEmpty() }?.let {
    selectionClause = "${UserDictionary.Words.WORD} = ?"
    arrayOf(it)
} ?: run {
    selectionClause = null
    emptyArray<String>()
}

// contentResolver.query()를 사용하여 행을 받아오기
// 반환되는 데이터는 Cursor 객체
mCursor = contentResolver.query(
UserDictionary.Words.CONTENT_URI,  // 받아오려는 데이터의 위치(URI)
projection,                       // 반환되는 행의 column
selectionClause,                  // 조건을 명시하는 곳으로 where절에 해당
selectionArgs,                    // 조건을 명시하는 where절에 ?에 해당하는 값에 해당
sortOrder                         // 정렬조건
)

// cursor 객체의 데이터 갯수를 확인하여 분기
when (mCursor?.count) {
    null -> {
        // null이면 에러헤 해당하는 것으로 에러를 처리할 코드를 작성
        
    }
    0 -> {
        // query가 성공적이지 못한 경우를 처리할 코드
    }
    else -> {
        // 데이터가 있는 경우를 처리할 코드
        mCursor?.apply {
            // word라는 column에 해당하는 인덱스값 얻기
            val index: Int = getColumnIndex(UserDictionary.Words.WORD)

            // moveToNext() 메서드를 사용하여데이터가 있으면 True를 획득 없으면 False를 획득
            while (moveToNext()) {
                // 인덱스를 사용하여 해당 인덱스에 해당하는 데이터 얻기
                newWord = getString(index)

            }
        }
    }
}
```
