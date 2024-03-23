---
title: RAII (Resource Acquisition Is Initialization)
categories:
- C++
- 따배씨++
tags:
- C++
- 따배씨++
- RAII
---

<br>

> C++ 초보가 따배씨++ 강의를 듣고 생긴 의문을 정리하고 고찰하는 게시글입니다.<br>
> 언제든지 잘못된 내용은 지적해주세요!!

<br>

> 📌 연관된 강좌는 **`15.1 - 이동의 의미와 스마트 포인터`** 입니다.<br>
> 또한 아래의 링크들을 참고하였습니다.<br>
> (슭님) https://blog.seulgi.kim/2014/01/raii.html <br>
> (류광님) https://occamsrazr.net/tt/297 <br>
> https://www.artima.com/articles/modern-c-style#part3 <br>
> https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization

<br><br>




C++를 관통하는 관용구 하나가 있습니다. `RAII` 원칙이 그 주인공인데요. <br> 
한글로 번역하는 과정에서 오해가 발생할 여지가 있다는걸 참작해도 정확히 뭔가 팍 와닿지않는 문구이기도 해요. 늘 논란의 도마 위에 얹혀져있는 RAII 원칙에 대해 알아보도록해요.

<br><br><br>

## 1) RAII (Resource Acquisition Is Initialization)
<br>

RAII 원칙. 직역하면 `자원 획득은 초기화이다.` (또는 자원 획득이 초기화이다로 보이기도 하죠?) 라는 의미에요.
<br><br>
언듯보면 자원획득? 특정한 자료구조를 동적할당할 때 사용하는 원칙인가? 싶지만, RAII는 ***소멸***에 좀 더 치중되어 있는 원칙입니다.
<br><br><br>

이게 당최 뭔 소리인가하면,

```cpp
#include <iostream>

void unSafe()
{
    // doing something
    throw -1;
}


int main()
{
    int* myarray = new int[10000];

    try
    {
        unSafe();
    }
    catch(...)
    {
        std::cerr << "Exception occured." << std::endl;
        return 0;
    }

    delete[] myarray;
}
```

위의 코드에서는 함수 unSafe()가 특정값을 throw하고 있어요.<br>
이 때 그 값을 catch한다면 어떻게 되나요? 에러 메시지를 출력한 뒤 early return하기 때문에, myarray는 할당 해제되지 못하고 그대로 memory leak을 발생시킬거에요.
<br>

"뭐야, 그럼 안까먹고 delete해주면 되는거 아니야?" 라고 생각하시겠지만, 결국 우리는 사람인지라 이리저리 다른 것들에 치이다보면 사소하지만 중요한 것들을 까먹기 쉽상이죠. 
<br>

여기서 RAII의 철학이 빛을 발합니다.
<br><br>

***사람은 까먹으니까, 컴파일러에게 할당 해제를 맡기자는거죠.***

<br><br><br><br><br><br><br><br>







## 2) 뭘 어떻게 맡겨?
<br>

배웠던 내용을 다시 돌아볼게요.
<br>
우리에겐 `Heap(힙)`과 `Stack(스택)`메모리가 있어요.
<br>

```cpp
int     array1[1000];                // to Stack
int*    array2 = new int[10000];     // to Heap
```

객체 array1은 스택 메모리에 위치하게 될거고, array2는 힙메모리에 위치하게 될거에요.
<br>

이 때, 둘은 `프로그램이 끝날 때` 어떻게 해야 하나요?
<br>

```cpp
// 프로그램 시작...

int array1[1000];

// ...프로그램 끝.
```

```cpp
// 프로그램 시작...

int* array2 = new int[10000]; 
// ...
delete[] array2;

// ...프로그램 끝.
```

당연한 소리지만, array2는 할당을 해제해줘야해요. 동적 할당된 인스턴스들이 위치한 힙메모리는, 전적으로 프로그래머에게 그 관리 책임을 위임하기 때문이에요.
<br>

![img](https://i.ibb.co/fQYWdFD/heapstack.webp){: width="60%" height="60%"}
	
<br><br>

그러자, 우리 C++의 아버지 Bjarne Stroustru가 생각합니다.<br>

<span style="font-size:1.25em;"> *"그럼, 지우는걸 스택에게 맡기면 되는거 아닐까?"* </span>

<br><br><br><br><br><br><br><br>







## 3) 스마트 포인터
<br>

여기서 나오게 된 개념이 바로 `스마트 포인터`입니다. <br>
객체가 존재하는 스코프를 벗어나면, 알아서 메모리를 해제해주는 포인터에요.
<br><br>

```cpp
#include <iostream>

class Autoptr
{
public:
    int* _ptr;

public:
    Autoptr(int* ptr_in = nullptr)
        : _ptr(ptr_in)
    {
        std::cout << "Smart pointer constructed by Autoptr" << std::endl;
    }

    ~Autoptr()
    {
        delete[] _ptr;
        std::cout << "Smart pointer destructed by Autoptr" << std::endl;
    }
};
```
```cpp
#include "Autoptr.h"

int main()
{
    Autoptr my_ptr1(new int[10000]);
}
```
<br>

위와 같은 Autopt클래스가 있다고 해요.<br>
`my_ptr1`의 인수는 int형 변수 10000개를 저장할 수 있는 공간을 운영체제에 요청하겠죠?<br>
하지만 '클래스' Autoptr의 객체는 스택메모리에서 관리하게 될거에요. main()이 끝날 때, 컴파일러는 소멸자를 자동으로 호출해서 메모리 할당을 해제하겠죠.<br><br>

그럼 이렇게하면 어떨까요?<br>
*동적 할당된 힙메모리 공간을 가리키는 포인터를 스택메모리에서 관리한다면?*
<br><br>

1) 운영체제에 동적 할당 공간을 요청하는 권한을 Autoptr생성자에게 넘깁니다. 이 때, 요청했던 공간을 가리키는 포인터가 멤버 변수 _ptr에 저장돼요.<br>
   
2) 이제 해당 공간의 포인터는 Autoptr이 관리하게 돼요. 물론, 화살표연산자를 활용하면 자료에 대해 접근도 가능하겠죠?<br>
   
3) 스코프가 소멸되면, 컴파일러는 늘 하던대로 Autoptr의 소멸자를 자동으로 호출해서 메모리 할당을 해지할거에요.<br>
   
3.1. 이 때, Autoptr의 소멸자에, _ptr이 가리키던 공간에 대한 할당을 해제하는 delete 키워드를 적어줍니다.

<br>

이제 자동으로 포인터를 지워주는 Autoptr이 완성되었어요. 와!<br>
(Autoptr을 템플릿으로 작성하면 다른 자료형에도 적용할 수 있고, 얕은 복사 문제를 해결하기 위해 복사 생성자나 대입 연산자를 오버로딩할 수도 있어요)
<br><br><br><br><br><br><br><br>







## 4) 그래서... 결국 RAII의 진정한 의미는요?
<br>

실제로 사람들 사이에서도 의견이 분분해요.<br>
원칙 이름이 너무 두루뭉실하게 잘못지어졌다는 의견이 대다수죠.<br>
*Scope-Bound Resource Management(스코프 범위 자원 관리, SBRM)* 으로 바꿔야 한다는 의견도 있었어요.<br><br>

더군다나 RAII 용어를 직접 만드셨던 Bjarne 옹 스스로도 별로 좋지않은 이름이라고 인정하기도 하셨구요.<br><br>

하지만 결국 RAII는 위와 같이 동적 할당의 소멸을 컴파일러가 관리해주도록 하는 디자인적 패턴을 나타나는 관용구로 굳어졌죠.<br><br>

위키피디아에서는 RAII를 다음과 같이 소개합니다.<br><br><br><br><br>

> *리소스 할당 (또는 획득)은 객체 생성(구체적으로 초기화) 중에 생성자 에 의해 수행되는 반면, 리소스 할당 해제(해제)는 객체 파괴(구체적으로 종료) 중에 소멸자에 의해 수행 됩니다. 즉, 초기화가 성공하려면 자원 획득이 성공해야 합니다.*

<br><br><br><br>

<span style="font-size:1.7em;"> *"자원 획득은 초기화다"* </span>



글쎄요.. Bjarne 옹이 생각한 자원 획득은 단순히 그 자원을 생성하는 것이 아닌, 이를 생성하고 소멸하는 것을 `책임`져주는 `클래스`를 만드는 것을 중요하게 생각하셨던게 아닐까요?<br><br>
