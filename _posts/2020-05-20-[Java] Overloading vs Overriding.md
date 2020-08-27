---
tags:
- Java
---





### [Java] Overloading vs Overriding



#### 공통점

- method 정의 시 이름을 같게 정의한다.
- 사용이 편리하다.
- 다형성 효과



<br/>

<br/>

<br/>



#### 차이점

##### Overloading(매서드 재정의)

- super class의 메서드를 sub class에서 재정의해서 사용하는 것

- 상속이 전제가 되어야 한다

- 상속받은 메서드와 전체적인 기능은 동일하지만 상세 구현이 다를 경우 기존의 상속 받은 메서드를 사용할 수 없으므로 새로운 메서드를 추가로 선언해야 한다.

  <br/>

- **규칙**

  - 메서드명, 인자, 리턴 타입을 동일하게 선언
  - Access Modifier(접근 제한자)는 부모와 같거나 부모보다 넓은 범위로 정의
  - 부모 메서드와 같은 예외를 던지거나 예외를 안던진다.

  <br/>

- **리턴 타입**

  - 1.7 버전 : 상속 받은 메서드와 리턴 타입이 반드시 같아야 한다.
  - 1.8 버전 : 상속 받은 메서드와 리턴 타입이 같거나 sub를 리턴한다.

<br/><br/>

##### Overriding

- 한 클래스 내에 메서드명이 같은 메서드들을 여러개 정의 하는 것. (즉, 메서드의 인자를 다르게 전달받고 싶을 경우 메서드의 이름을 같게 하고 파라미터 타입을 다르게 해서 사용하는 것)
- 상속이 아니어도 사용이 가능하다. 단, 인자가 달라야 한다.
- 같은 기능을 하는 메서드를 다양한 타입의 인자로 사용하고 싶을 경우
- 자바의 대표적인 overloading 함수는 println. (이클립스에서 ctrl + space bar를 통해 확인)

 \+ 클래스 내의 **메인 메서드**도 오버로드 할 수 있다.

<br/>

- **규칙**

  - 파라미터의 **개수**를 다르게 선언
  - 파라미터의 **타입**을 다르게 선언
  - 리턴 타입은 상관 X

  

```java
private static void sum(int a, int b){
    System.out.println("a+b = ", a+b);
}
private static void sum(int a, int b, int c){ //파라미터의 개수를 다르게 선언.
    System.out.println("a+b+c = ", a+b+c);
}
private static void sum(double a, double b){ //파라미터의 타입을 다르게 선언.
    System.out.println("a+b = ", a+b);
}
```

<br/><br/><br/>