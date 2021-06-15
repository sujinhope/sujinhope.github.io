---
tags:
- Network
- 면접질문

---

<br/>

**목차**

1. <a href="#title1">동기(Sync)와 비동기(Async)</a>
2. <a href="#title2">블로킹(Blocking)과 논블로킹(Non-Blocking)</a>
3. <a href="#title3">예제 - 동기+블로킹, 동기+논블로킹, 비동기+블로킹, 비동기+논블로킹</a>

<br/>

<br/>

이번 시간엔 동기와 비동기, 블로킹과 논블로킹에 대해 알아보려고 한다.

이전에 동기와 비동기에 대해서 공부할 때 단순히 하나의 작업이 처리된 후에 다음 일이 처리되면 동기, 여러 작업이 동시에 처리되면 비동기라고 이해하고 있었는데, 동기와 비동기를 말 할 때 블로킹/논블로킹의 개념을 함께 이해해야 된다는 것을 알게 되었다.

그동안 내가 단순히 **동기**와 **비동기** 개념으로 이해하고 있었던 것은 엄밀히 따지자면 **동기+블로킹**, **비동기+논블로킹**이었던 것 같다. 동기/비동기와 블로킹/논블로킹은 엄연히 다른 개념으로 한 번 짚고 넘어갈 필요가 있다.

<br/>

들어가기에 앞서 쉽게 이해할 수 있도록 간단히 정리해보자면 **동기**와 **비동기**는 작업을 **어떠한 흐름으로 처리**할 것이냐, **블로킹**과 **논블로킹**은 처리하고 있는 작업이 **전체의 흐름을 막느냐/안막느냐**의 차이이다.

<br/>

<br/>

<h3 id="title1">1. 동기(Sync)와 비동기(Async)</h3>

<br/>

<center><img width="618" alt="스크린샷 2021-06-13 오후 6 36 56" src="https://user-images.githubusercontent.com/33229855/121802264-68b79100-cc76-11eb-81b9-5959baf90956.png" style="zoom:85%;" ></center>

<br/>

##### **1) 동기(Sync)**

동기는 처리되는 작업의 요청과 결과가 한 자리에서 일어난다. 즉, 현재 처리되고 있는 작업의 요청이 모두 완료하여 응답을 리턴한 후에 다음 작업을 처리할 수 있다.

이 때, 동기는 <b style="color:#EC6B66">호출하는 함수</b>가 <b style="color:#A27DF7">호출된 함수</b>의 작업이 완료되었는지를 계속 신경쓰고 있다.

<br/>

##### **2) 비동기(Async)**

반대로, 비동기는 <b style="color:#EC6B66">호출하는 함수</b>가 <b style="color:#A27DF7">호출된 함수</b>의 작업 완료 여부를 신경쓰지 않는다. 호출하는 함수는 함수를 호출할 때 **callback 함수**를 같이 전달하고, 작업이 완료되면 **callback 함수**가 실행된다. 즉, <b style="color:#A27DF7">호출된 함수</b>가 수행결과와 종료를 직접 신경쓰고 처리한다.

따라서 **Task1**이 완료여부에 상관 없이 **Task2**를 처리할 수 있다.

<br/>

<br/>

<br/>

<h3 id="title2">2. 블로킹(Blocking)과 논블로킹(Non-Blocking)</h3>

블로킹과 논블로킹은 호출된 함수의 **제어권**과 관련이 있다.

<br/>

##### **1) 블로킹(Blocking)**

<b style="color:#A27DF7">호출된 함수</b>가 자신의 작업이 종료될 때까지 **제어권**을 갖고 있는 것을 말한다. 즉, <b style="color:#A27DF7">호출된 함수</b>의 작업이 종료될 때까지 <b style="color:#EC6B66">호출하는 함수</b>는 다른 작업을 진행할 수 없다.

<br/>

##### **2) 논블로킹(Non-Blocking)**

반면, 논블로킹은 <b style="color:#A27DF7">호출된 함수</b>가 자신의 작업이 종료되지 않았더라도 함수의 제어권을 자신을 <b style="color:#EC6B66">호출하는 함수</b>로 바로 넘겨주는 것을 말한다.

<br/><br/>

<br/>

### 3. 예제 - 동기+블로킹, 동기+논블로킹, 비동기+블로킹, 비동기+논블로킹 

##### 1) 동기 + 블로킹 (Sync + Blocking)

동기+블로킹의 방식은 코드가 순서대로 실행되는 것을 생각하면 된다. A, B, C가 실행될 때 A가 실행되고 결과값을 반환하면 B가 실행되고, B의 작업이 완료되면 C가 실행되는 것이다.

```javascript
console.log("1");
console.log("2");
console.log("3");
```

<br/>

##### 2) 동기 + 논블로킹 (Sync + Non-Blocking)

 <b style="color:#A27DF7">호출된 함수</b>에서 작업을 완료하지 않았더라도 제어권을 바로 반납하여 다른 작업을 진행할 수 있도록 한다.

→  <b style="color:#EC6B66">호출한 함수</b>는 계속해서  <b style="color:#A27DF7">호출된 함수</b>의 작업이 끝났는지 확인

→ (작업이 미완료일 경우) <b style="color:#A27DF7">호출된 함수</b> 미완료 회신

→ **그동안 다른 작업 수행 가능**

→  <b style="color:#EC6B66">호출한 함수</b>가  <b style="color:#A27DF7">호출된 함수</b>의 작업이 끝났는지 확인

→ (작업이 완료되었을 경우)  <b style="color:#A27DF7">호출된 함수</b> 결과값 return

<br/>

##### **3) 비동기 + 블로킹 (Async + Blocking)**

 <b style="color:#A27DF7">호출된 함수</b>는 함수의 수행 결과를 콜백함수를 통해 전달한다. 하지만 논블로킹일 경우와 달리  <b style="color:#A27DF7">호출된 함수</b>가 제어권을 계속 가지고 있어 <b style="color:#A27DF7">호출된 함수</b>**의 작업이 완료되기 전까지 다른 작업을 진행할 수 없다.**

<br/>

##### **4) 비동기 + 논블로킹 (Async + Non-Blocking)**

비동기+논블로킹 개념은 자바스크립트의 대표적인 비동기함수인 `setTimeout()` 으로 설명해보려고 한다. `setTimeout()`  함수는 일정시간 이후 함수를 실행시키고 싶을 때 사용하는데, 콜백함수와 지연시간을 매개변수로 사용한다.

**비동기** - 여기서 `foo()`는 콜백함수로, 위의 비동기에서 설명했듯이 함수를 호출한 쪽에서 처리하지 않고, `setTimeout()`이 실행되고 나면 **콜백함수**를 통해서 알아서 결과가 처리된다.

**논블로킹** - 만약, 블로킹이라면 `setTimeout()`이 수행되고 3초 후 Async가 출력되고 그 다음에 Non-Blocking이 출력될 것이다. 하지만 실제 코드에선 `setTimeout()`이 수행되는 것을 기다리지 않고 바로 아래의 Non-Blocking 문구가 출력된다. <b style="color:#A27DF7"> 호출된 함수</b>가 제어권을 가지고 있지 않고 바로 제어권을 반납하여 전체 흐름에 영향을 주지 않고 다음 코드가 실행되었기 때문이다.

```javascript
function foo() {
  console.log("Async");
}
setTimeout(foo, 3000);	// 3초 뒤 실행
console.log("Non-Blocking");

// 출력결과
// Non-Blocking
// Async
```

<br/>

##### <br/><br/>

<h5 id="title2">참고 사이트</h5>

- https://siyoon210.tistory.com/147

  처음 이해할 때 참고했는데 설명이 진짜 쉽게 잘 되어있다 👍

- https://musma.github.io/2019/04/17/blocking-and-synchronous.html

- https://velog.io/@wonhee010/%EB%8F%99%EA%B8%B0vs%EB%B9%84%EB%8F%99%EA%B8%B0-feat.-blocking-vs-non-blocking

- https://stackoverflow.com/questions/748175/asynchronous-vs-synchronous-execution-what-does-it-really-mean

<br/><br/><br/><br/>