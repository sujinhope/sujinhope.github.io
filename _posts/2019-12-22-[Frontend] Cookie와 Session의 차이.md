#  Cookie와 Session의 차이

<br/>

우선 Cookie와 Session의 차이를 이해하기 위해선 Http통신의 특징부터 알아야 한다.

<br/>

<b style="color:#E94A1D">[🍂](https://apps.timwhitlock.info/emoji/tables/unicode#emoji-modal)&nbsp; HTTP 통신이란?</b>

***Hyper Text Transfer Protocol***. www 상에서 서버와 클라이언트가 정보(데이터)를 주고 받을 수 있는 프로토콜로, HTML문서를 주고 받는데 사용된다.

<br/>

**TCP방식과의 차이점**

- TCP 방식 : Client - Server 중 한 곳이 연결을 끊을 때까지 연결을 유지한다.

- HTTP 방식 : Client가 Server에서 HTML을 다운받고 나면 연결을 끊어버린다.

  => HTTP통신의 특징 - **비연결(Stateless)**

<br/>

<b style="color:#E94A1D">[🍂](https://apps.timwhitlock.info/emoji/tables/unicode#emoji-modal)&nbsp; HTTP 통신의 특징</b>

1. **Stateless**

   클라이언트와 서버가 연결을 성공한 후, 클라이언트의 요청에 대해 서버가 응답을 하고 나면 연결을 끊어버리는 것을 말한다.

   

   **장점**

   &nbsp; &nbsp; 통신 간의 연결 상태 처리나 정보의 저장을 관리할 필요가 없어서 서버 디자인이 간단해진다.

   **단점**

   &nbsp;&nbsp; 연결을 끊어버리기 때문에 서버에서 클라이언트의 현재 상태를 알 수 없다. 즉, 로그인을 이미 했더라도 연결을 끊었기 때문에 이후 작업에 대해 서버는 클라이언트가 로그인을 했었는지 안했었는지 알 수 없다.

   *이 문제를 위해 ==Cookie와 Session==으로 클라이언트의 상태를 저장해서 실제로 연결은 끊어졌지만 사용자에게 마치 연결이 되어 있는 것처럼 서비스를 제공한다.*

   <br/>

2. **Request, Response**

   **Request(요청)**

   웹 브라우저(Client)에서 웹 서버로 보내는 요청. 서버에게 데이터를 전송할 때 Request 객체에 담아 전달하게 된다.

   **Response(수신)**

   웹 서버에서 웹 브라우저(Client)로의 응답. 클라이언트로부터 서버에게 요청이 일어나고 나면 서버는 브라우저에 전달할 데이터를 Response 객체를 통해 전달한다.

<div align="center"><img src="https://user-images.githubusercontent.com/33229855/70770180-c2a07780-1daf-11ea-9170-8b8fad493fbb.png" style="align:center"/></div>



## Cookie와 Session의 차이

 위에서 언급한 HTTP 통신의 특징인 ***Stateless*** 때문에 서버는 클라이언트의 상태를 알지 못한다. 따라서 서버는 클라이언트의 요청이 있을 때마다 클라이언트의 상태를 계속해서 확인해야 하고, 클라이언트의 정보를 유지하기 위해서는 매번 연결을 해야 하는 번거로움이 있다. 

 이를 해결하기 위해 ***Cookie***와 ***Session***으로 클라이언트의 정보를 저장한다.

<br/>

**차이점**

- 쿠키와 세션의 역할은 비슷하다. 사용자의 정보를 저장하는 것이다.
- 가장 큰 차이점은 사용자의 정보가 저장되는 위치이다. ***Cookie***는 클라이언트 측에, ***Session***은 서버 측에 저장된다.
- 보안 면에서는 세션이 더 우수하고, 세션의 경우 서버의 처리가 필요하기 때문에 요청 속도는 쿠키가 더 빠르다.

<br/>

<b style="color:#E94A1D">[🍂](https://apps.timwhitlock.info/emoji/tables/unicode#emoji-modal)&nbsp;Cookie</b>

- 사용자의 디스크나 웹 브라우저 메모리에 저장된다.

- 클라이언트 측에 "key와 value"형태의 **text 타입**으로 데이터가 저장되며 데이터의 크기에 제한이 있다.

  

  **프로세스**

  1. 브라우저에서 웹 페이지 접속
  2. 웹 서버에서 쿠키를 생성.
  3. 생성한 쿠키에 데이터를 담아 요청에 응답할 때 클라이언트에게 함께 전송.
  4. 클라이언트가 보관하다가 서버에 재요청할 때 쿠키를 함께 전송.
  5. 클라이언트와 서버가 로그인 정보가 유지되어있는 것처럼 사용.

  <br/>

  **쿠키 유효기간 설정**

  ```java
  setMaxAge(int sec); //쿠키의 유효기간을 설정한다.
  
  //sec의 값에 따라 쿠키의 유효기간이 정해진다.
  음수(-1)	: 메모리에만 저장. 브라우저가 종료되면 사라진다.(일시적인 변수로 저장)
  0		 : 기존에 발행한 쿠키를 삭제한다.
  양수		: 지정한 초만큼 쿠키를 유지한다.
      	   단, 지정한 시간 전에 브라우저 종료 시 파일 형태로 저장됨.
  ```

  **쿠키 생성**(Java)

  ```java
  //쿠키 생성
  Cookie cookie = new Cookie(name, value); //String, String
  cookie.setMaxAge(1000000);
  
  //쿠키 발행
  response.addCookie(cookie);
  ```



<br/>

<b style="color:#E94A1D">[🍂](https://apps.timwhitlock.info/emoji/tables/unicode#emoji-modal) &nbsp;Session</b>

- 웹 서비스를 위한 사용자의 정보를 <span class="evidence">서버측</span>에 저장한다.

- 서버 측에 **객체 타입**으로 저장된다. *서버가 수용 가능한 만큼 저장할 수 있다.*

  **프로세스**

  1. 클라이언트가 서버에 접속 시 세션 ID를 발급
  2. 클라이언트는 쿠키(쿠키이름 : JSESSIONID)를 이용해 세션 ID를 저장해서 가지고 있음.
  3. 클라이언트가 서버에 재 요청 시 쿠키(JSESSIONID)를 이용하여 세션ID 값을 서버에 전달.

  <br/>

  **저장 기간** 

  &nbsp;&nbsp;session.invalidate() 혹은 웹 브라우저가 종료될 때까지 데이터가 유지된다.

  **세션 생성**(Java)
  
  ```java
  //세션 생성
  HttpSession session = request.getSession();
  
  //세션에 데이터 저장
  session.setAttribute("name", value);
  ```


<br/>





