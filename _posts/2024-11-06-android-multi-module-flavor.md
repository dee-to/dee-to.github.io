---
title: Android Multi Module / Flavor
---

<br>

## 1) Android Module
<br>

<center><img src="https://developer.android.com/static/images/guide/navigation/multimodule-structure.png?hl=ko" alt=""></center>
<br>

<center><img src="https://developer.android.com/static/images/guide/navigation/multimodule-screen.png?hl=ko" alt=""></center>
<br><br><br>


`Multi Module`은 애플리케이션을 `모듈 별로 나누어` 만들자는 방법론입니다. <br><br>



멀티 모듈을 적용하면, 애플리케이션을 data나 domain 등의 모듈로 세분화시켜 개발한 뒤, <br> 
최종적으로 모듈들을 합쳐서 하나의 애플리케이션을 만들게 돼요. 이런 방식을 `모듈화 프로그래밍`이라고도 한답니다. <br><br>

모듈화 프로그래밍은 이해하기 어려울 수 있어요. <br>
하지만 테스트가 빠르고, 수정 사항에 유연하게 대처할 수 있다는 큰 장점이 존재한답니다.
<br><br><br><br><br><br>





## 2) 모듈화의 기준
<br>


안드로이드 공식에서는 일반적으로 모듈을 구성하는 세 가지 기준을 세우고 있어요. <br><br>

- `응집도` <br>
관련된 코드가 `얼마나 밀접하게` 서로 모여있나에 관한 척도에요. <br>
응집도가 높으면 오류가 발생했을 때 한 곳에 모여있을 가능성이 크니 유지보수하기 쉽답니다.
<br><br>


- `결합도` <br>
`모듈들끼리 얼마나 밀접하게 연관되어 있나`에 관한 척도에요. <br>
결합도가 낮으면 서로 연관되어 고쳐야 할 부분이 적어지기 때문에 유지보수하기 쉽답니다.
<br><br>


- `세분성` <br>
세분성은 모듈의 크기, 복잡도에 대한 개념으로 `모듈이 나누어지는 기준, 크기 및 세부화 정도`를 나타내요.
<br>
> 세분성 = 모듈 수 / 코드 베이스

<br><br><br><br><br><br>





## 3) 모듈 나누어보기(계층별)
<br>

<center><img src="https://developer.android.com/static/topic/libraries/architecture/images/mad-arch-overview.png?hl=ko" alt=""></center>
<br><br><br>


- UI Layer <br>
UI계층. ViewModel, View 등이 포함돼요. 일반적으로 `app` 모듈 또는 `presentation` 모듈이 됩니다.
<br><br>

- Domain Layer <br>
복잡한 비즈니스 케이스의 캡슐화를 담당한다. <br>
결국 Java/Kotlin으로 이루어진 기능들을 이야기하고, 하나의 Usecase모델을 잘 묶어 표현한 레이어를 의미해요. 
<br><br>

- Data Layer <br>
`Repository`(Domain과 Data 사이를 잇는 데이터 접근 로직)나 `Resource`(앱 구현에 사용되는 자료들)를 모아놓은 레이어에요.
<br><br>

응집도를 높이기에는 이만한 점이 없지만, 코드가 거대해지면 결국 모듈도 거대해져 그 의미가 퇴색하게 됩니다. <br>

> 해당 방법론은 `클린 아키텍처`라고 부르기도 해요.

<br><br><br><br>



## 4) 모듈 나누어보기(기능별)
<br>

<center><img src="https://i.ibb.co/vJ3KF0W/2.png" alt=""></center>
<br><br>

위 처럼 요리, 요리사, 리뷰로 나누어서 기능을 구성하게 되면, 요리는 요리사와 리뷰를 참고하게 될거에요. <br><br>


<center><img src="https://i.ibb.co/pvJrSHT/21.png" alt=""></center>
<br><br>

그럼 각 모듈이 강하게 연결되므로, 모듈화의 기준을 상당부분 충족시킬 수 없게되죠. 



<br><br><br><br><br><br>






## 5) 모듈 나누어보기(기능/계층별)
<br>

이러한 경우를 막기 위해, 우리는 이를 기능/계층별로 나누게 됩니다.

<center><img src="https://i.ibb.co/BjCPDty/211.png" alt=""></center>
<br><br>

이렇게 두 솔루션을 합쳐서 애플리케이션을 제작하게 되면, <br>

- 각 모듈은 독립적이기 때문에 코드 응집도를 높일 수 있고,
- 각 모듈이 느슨하게 결합되므로 유지보수가 용이하며,
- 모듈이 세분화되어 있으므로 쉽게 재사용할 수 있게 됩니다.
  
<br><br><br>



<center><img src="https://i.ibb.co/VjXf1rm/2fdsa.png" alt=""></center>
<br><br>


그리고 모듈별로 빠르게 테스트 해야 할 부분이 생기면, 데모버전으로 만들어서 테스트 해볼 수도 있죠.



<br><br><br><br><br><br>






## 6) flavor
<br>

<center><img src="https://i.ibb.co/ky1KwjS/2024-11-06-8-20-33.png" alt=""></center>
<br><br>



flavor는 `맛`입니다. <br>
베스킨라빈스 31을 방문하면 정말 다양한 맛의 아이스크림들이 있지만, 본질은 다 같죠. 맛은 달라도 모두 아이스크림이니까요. <br><br>



```xml
android {
    ...
    
    flavorDimensions "api"
    productFlavors {
        prod {
            dimension "api"
            versionCode 1
            versionName "1.0"
            applicationIdSuffix ".prod"
            versionNameSuffix "-prod"
        }
        dev {
            dimension "api"
            versionCode 1
            versionName "1.0"
            applicationIdSuffix ".dev"
            versionNameSuffix "-dev"
        }
    }
}
```
<br><br>


이렇게 애플리케이션을 각각의 flavor로 나누고 나면, 실제 Build Variants에서 각각을 다르게 빌드할 수 있다는 것을 확인할 수 있어요. <br>
<center><img src="https://velog.velcdn.com/images/panicv/post/2439084d-51f0-44d9-a2c0-03014fa43e2d/image.png" alt=""></center>
<br><br>



src/ 폴더로 들어가 dev와 prod를 폴더로 생성하고, 안에 `prod/java/com/Flavor.kt`, `dev/java/com/Flavor.kt`를 생성하면...

<center><img src="https://velog.velcdn.com/images/panicv/post/dbadc80b-f56c-4f72-b95b-c1ce933079e1/image.png" alt=""></center>
<br><br>



<center><img src="https://velog.velcdn.com/images/panicv/post/9027a2c2-ef57-4d3e-b15c-a74ade3df0e0/image.png" alt=""></center>
<br><br>



<center><img src="https://velog.velcdn.com/images/panicv/post/52487641-1b1f-4954-9cb1-71f0ca0ea935/image.png" alt=""></center>
<br><br>

Variant를 변경할 때 마다 다른 폴더를 불러올 수 있게 된답니다.
