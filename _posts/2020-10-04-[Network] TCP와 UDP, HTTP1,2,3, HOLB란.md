---
tags:
- Network
---



<ul>
	<li><a href="#1">HTTP</a></li>
	<li><a href="#2">TCP</a></li>
	<li><a href="#3">UDP</a></li>
	<li><a href="#4">HOLB란?</a></li>
</ul>

<br/><br/>

HTTP, TCP와 UDP, TCP의 연결/해제 방식인 3-way handshaking과 4-way handshaking에 대해서 알아본다.

<br/><br/>

<br/>

<h3 id="1">HTTP</h3>

- Hyper Text Transfer Protocol

- ##### HTTP/1

  - 한 번의 연결(Handshake)에 한 번의 요청만 처리

- ##### HTTP/2

  - `멀티플렉싱`을 사용하여 한 번의 연결(Handshake)에 여러 요청을 처리
  - Handshake 과정을 줄임으로써 `Latency`를 낮춤

- ##### HTTP/3

  - `TCP방식`을 사용하는 HTTP/1, HTTP/2와 달리 `UDP방식`을 사용
  - HTTP/2와 마찬가지로 `멀티플렉싱`을 지원한다.

<br/><br/><br/>

<h3 id="2">TCP</h3>

- 연결 지향
- `연속적인 데이터 전송의 신뢰성`을 보장한다.
- 이러한 과정을 위해 서버와 클라이언트가 연결을 하거나 해제할 때 `Handshake` 과정을 거친다.

**3-way handshake**

1. 요청자가 수신자에게 연결 요청을 위해 `시퀀스 넘버`를 `SYN 패킷`에 담아서 전송한다.
2. 수신자는 확인했다는 의미로 수신한 `시퀀스 넘버+1`을 `SYN`에 담아 보내고 새로 시퀀스 넘버를 생성해 `ACK`에 담아서 보낸다.
3. 송신자는 연결이 성립되었다고 판단하여 `ESTABLISHED`상태로 들어가고, 수신자에게 받은 `ACK`의 시퀀스 넘버에 1을 더한 값을 다시 승인 번호로 사용하여 보내준다.
4. 수신자가 제대로 받았을 경우 연결이 되었음을 판단하고 `ESTABLISHED`상태로 전환된다.

<img src="https://user-images.githubusercontent.com/33229855/95015981-10fa6c00-068b-11eb-9ce5-caf2d572c926.png" alt="image" style="zoom:80%;" />

<br/>

<br/>

##### 4-way handshake

한 쪽에서 일방적으로 연결을 끊어버릴 경우 다른 한 쪽은 연결이 끊어졌는지 알 방법이 없다. 또한 아직 처리하지 못한 데이터가 있을 수 있기 떄문에 양 쪽이 정상적으로 연결을 종료할 준비가 되었는지 확인해야 한다.

3-way-handshake와 달리 순차적으로 주고받는 것이 아니라 **상대방이 응답을 줄 때까지 대기**하는 과정이 포함된다. 따라서 중간에 잘못된다면 계속 대기하고 있는 `Deadlock` 상황이 연출될 수 있다.

1. 연결을 종료하고자 하는 **요청자**가 `FIN`패킷을 보내며 `FIN_WAIT_1`상태로 들어선다.

   이 때 `FIN`패킷에 담아 보내는 시퀀스 넘버는 새로 생성하는 것이 아니라 계속 사용하던 시퀀스 넘버에 맞춰 다음 넘버를 전송한다.

2. 수신자는 우선 확인했다는 뜻의 `ACK` 패킷을 보내고 `TIME_WAIT` 상태로 변경 후 자신의 통신이 끝날 때까지 기다린다.

   `ACK`를 받은 요청자는 `FIN_WAIT_2` 상태가 된다.

3. 수신자의 통신이 끝났으면 연결이 종료되었다는 뜻의 `FIN` 패킷을 전송하고 `LAST_ACK` 상태가 된다.

4. 요청자는 `FIN` 패킷을 받고나면 `ACK` 패킷을 전송하고 한동안 `TIME_WAIT` 상태를 유지한다.

   **수신자에서 FIN 패킷을 전송하기 전에 전송한 패킷이 Routing 지연이나 패킷 유실로 인한 재전송 등으로 인해 FIN 패킷보다 늦게 도착하는 상황을 위해 FIN을 수신하더라도 일정시간(Default:240초) 동안 세션을 남겨놓고 기다린다.**

5. 수신자는 `ACK` 패킷을 받고 `CLOSED`상태가 되며 연결을 종료한다.

`FIN_WAIT_1`, `FIN_WAIT_2` 상태는 TIME_OUT이 되면 자동으로 `CLOSED`된다.

<img src="https://user-images.githubusercontent.com/33229855/95016153-199f7200-068c-11eb-8f16-808a095c9a53.png" alt="image" style="zoom:80%;" />

<br/><br/><br/>

<h3 id="3">UDP</h3>

- User Datagram Protocol

- 데이터그램 방식을 사용하며 패킷의 목적지가 정해져있다면 중간 경로는 어딜타든 신경쓰지 않기 때문에 Handshake과정을 하지 않는다.

  즉, TCP방식에 비해 Latency가 굉장히 적지만 신뢰성을 보장하기 어려움

<br/><br/>

##### QUIC

- TCP가 가지고 있는 문제들을 해결하고 Latency의 한계를 뛰어넘고자 구글이 개발한 UDP기반 프로토콜

- TCP 핸드쉐이크 과정을 최적화하는 것에 초점을 맞춰서 설계되었다.

- TCP의 경우 IP주소와 포트, 연결 대상의 IP주소와 포트로 연결을 식별하기 때문에 클라이언트의 IP가 바뀌는 상황이 발생할 경우 연결이 끊어진다. 이런 경우 다시 3-way-handshake 과정을 거치고, Latency가 발생한다. 모바일 환경의 경우 Wifi와 셀룰러 간 전환이 일어날 경우 클라이언트의 IP가 변경되기 때문에 문제가 될 수 있다.

  QUIC은 클라이언트의 IP와는 상관없이 랜덤하게 생성되는 `Connection ID`를 사용하기 때문에 클라이언트의 IP가 변경되더라도 기존의 연결을 계속 유지할 수 있다.

  즉, Handshake 과정을 생략함으로써 `Latency`를 줄일 수 있다.

<br/><br/>

<br/>

<h3 id="4">HOLB</h3>

- Head Of Line Blocking
- 어떤 요청에 병목이 생겨서 전체 latency가 증가하는 것
- TCP를 사용하는 통신에서 패킷은 무조건 정확한 순서대로 처리되어야 한다. 수신 측은 송신 측과 주고받은 시퀀스 번호를 참고하여 패킷을 재조립 해야하기 때문.
- 따라서 중간에 패킷이 손실될 경우 완전한 데이터로 재조립하기 어렵기 때문에 송신 측은 해당 패킷이 제대로 전달되지 않았을 경우 재전송 해야 한다.
- 패킷이 처리되는 순서도 정해져있기 때문에 이전에 받은 패킷을 파싱하기 전까지 다음 패킷을 처리할 수도 없다. 패킷이 중간에 유실되거나 수신 측의 파싱 속도가 느릴 때 통신에 병목이 발생하게 되는 현상을 **HOLB**라고 부른다.
- TCP 자체의 문제이기 때문에 HTTP/1 과 HTTP/2 모두 해당 문제를 가지고 있다. HTTP/3는 UDP를 기반으로 만들어진 프로토콜 QUIC 위에서 동작하며 다른 방법으로 신뢰성을 확보한다.



<br/><br/><br/>