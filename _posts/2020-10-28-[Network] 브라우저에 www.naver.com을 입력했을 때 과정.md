---
tags:
- Network
---





 기술 면접 단골 질문(특히 라인플러스) "브라우저에 URL을 입력했을 때 어떤 일이 일어나는가?"에 대한 과정을 살펴보겠습니다.

<br/><br/>

1. 브라우저에 www.naver.com 을 입력한다.

2. 브라우저의 URL 파싱

   어떤 프로토콜, ULR, 포트로 보낼 지 해석

   기본값 : HTTP(80), HTTPS(443)

   ![image](https://user-images.githubusercontent.com/33229855/97395920-5bd56f80-1929-11eb-8aa3-06473f7b4516.png)

3. DNS Resolver가 도메인 주소를 IP 주소로 변환하는 과정을 거친다.

   3-1. Local DNS에 해당 URL주소의 IP 주소를 요청

   3-2. 없을 경우 root DNS 서버에 해당 URL의 IP 주소를 요청

   3-3. root DNS에도 주소가 없을 경우 `.com` 도메인을 관리하는 TLD DNS 서버에 요청

   3-4. `naver.com`의 IP 주소를 관리하는 하위 DNS서버에 요청

   3-5. Local DNS에서 응답받은 IP 주소를 캐싱

4. 라우터를 통하여 해당 서버의 게이트웨이까지 이동

5. ARP를 이용하여 IP 주소를 MAC 주소로 변환

6. TCP handshaking 과정을 통해 TCP 소켓 연결 수립한다.

7. 브라우저가 Request Message 전송한다.

8. 서버가 URL을 프로그램이나 정적 파일로 연결한다.

9. 서버가 Response Message 전송한다.

10. 브라우저가 응답을 형식에 맞춰서 화면에 표시한다.

<br/><br/>