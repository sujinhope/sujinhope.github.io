---
tags:
- Java
- 면접질문
---

<br/>

**목차**

1. <a href="#title1">함수형 인터페이스</a>
2. <a href="#title2">Stream API</a>
3. <a href="#title3">Lambda</a>
8. <a href="#title8">참고 링크</a>

<br/><br/><br/>

그동안은 알고리즘이나 프로젝트를 진행할 때 Java8을 사용해 오다가 이번에 회사에서 Java11을 이용해 프로젝트를 진행하게 되었다. Java8과 Java11은 특히 변화와 특징이 많아 그동안 기술 면접에서도 종종 질문이 들어왔던 이슈인데, 이번 기회에 정리해보려고 한다.

Java11의 특징은 다음 포스트에 정리해 두었다.

<br/><br/>

<h3 id="title1">1. 함수형 인터페이스(Functional Interface)</h3>

Java8의 핵심은 함수형 프로그래밍을 지원한다는 것이다. 함수형 인터페이스를 위해서는 함수형 프로그래밍에 대해 먼저 알아야 하는데, 함수형 프로그래밍에 대해서는 좀 더 자세히 정리하고 싶어서 여기서는 간단히만 정리하고 따로 포스팅을 할 예정이다.

간단히 설명하면 함수형 프로그래밍에서는 함수가 **1급 시민**의 특성을 가진다는 것이 가장 큰 특징이다. **1급 시민**이란, `변수에 저장할 수 있고`, `함수의 리턴값일 수 있고`, `다른 함수에 매개변수로 전달`할 수 있는 특성을 가진 것을 의미한다.

Java8 이전의 자바에서 함수는 변수에 저장하거나 다른 함수의 파라미터나, 리턴 값으로 사용할 수 없었다. 하지만 '함수형 인터페이스'가 도입되면서 함수를 객체로서 다룰 수 있게 되었다.

<br/>

##### 함수형 인터페이스

자바에서 함수형 인터페이스는 추상 메소드가 단 하나 뿐인 인터페이스를 말한다. static 메소드나 자바8부터 인터페이스에서 사용할 수 있게된 default method는 카운트에 포함되지 않는다.

`@FunctionalInterface` 어노테이션을 명시하지 않아도 추상 메소드가 하나 뿐일 경우 자동으로 함수형 인터페이스로 인식된다. 하지만 어노테이션을 사용할 경우 함수형 인터페이스 조건을 지키지 않았을 경우 컴파일 에러를 발생시키기도 하고, 다른 사람들이 봤을 때 한 번에 알 수 있기 때문에 명시해주는 것이 좋다.

<br/>

아래에 함수형 인터페이스를 정의하고 호출하는 간단한 예제를 작성해보았다. 

<br/>

세 개의 파라미터를 매개변수로 받는 추상메소드 **doSomething()**을 가지는 `Functional` 인터페이스를 정의하였다.

```java
// Functional.java
@FunctionalInterface
public interface Functional {
    public int doSomething(int a, String b, LocalDate c);
}
```

<br/>

##### 함수형 인터페이스 사용법

메인함수에서 Functional 인터페이스의 doSomething 함수를 호출해서 사용해보았다.

1. Override

   Comparator를 사용할 때처럼 doSomething 함수를 Override하여 학생의 번호, 이름, 생일을 파라미터로 받아 정보를 출력하고 나이를 반환하는 함수로 구현하였다.

2. Lambda 표현식 이용

   람다 표현식을 이용하면 Overriding 할 필요 없이 간결하게 사용할 수 있다. 이를 이용하여 doSomething함수가 졸업식 정보를 출력하도록 재정의하였다. 그냥 람다 함수로 사용할 때와 달리 변수에 저장할 수 있으며, **재사용이 가능**하다.

```java
// FunctionalTest.java
public class FunctionalTest {
    public static void main(String[] args) {
        
        // 1. Override
        Functional functional = new Functional() {
            @Override
            public int doSomething(int num, String studentName, LocalDate birth) {
                System.out.println(num + "번째 학생 " + studentName + "의 생일 : " + birth);
                LocalDate today = LocalDate.now();
                int age = today.getYear() - birth.getYear() + 1;
                return age;
            }
        }
        System.out.println("나이: " + functional.doSomething(1, "sujinhope", LocalDate.parse("1995-01-01")));
        
        // 2. 람다표현식 이용하기
        Functional newFunctional = (a, b, c) -> {
            System.out.println(a + "번째 학생 " + b + "의 졸업식 : " + c);
            return a;
        }
        newFunctional.doSomething(2, "forB", LocalDate.parse("2020-01-01"));
      
    }

}
```

<br/>

결과

```
1번째 학생 sujinhope의 생일 : 1995-01-01
나이: 27
2번째 학생 forB의 졸업식 : 2020-01-01
```

<br/>

<br/>

##### 자바에서 제공하는 함수형 인터페이스

자바에는 기본적으로 제공하는 다양한 함수형 인터페이스들이 있다. 제공되는 함수형 인터페이스들은 `java.util.function` 패키지에 정의되어 있다.

- Function\<T, R> : T 타입의 인자를 받고, R 타입의 객체를 리턴
- Predicate\<T> : T 타입의 인자를 받고 boolean을 리턴
- Consumer\<T> : T 타입의 객체를 인자로 받고 리턴 값이 없음
- Supplier\<T> : 인자를 받지 않고 T 타입의 객체를 리턴
- Runnable : 인자를 받지 않고 리턴값도 없음
- ...

<br/>

<br/><br/>

<br/>

<h3 id="title2">2. Stream API</h3>

Java8에서 새로 추가되었으며 `for문`과 같은 반복문이나 반복자를 사용하지 않고 **함수형 인터페이스(람다식)를 적용**하여 다양한 타입의 데이터를 반복적으로 처리할 수 있는 기능이다. Stream을 직접 사용하면서 느꼈던 장점은 기존 방식에 비해 간결하고 가독성이 높다는 것이다.

사용법은 아마 자바스크립트를 써봤던 사람이라면 익숙한 방식일 것이라고 생각한다.

<br/>

Stream API는 크게 생성, 중개연산, 최종연산 세 단계로 구분된다. 중개연산의 경우 **메소드 체이닝**이 가능하다.

1. 생성

   ```java
   // 리스트
   List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7);
   Stream<Integer> stream = list.stream();
   
   // 배열
   String[] arr = new String[]{"Jan", "Feb", "Mar", "Apr", "May"};
   Stream<String> streamArr = Arrays.stream(arr);
   
   // 가변 매개변수
   Stream<Double> streamDouble = Stream.of(10.28, 12.25, 1.1);
   ```

2. 중개연산

   filter(), map(), flatMap(), forEach(), ...

   ```java
   stream
       .filter((e) -> {return e%2 == 0;})		// 조건에 만족하는 요소들 필터링
       .forEach(e -> System.out.println(e)); // 괄호, 중괄호(한 문장일 경우만) 생략 가능
   ```

3. 최종연산

   count(), max(), 

   ```java
   stream.count(); // stream 최종 결과값의 개수를 반환
   ```

<br/>

<br/>

##### **특징**

- Collection 인터페이스에 `stream()` 메소드가 정의되어 있기 때문에 Collection 인터페이스를 구현한 모든 List와 Set과 같은 컬렉션 클래스에서 `stream()` 메소드로 스트림 생성이 가능하다.

- 공통된 접근 방식을 제공한다. stream으로 변경하고 나면 List, Set, Map에서 모두 공통된 방식으로 데이터에 접근할 수 있다.

- 내부 반복을 통해서 작업을 수행한다.

- **재사용이 불가능**하다.

- **원본 데이터를 변경하지 않는다.**

- `filter-map` 기반의 API를 사용하여 **lazy 연산**을 통해서 성능을 최적화한다.

  **\* lazy 연산**

  불필요한 연산을 피하기 위해 연산을 지연시키는 것
  
  ```
  https://dororongju.tistory.com/137 // 정리 Very Good!
  ```



##### Iterator와의 차이점

- 람다식을 제공하므로 코드가 간결해짐
- 내부 반복자를 사용하여 병렬처리가 쉬움

<br/><br/><br/><br/>

<h3 id="title3">3. Lambda</h3>

람다 표현식은 함수명이 없는 익명함수를 말한다. 람다 표현식은 함수형 인터페이스를 간편히 구현하는 역할을 하며, **1급 객체**로 분류되어 함수의 매개변수나 리턴 값으로 반환할 수도 있다.

기본적인 람다식 구조는 아래와 같다.

표현식 바디에 식이 한 개 뿐일 경우 중괄호 생략이 가능하다.

```java
// ( 매개변수 ) -> { 표현식 };
(int a, int b) -> {return a + b};
(String str) -> System.out.println(str); // {} 생략
```

<br/>

##### 메소드 참조 표현식 - `::`

람다식에서 파라미터를 중복해서 사용하기 싫을 경우 사용한다. 람다 표현식에서만 사용 가능하며 이름만으로 특정 메소드를 호출할 수 있는 기능이다.

사용 방법은 `인스턴스::메소드명(또는 new)` 양식을 사용하면 된다.

```java
// 기존
stream.forEach(e -> System.out.println(e));
// ::
stream.forEach(System.out::println);
```

<br/>

<br/>

##### 장점

- 코드가 간결해진다.
- Lazy Loading을 이용하여 성능이 향상된다.
- 함수형 프로그래밍을 바탕으로 병렬처리가 가능하다.

##### 단점

- 람다식을 남용할 경우 코드의 가독성을 떨어뜨릴 수 있다.

<br/><br/><br/>

##### <br/>

<h5 id="title7">참고 사이트</h5>

- <a href="https://empty-cloud.tistory.com/76">https://empty-cloud.tistory.com/76</a>
- Stream API
  - <a href="https://dororongju.tistory.com/137">https://dororongju.tistory.com/137</a>
  - <a href="https://velog.io/@foeverna/Java-513-%EC%8A%A4%ED%8A%B8%EB%A6%BC-API-Stream-API">https://velog.io/@foeverna/Java-513-%EC%8A%A4%ED%8A%B8%EB%A6%BC-API-Stream-API</a>
  - <a href="http://tcpschool.com/java/java_stream_creation">http://tcpschool.com/java/java_stream_creation</a>
  - 
- 함수형 프로그래밍
  - <a href="https://gsmesie692.tistory.com/267">https://gsmesie692.tistory.com/267</a>

<br/><br/><br/><br/><br/>