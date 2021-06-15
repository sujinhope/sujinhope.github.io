---
tags:
- Java
- 면접질문

---

<br/>

**목차**

1. <a href="#title1">GC의 변화 - G1 GC(Garbage First GC)</a>
2. <a href="#title2">도커 컨테이너의 향상된 기능</a>
3. <a href="#title3">다중 릴리즈 jar files</a>
4. <a href="#title4">기타 버전별 업데이트(9, 10, 11)</a>

<br/><br/><br/>

이번 포스팅은 Java11의 특징. 즉, Java8 이후 Java11 까지의 대략적인 변화에 대해 정리해두었다.

Java8의 특징을 알고 싶다면 이전 포스트에 정리해두었다.

<br/><br/>

<h3 id="title1">1. GC의 변화 - G1 GC(Garbage First GC)</h3>

대표적인 변화는 기본 GC가 변경되었다는 점이다. 이전 Java8에서는 **Parallel GC**가 기본 콜렉터로 사용되다가 Java9부터는 **G1 GC**가 기본 GC로 채택되었다.

*여기선 대략적인 내용을 살펴보고 가비지 컬렉터와 G1 GC의 자세한 내용은 따로 포스팅 할 예정이다.*

<br/>

G1 GC는 힙 영역을 전통적인 다른 GC들과 달리 물리적으로 구분하지 않고 논리적으로 일정한 크기의 Region으로 나눠 구분한다. GC에서 STW는 피할 수 없기 때문에 G1 GC는 빠른 처리 속도를 달성하면서 STW 시간을 최소한으로 줄이는 것을 목표로 한다.

G1 GC는 사용자가 pause target time을 설정할 수 있는데, 이 시간 동안 collect 할 수 있는 최대한 많은 region들을 추정하고, 완벽하진 않지만 높은 확률로 이 목표 시간을 지키도록 실행된다. 

참고로 이후 Java10에서 G1 GC의 full GC 시 전체 프로세싱을 병렬로 수행하도록 개선하여 성능을 더욱 향상시켰다. 

<center><img width="539" alt="스크린샷 2021-06-15 오후 7 26 13" src="https://user-images.githubusercontent.com/33229855/122037456-8ec46900-ce0f-11eb-9e05-58753e16a1b9.png"></center>

<br/><br/>

<h3 id="title2">2. 도커 컨테이너의 향상된 기능</h3>

Java10 이전에는 컨테이너의 메모리와 CPU 제약 조건이 JVM에 인식되지 않았다. Java8의 경우에 JVM에서는 디폴트 최대힙 사이즈를 기본 호스트의 실제 물리적 메모리의 1/4로 설정하였다.

Java10부터는 JVM에서 제한그룹(cgroups)에 설정된 메모리와 CPU 제한을 사용하여 디폴트 최대힙 사이즈를 컨테이너 메모리 제한의 1/4로 설정한다.

<br/><br/>

<h3 id="title3">3. 다중 릴리즈 jar files</h3>

Java9 부터 여러 버전을 포함하는 jar 파일을 생성할 수 있다.

<a href="https://www.baeldung.com/java-multi-release-jar">https://www.baeldung.com/java-multi-release-jar</a> 사이트에 나와있는 간단한 예제를 번역하여 설명해보겠다.

<br/>

JRE7+ 에서 실행되는 환경에서 jdk7로 작성된 윤년을 확인할 수 있는 클래스 DateHelper를 살펴보자. 

```java
public class DateHelper {
    public static boolean checkIfLeapYear(String dateStr) throws Exception {

        Calendar cal = Calendar.getInstance();
        cal.setTime(new SimpleDateFormat("yyyy-MM-dd").parse(dateStr));
        int year = cal.get(Calendar.YEAR);

        return (new GregorianCalendar()).isLeapYear(year);
    }
}
```

Java8의 LocalDatetime을 사용해 보았다면 date를 아래처럼 간단하게 파싱할 수 있다는 사실을 알 수 있다. 하지만 그것을 이용하기 위해서는 JDK8+로 전환해야 하고, 그 경우 기존 코드는 JRE7에서 동작할 수 없어 전체 코드를 수정해야 하는 상황이 발생한다.

위처럼 간단한 프로그램의 경우 바꿔줄 수 있지만 복잡한 프로젝트의 경우 현실적으로 무작정 변환하는 것이 쉽지 않다.

```java
// jdk8+
public class DateHelper {
    public static boolean checkIfLeapYear(String dateStr) throws Exception {
        return LocalDate.parse(dateStr).isLeapYear();
    }
}
```

<br/>

이러한 문제점을 위해 Java9 부터는 **Multi-Release Jar Files**를 제공한다.

바로 기존 버전으로 작성된 클래스는 그대로 두고, 새로운 JDK 버전으로 작성된 클래스를 함께 패키징 하는 것이다. 런타임 시에는 더 높은 버전이 우선시 되어 실행된다.

예를 들어, 기본으로 Java7을 사용하고, Jvav9, Java10이 함께 패키징 되어 있다면 JVM10 이상은 10을, JVM9일 경우 9를 실행한다. 두 경우에 기본인 Java7로는 실행되지 않는다.

<br/>

##### 폴더 구조

Multi Release Jar Failes를 사용할 경우 버전 이름으로 직접 매핑되므로 동일한 패키지 내에 서로 다른 버전을 같이 둘 수 없다. 따라서 별도의 폴더를 생성해야 하며, 아래와 같이 기존 클래스의 패키지 구조와 동일하게 유지하며 새로 파일을 생성해야 한다.

```폴더구조
src/
	main/
		java/
			com/
				sujinhope/
					multireleaseapp/
						DatePicker.java
		java9/
			com/
				sujinhope/
					multireleaseapp/
						DatePicker.java
```

<br/>

##### <br/><br/>

<h3 id="title4">4. 기타 버전별 업데이트(9, 10, 11)</h3>

기타 자잘한 업데이트 내용은 다음과 같다.

<br/>

##### **1) Java9**

1-1) 다이아몬드 연산자(Diamond Operator)

Java8에서는 다이아몬드 연산자라는 기능을 사용할 수 있지만 익명 클래스에 사용할 경우 컴파일 오류가 발생한다. Java9 이후부터는 익명클래스에도 사용할 수 있도록 개선되었다.

```java
MyHandler<Integer> intHandler = new MyHandler<>(10){  }; // Anonymous Class
MyHandler<?> handler = new MyHandler<>("Onehundred"){  }; // Anonymous Class
```

<br/>

1-2) `@Deprecated` 강화

`@Deprecated`는 나도 그저께 사수님이 알려주셔서 처음 알게되었다. 해당 어노테이션은 '중요도가 떨어져서 앞으로는 사용되지 않을 것', 즉 권장하지 않는 클래스나 메소드에 붙어있다. 그동안은 비록 호환성을 위해 바로 삭제되지는 않았지만, Java9부터는 `@Deprecated`에 **forRemoval()**과 **since()**가 추가되었다.

```java
@Deprecated(since="9", forRemoval=true)
```

since는 해당 메소드가 사용되지 않게된 버전을 나타내고, forRemoval은 다음 버전부터는 삭제될 수도 있다는 것을 경고하는 것이다.

<br/>

1-3) Jigsaw

기존 자바 패키징 매커니즘엔 몇 가지 문제점이 이슈되고, 이 원인은 자바의 캡슐화 지원이 부족하기 때문이라고 생각된다. 무슨 말인가 예를 들어보면, 패키징 된 JAR 파일 내에는 public으로 된 여러 클래스와 메소드들이 존재할 것이다. 이 중 외부에서 호출되면 안되는 내부 API가 있더라도 public으로 선언되어 있다면 누구나 쉽게 호출이 가능하다. 

이 경우 API 개발자가 성능 향상이나 리팩토링을 위해 해당 API를 업데이트 하고 싶더라도 누군가 호출해서 사용하고 있을 수도 있기 때문에 쉽게 변경하지 못할 것이다. 

Jigsaw는 이를 해결하기 위한 모듈화 시스템이다. 모듈을 생성하고 외부에서 호출할 수 있는 API를 직접 명시하면 API 작성자가 노출한 API외에는 외부에서 접근이 불가능하다. 또한 기존에는 JRE 일부만 배포가 불가능했지만 필요한 모듈만 모아 경량화된 런타임 이미지를 만들 수 있다는 장점도 있다.

##### <br/><br/>

##### **2) Java10**

2-1) 로컬 변수 **var** 사용 가능

초기화된 로컬 변수를 선언할 경우, 반복문에서 지역변수 사용 시에만 **var** 키워드를 사용할 수 있다.

```java
var test = 1;
var list = new ArrayList<String>();
```

<br/>

##### <br/>**3) Java11**

3-1)  Lambda 파라미터로 var 사용

- Java8에서 등장했다가 Java10에서 사라지고 Java11에 다시 생겼다.

- 람다는 기본적으로 타입을 스킵할 수 있지만 @Nullable 등의 어노테이션을 사용하기 위해 타입명시가 필요할 때 사용한다. 괄호로 묶어야 하며 **다른 타입과 혼용이 불가능**하다.

  ```java
  (var a, var b) -> a + b; // 가능
  (var a, b) -> a + b; // 불가능 - b도 var을 붙여야 함.
  (var a, String b) -> a + b; // 불가능 - 타입 혼용 불가
  ```

<br/>

3-2) 새로운 String 메소드 추가

- `strip()` : 문자열 앞, 뒤 공백 제거. 기존 `trim()`은 u+200이하의 공백만 인식하고, 그 외 유니코드에 존재하는 다른 공백들은 인식하지 못하는 문제점이 있는데 이를 해결해준다. 또한 `trim()`에 비해 성능도 훨씬 빠르다고 한다.

  ```
  U+0009–U+000D (제어 문자 - 탭, CR, LF 등)
  U+0020 빈칸(space)
  U+0085 NEL (제어 문자 - 다음 줄)
  U+00A0 줄바꿈 안하는 빈칸
  U+1680 OGHAM 빈칸 표시
  U+2000–U+200B (여러 종류의 빈칸들)
  U+2028 LS (줄 구분자)
  U+2029 PS (문단 구분자)
  U+202F NNBSP (줄바꿈 안하는 좁은 빈칸)
  U+3000 상형문자 빈칸
  ```

- `isBlank()` : 문자열이 비어있거나 공백만 포함된 경우 true를 반환한다.

- `lines()` : 문자열을 라인 단위로 쪼개는 스트림을 반환한다.

- `repeat(n)` : 문자열을 n번 반복하여 붙인 문자열을 반환한다.

##### <br/>

3-3) HTTP Client

- Non-Blocking Request와 Response를 지원한다.
- HTTP/2를 지원한다.

<br/>

그 밖에 더 다양한 업데이트 내용은 아래 사이트와 블로그에 정리가 잘 되어 있으니 참고하면 좋을 것 같다.

https://docs.microsoft.com/ko-kr/azure/developer/java/fundamentals/reasons-to-move-to-java-11#ref7

https://daddyprogrammer.org/post/10411/jdk-roadmap-change-jdk9-11/

##### <br/><br/>

<h5 id="title2">참고 사이트</h5>

- <a href="https://luavis.me/server/g1-gc">https://luavis.me/server/g1-gc</a>
- <a href="https://simyeju.tistory.com/67">https://simyeju.tistory.com/67</a>
- <a href="https://parkcheolu.tistory.com/174?category=784687">https://parkcheolu.tistory.com/174?category=784687</a>
- <a href="https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-java9-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EB%B3%B4%EA%B8%B0/">https://www.popit.kr/%EB%82%98%EB%A7%8C-%EB%AA%A8%EB%A5%B4%EA%B3%A0-%EC%9E%88%EB%8D%98-java9-%EB%B9%A0%EB%A5%B4%EA%B2%8C-%EB%B3%B4%EA%B8%B0/</a>
- <a href="https://greatkim91.tistory.com/197">https://greatkim91.tistory.com/197</a>

<br/><br/><br/><br/>