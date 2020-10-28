---
tags:
- Network
---



<ul>
	<li><a href="#2">공격을 막는 방법</a></li>
</ul>

<br/><br/>

- TCP의 3-way handshake 구조적 약점을 이용하는 공격

- 소스 IP를 존재하지 않는 출발지 IP주소로 위조(Spoofing)한 후 서버의 특정 포트에 대량의 SYN packet을 발송하고 연결을 완료하지 않아서 **백로그 큐**를 꽉차게 함으로써 더 이상의 새로운 연결 요청을 받을 수 없도록 함

  > Backlog Queue = Incomplete Queue + Complete Queue

  <br/>

- 원래는 SYN+ACK 응답 후 Incomplete Connection Queue에 연결 요청정보를 저장하고 이후 클라이언트가 ACK응답을 보내면 Incomplete Connection queue 에 있던 연결 요청 정보를 Completed Connection Queue로 이동하여 Accept() 시스템 콜을 통해 연결소켓이 생성되면서 연결 요청정보가 삭제되며 최종 연결이 완료

![image](https://user-images.githubusercontent.com/33229855/97392520-4b22fa80-1925-11eb-8fe7-9332a683beb0.png)

<br/><br/><br/>

<h3 id="2">공격을 막는 방법</h3>

1. 백로그 큐의 사이즈를 늘린다.
   - 백로그 사이즈를 늘려도 그 이상으로 공격을 보낼 경우 문제를 피할 수 없기 때문에 임시적인 방법일 뿐

2. tcp_syncookies 값을 1로 설정

3. 동일 Client(IP)의 연결(SYN) 요청에 대한 임계치를 설정해 과도한 연결 요청이 발생하는 것을 차단

   ex) IP테이블 설정 1초에 10회 발생 시 차단

4. First SYN Drop

   연결 요청 패킷을 보내는 클라이언트가 실제로 존재하는지를 파악. 첫 번째 SYN을 Drop하여 재요청 패킷이 도착하는지 확인함으로써 출발지 IP가 Spoofing 되었는지를 판단한다.

<br/><br/>