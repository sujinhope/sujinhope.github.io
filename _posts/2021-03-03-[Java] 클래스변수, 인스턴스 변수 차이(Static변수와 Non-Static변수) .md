---
tags:
- CS
- Java
- 면접
---





**목차**

1. <a href="#title1">클래스 변수, 인스턴스 변수</a>
2. <a href="#title2">클래스 변수, 인스턴스 변수, 지역 변수가 메모리에 적재되는 위치</a>
3. <a href="#title3">클래스 변수(Static)의 주의점, 클래스 변수가 필요한 이유</a>



<br/><br/>

<h2 id="title1">
	클래스 변수, 인스턴스 변수
</h2>

<br/>

##### 클래스 변수(<span style="color:#ff6347">Static 멤버</span>)

- 클래스 내에 `Static` 키워드로 선언된 변수
- 처음 JVM이 실행되어 클래스가 메모리에 올라갈 때 ~ 프로그램이 종료될 때까지 유지
- 클래스가 여러 번 생성되어도 `Static` 변수는 처음 **딱 한 번**만 생성됨
- 동일한 클래스의 모든 객체들에 의해서 공유됨

<br/>

##### 인스턴스 변수(<span style="color:#ff6347">Non-static 멤버</span>)

- 클래스 내에 선언된 변수
- 객체 생성 시마다 매번 새로운 변수가 생성됨
- 클래스 변수와 달리 공유되지 않음

<br/>

 아래 예제에서 `StaticTest` 인스턴스를 두 개 생성한 후 `staticTest1`의 클래스 변수와 인스턴스 변수를 수정해보았다. 

 클래스 변수 `classVar`은 처음 한 번만 생성되고 동일한 객체를 `staticTest1` 과 `staticTest2` 에서 서로 공유하기 때문에 양쪽이 동시에 바뀌었지만 `instanceVar`은 객체 생성 시 마다 매번 새로 메모리에 할당되기 때문에 `staticTest1.instanceVar`만 변경된 것을 확인할 수 있다.

<img width="626" alt="스크린샷 2021-03-03 오후 9 46 25" src="https://user-images.githubusercontent.com/33229855/109807972-e9bfbf00-7c69-11eb-870a-20054ed977fc.png">

<br/>

```java
// 출력 결과
1. 10, 28
2. 10, 28
  
1. 12, 400
2. 12, 28
```

<br/>

<br/><br/>

<h2 id="title2">클래스 변수, 인스턴스 변수, 지역 변수가 메모리에 적재되는 위치</h2>

<br/>

##### 지역변수

- 메소드 블럭 안에 선언된 변수로 **메소드 호출 시점 ~ 메소드 종료 시점** 동안 유지된다.

<img src="https://user-images.githubusercontent.com/33229855/109810230-9ef37680-7c6c-11eb-9cf5-1651fd0ff11d.png" alt="image" style="zoom:50%;" />

<br/><br/>

##### 메모리에 적재되는 위치

- 위의 예제처럼 `staticTest1`, `staticTest2` 두 객체를 생성했을 때 **new()**로 생성된 두 객체가 각각 **Heap**에 할당되고 각 객체를 가리키는 `staticTest1`변수와 `staticTest2`변수가 **stack**에 생성된다. 
- <b style="color:#ff6347">인스턴스 변수</b>와 <b style="color:#ff6347">지역변수</b>는 객체가 생성될 때마다 **Stack**영역에 매번 새로 생성되지만 <b style="color:#1e90ff">클래스 변수</b>는 **Static Area**에 한 개만 생성되고 하나의 영역을 공유한다.

<img src="https://user-images.githubusercontent.com/33229855/109814411-d0bb0c00-7c71-11eb-90b8-a9c6158d53e9.png" alt="image" style="zoom:50%;" />

<br/>

<br/>

<br/><br/>

<h2 id="title2">
	클래스 변수(Static)의 주의점, 클래스 변수가 필요한 이유
</h2>

<br/>

##### 주의

- 실제 static 멤버의 생성 시점은 JVM에 따라 다르다.

- 보통 JVM은 필요한 대부분의 클래스를 처음부터 로딩하기 때문에 static멤버의 생성 시점은 JVM이 시작되는 시점이라고 할 수 있다.

- 제약조건

  - **static 메소드**는 오직 **static멤버**만 접근 가능

    => static메소드도 static멤버와 로드되는 시점이 동일하기 때문에 객체가 생성되지 않은 상황에서도 변수를 사용할 수 있어야 한다.

  - **this** 키워드 사용 불가

    => this는 호출 당시 실행 중인 객체를 가리키는 레퍼런스인데 static메소드는 객체가 생성되지 않은 상황에서도 호출이 가능하기 때문이다.

<br/>

##### 클래스 변수가 필요한 이유

- 자바는 캡슐화 원칙에 따라서 C/C++과 달리 어떤 변수나 함수도 클래스 바깥에 존재할 수 없다. 따라서 전역변수나 전역메소드로 사용해야 할 경우 **static**을 이용해서 해결한다.

<br/>

<br/><br/>