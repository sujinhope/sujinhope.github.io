# 2020-11-03-[Java] String vs StringBuffer vs StringBuffer





**목차**

1. <a href="#title1">String과 StringBuilder, StringBuffer의 차이</a>
2. <a href="#title2">StringBuilder와 StringBuffer의 차이</a>
3. <a href="#title3">특징 - 정리</a>
4. <a href="#title4">M</a>



<br/><br/>

자바에서 String과 StringBuilder, StringBuffer의 차이는 매우 중요하고 면접에서 자주 물어보는 단골 질문이다. 이번 포스팅에서는 셋의 차이와 각자의 특징에 대해 알아보려고 한다.

<br/><br/>

### String vs StringBuilder, StringBuffer

String과 StringBuilder, StringBuffer의 가장 큰 차이는 값이 변경되는지, 변경되지 않는지의 여부이다. 각 객체를 수정하였을 때 실제 메모리에 어떤 변화가 있는지를 확인하면 쉽게 이해할 수 있다.

<br/><br/>

##### 실제로 String, StringBuffer, StringBuilder가 메모리에 저장되는 방식

![image](https://user-images.githubusercontent.com/33229855/109012441-5d574e80-76f5-11eb-8c39-db432b0fc3ca.png)



<br/><br/>

 우선 자바에서 객체가 저장되는 방식은 위와 같다. `String str = "sujinhope"`에서 변수 `str` 은 **스택**에 할당되고 실제 저장되는 값인 `sujinhope`은 **힙** 영역에 저장된다. 

 이 때 각 변수를 새로운 값으로 변경하게 되면 `StringBuilder`와 `StringBuffer`는 기존 값이 변경된다. 그러나 `String`으로 생성한 객체는 값이 변경되는 것이 아니라 **힙에 새로 메모리가 할당**되면서 스택에 있던 변수는 새로운 위치를 가리키게 되고 기존 값은 어느 누구도 참조하지 않는 쓰레기값(Garbage)이 된다.

<br/>

따라서 `+` 연산자의 사용이나 새로운 값을 대입하는 등 문자열 연산이 많아질 경우, **힙** 영역에 **Garbage**가 계속 생성되어 애플리케이션의 성능에 영향을 미치기 때문에 되도록 StringBuilder나 StringBuffer를 사용하는 것이 좋다.

<br/>

##### 실제로 String, StringBuffer, StringBuilder의 값을 수정했을 때 hashCode() 확인

<img src="https://user-images.githubusercontent.com/33229855/109005233-106f7a00-76ed-11eb-9086-6d24d179a8b6.png" alt="image" style="zoom:67%;" />

<br/>

변수를 수정해보았을 때 String은 값이 변경되는 것이 아니라 아예 메모리에 새로운 변수가 할당됨을 확인할 수 있다.

<img src="https://user-images.githubusercontent.com/33229855/109012679-a0192680-76f5-11eb-8dc0-474213be47ce.png" alt="image" style="zoom:67%;" />

<br/><br/><br/>

### StringBuilder와 StringBuffer의 차이

그렇다면 과연 StringBuilder와 StringBuffer의 차이는 무엇일까?

둘의 차이는 멀티쓰레드 환경에서 **Thread Safe 한가 아닌가**()동기화가 되느냐 되지 않느냐의 차이다. `StringBuilder`의 경우 **Synchronization**이 적용되어 있기 때문에

<br/><br/><br/>

### 특징 - 정리

##### String

- char형의 배열로 생성되며 final이 붙어있기 때문에 **크기가 고정**되어있고 값을 바꿀 수 없음

- 따라서 문자를 변경할 때 뒤에 추가되거나 삭제되는 것이 아니라 **아예 새로운 문자열에 복사**하는 식으로 변경됨.

- 따라서 문자열 연산이 많이 일어날수록 **Heap영역에 Garbage가 생성**되어서 애플리케이션 성능에 치명적인 영향을 끼침

  => String 연산이 많은 경우 StringBuilder나 StringBuffer를 이용하는 것이 좋다.

<br/><br/>

##### StringBuilder

- 변경 가능한 문자열
- 멀티쓰레드에 **Thread Safe**하도록 동기화 되어 있음
- Synchronization이 적용되어 있기 때문에 Multi Thread 환경에서 안전함

<br/><br/>

##### StringBuffer

- 변경 가능한 문자열
- **Buffer**의 크기를 지정하지 않으면 **16개**의 문자를 저장할 수 있는 버퍼 생성
- Synchronization이 적용되어 있지 않기 때문에 StringBuilder에 비해 연산 속도가 빠른 편이다.
- **equals()**메서드를 오버라이딩 하지 않아서 **==**로 비교한 것과 같은 결과이다. 같은지 비교하기 위해서는 .toString()을 이용한 후 equals()를 사용한다.

<br/><br/>

### 속도 비교

- String <<<<<< StringBuilder < StringBuffer

<br/><br/><br/>

<br/>

<br/><br/>

<br/>