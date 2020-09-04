---
layout: article
title: REST, Restful
article_header:
  type: overlay
  theme: dark
  background_image:
    src: /docs/assets/images/background1.jpg

tags:
- Web
- Backend
- Network
---





<br/>





**목차**

1. <a href="#title1">RESTful</a>
2. <a href="#title2">REST(REpresentational State Transfer)</a>
   1. <a href="#subtitle1">장단점</a>
   2. <a href="#subtitle2">필요한 이유</a>
   3. <a href="#subtitle3">구성요소</a>
   4. <a href="#subtitle4">특징</a>

<br/><br/>

<h3 id="title1">RESTful</h3>

- REST API를 제공하는 웹 서비스를 RESTful하다고 한다.

- REST 원리를 따르는 시스템.

  <br/>

  <br/>
  
  <br/>

<h3 id="#title2">Rest(REpresentational State Transfer)</h3>

- 자원을 이름으로 구분하여 해당 자원의 정보를 주고받는 모든 것.

  ex - DB의 학생정보가 자원일 경우 'students'를 자원의 표현(이름)으로 정함.

  웹 사이트의 이미지, 텍스트, DB 등 모든 자원에 고유한 ID인 HTTP URI를 부여함.

- 데이터가 요청되어 지는 시점에 JSON이나 XML을 통해 데이터를 주고 받는다.

- 네트워크 상에서 Client와 Server 사이의 통신 방식 중 하나.

- **HTTP URI를 통해 자원을 명시하고 Post, Get, Put, Delete, Head와 같은 HTTP Method를 통해 해당 자원에 대한 CRUD를 적용하는 것.**

<br/>

<h5 id="subtitle1">장단점</h5>

- 장점
  - HTTP 프로토콜 인프라를 그대로 사용. HTTP 표준 프로토콜을 따르는 모든 플랫폼엥서 사용 가능.
  - 서버와 클라이언트의 역할을 명확하게 분리한다.
  - REST API 메시지가 의도하는 바를 명확하게 나타내기 때문에 의도를 쉽게 파악할 수 있음.

- 단점
  - 표준이 존재하지 않음.
  - 사용할 수 있는 메서드가 4가지 뿐(형태가 제한적임)
  - 구형 브라우저가 제대로 지원해주지 못하는 부분들이 있음.(PUT, DELETE 사용 불가 등)

<br/>

<h5 id="subtitle2">필요한 이유</h5>

- 다양한 클라이언트의 등장 - 요즘은 다양한 브라우저와 안드로이드, 아이폰과 같은 모바일 디바이스에서도 통신이 가능해야 한다.

  <br/>

<h5 id="subtitle3">구성요소</h5>

- URI(자원)
- HTTP Method(행위)
- 표현

<br/>



<h5 id="subtitle4">특징</h5>

- Server-Client 구조

  자원이 있는 쪽이 Server, 요청하는 쪽이 Client. 

  - Server : API를 제공하고 비즈니스 로직 처리 및 저장을 책임진다.

  - Client : 사용자 인증 or context(세션, 로긍니 정보) 등을 직접 관리 및 책임.

- Stateless

  - Client의 context를 Server에 저장하지 않음. 구현이 단순하다.

  - 각 요청을 별개로 인식하고 처리. 이전 요청이 다음 요청의 처리에 연관되지 않는다. 

    => 서비스의 자유도가 높아짐.

- Cacheable

  - HTTP의 특징인 캐싱 기능 적용 가능.(Last-Modified 태그나 E-Tag를 이용)
  - 응답시간이 빨라지고 REST Server 트랜잭션이 발생하지 않기 때문에 전체 응답시간, 성능, 서버 자원 이용률 향상

- Layered System(계층화)

  - Client는 REST API Server만 호출하는데 REST Server는 다중 계층으로 구성 가능.
  - 로드밸런싱, 공유 캐시 등을 통해 구조상의 유연성을 줄 수 있고 확장성과 보안성 향상 가능

<br/><br/>