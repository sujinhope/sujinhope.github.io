

##### 참고 링크

- 아래 링크에서 설명해 준 내용을 요약해서 정리해보았다.

```url
https://nerogarret.tistory.com/47?category=800142
```

<br/>

<br/>

##### TIP

```bash
# ubuntu 명령어
# 참고 - https://velog.io/@devyang97/Linux-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC-Ubuntu-%EC%82%AC%EC%9A%A9

$ pwd		# 현재 디렉토리 경로 확인
$ ls		# 현재 디렉토리 내 폴더, 파일 리스트 출력
			# option) -a, -l, -al, -R, ...
$ ls [특정폴더명]  # 특정 폴더 내 폴더, 파일 리스트 출력
$ mkdir		# 디렉토리 생성
$ vi [파일명]	# 파일 편집
$ cat [파일명] # 파일 내용 출력

$ cd [디렉토리명] # 디렉토리로 이동
$ cd .. 		# 상위 디렉토리로 이동
$ cd			# 루트 디렉토리로 이동
```



<br/><br/>

### RUNSERVER

##### 1. EC2 접속

```bash
$ ssh -i J3A401T.pem ubuntu@j3a401.p.ssafy.io
```

<br/>

##### 2. python3-venv 설치, 프로젝트 clone

```bash
$ sudo apt-get install python3-venv
$ git clone 프로젝트 주소
```

![image](https://user-images.githubusercontent.com/33229855/93755079-acc4ba80-fc3d-11ea-87f1-7fc060e165c6.png)

<br/>

##### 3. 가상환경 생성

```bash
ubuntu: ~$ cd ~
ubuntu: ~$ python3 -m venv myvenv
ubuntu: ~$ ls	# myvenv 생성되었는지 확인
```

![image](https://user-images.githubusercontent.com/33229855/93755312-05945300-fc3e-11ea-91e1-1158a58bd4e9.png)

<br/>

##### 4. 가상환경 활성화

```bash
ubuntu: ~$ source myvenv/bin/activate
# (myvenv) ubuntu: ~$  ---- activate 후에 앞에 (myvenv) 생김
```

<br/>

##### 5. 프로젝트 폴더로 이동, requirements.txt를 통해 패키지 설치

```bash
(myvenv) ubuntu: ~$ cd test_aws/sub2/backend
# manage.py가 있는 위치로 이동(ls 명령어로 확인)
```

![image-20200921191251233](C:\Users\multicampus\AppData\Roaming\Typora\typora-user-images\image-20200921191251233.png)

```bash
(myvenv) ubuntu: ~$ pip3 install -r requirements.txt
```

<br/>

##### 6. runserver

포트는 본인이 설정

```bash
(myvenv) ubuntu: ~$ python3 manage.py runserver 0:8000
```

<br/>

도메인주소와 포트(8000)을 이용해서 브라우저 접속 확인(Error -> **7**에서 확인)

```
https://j3a401.p.ssafy.io:8000
```

<br/>

##### 7. allowed host 설정

프로젝트 폴더 내 settings.py에서 allowed host 설정

```bash
# settings.py가 있는 위치로 이동
(myvenv) ubuntu: $ cd backend		# backend 대신 본인 프로젝트명 사용
(myvenv) ubuntu: $ vi settings.py	# settings.py 편집
```

<br/>

```bash
# ALLOWED_HOSTS = ['127.0.0.1', 'localhost', 'j3a401.p.ssafy.io'] 추가
```

![image](https://user-images.githubusercontent.com/33229855/93756273-c6670180-fc3f-11ea-9753-f7e316aee822.png)

<br/>

##### 8. runserver

```bash
(myvenv) ubuntu: $ python3 manage.py runserver 0:8000
# https://j3a401.p.ssafy.io 접속 후 확인
```

<br/><br/><br/><br/>



### uWSGI 서버 연결하기

##### 1. uwsgi 패키지 설치

```bash
$ source ~/myvenv/bin/activate  # 가상환경 활성화(이미 되어있다면 패스)
$ pip3 install uwsgi
```

<br/>

##### 2. uwsgi 서버를 이용해서 Django 프로젝트 연결

양식

```bash
$ uwsgi --http :[포트번호] --home [가상환경 경로] --chdir [Django프로젝트 경로] -w [wsgi 모듈이 있는 폴더].wsgi
```

예제

```bash
$ uwsgi --http :8000 --home /home/ubuntu/myvenv/ --chdir /home/ubuntu/test_aws/sub2/backend -w /home/ubuntu/test_aws/sub2/backend/backend.wsgi
```

<br/>

- **가상환경 경로** : 위에서 ~에 myvenv를 설치했으므로 ~/myvenv/ 이지만 **절대경로**로 지정해주는 것이 좋음.

  `/home/ubuntu/myvenv/`

- **장고 프로젝트 폴더 경로** : manage.py가 있는 경로. 마찬가지로 **절대경로**로 지정

  `/home/ubuntu/test_aws/sub2/backend`

- **wsgi 모듈이 있는 폴더** : 위 프로젝트 폴더 내에 backend(==각자 프로젝트마다 다름==) 안에 wsgi.py가 있는 **폴더명** 뒤에 .wsgi 추가

  `/home/ubuntu/test_aws/sub2/backend/backend.wsgi`

  ![image](https://user-images.githubusercontent.com/33229855/93757308-7b4dee00-fc41-11ea-9126-649c323926db.png)

<br/>

==서버 배포 완료==

![image](https://user-images.githubusercontent.com/33229855/93757764-44c4a300-fc42-11ea-9ead-5fed12b1c237.png)



<br/><br/><br/><br/><br/>



































